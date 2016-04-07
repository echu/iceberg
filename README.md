# Iceberg

In the land of Docker containers, sometimes I find it useful to kill a
couple and see how errors propagate through the fragile cluster of
containers I've linked together.

This repository contains a shell script that picks a randomly running
container and executes `docker kill` against it. Note that this behavior
is distinct from a container that crashes: `docker kill` issues SIGKILL
against the container and potentially gives it a chance to cleanup
(depending on what's running on PID 1). Nonetheless, the effect  is 
equivalent: a previously running container has disappeared (along with 
any volumes, ports, services, etc. it may have been hosting).

By making (potentially catastrophic) low-probability events occur, we
can build more robust systems that rely on Docker containers. The
alternative is to sink a monolith on an iceberg.

## Dependencies
This uses the GNU coreutils `shuf` command to pick a random running 
container. It should work on Linux systems with no modification, but
on OSX will require

```
brew install coreutils
```

Windows doesn't run shell scripts, yet.

## What's this `grep -v $HOSTNAME`?
In the event that the script is run inside a Docker container
communicating with the host daemon, it is possible to kill yourself. I
suppose a suicidal iceberg is nothing to be afraid of, but `grep -v
$HOSTNAME` uses the container hostname (usually its ID or name) to
filter our itself from the list of targets.

Note that if you set `--hostname` when running a container containing
this script then all bets are off and self-imploding icebergs are a
distinct possibility.

## Use cases

1. Copy the script into a docker container and run it in production, e.g.,

    ```
    docker run \
	-v /var/run/docker.sock:/var/run/docker.sock \
	-v $PWD:/iceberg alpine \
	/bin/sh iceberg/iceberg.linux
    ```
    The script as written above won't run, since alpine doesn't come with
    Docker installed, but that's a minor detail. Also, I don't really mean
    that you should run this in production unless you're positive.

1. Ensure that services hosted on containers are robust to failure.
1. Identify single points of failure in a distributed system dependent
on containers.
1. Poor man's chaos monkey (`cron` job with `iceberg`).
1. Verify that service discovery works on container restarts.
1. Verify that persistent disks are handled robustly, especially for
databases in containers.
1. Other things I haven't thought of.

## Changelog

### Version 0.1.0

- Uniformly at random kills Docker containers
- Can be run inside a container
- Initial release
