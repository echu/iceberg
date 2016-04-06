# Iceberg

In the land of Docker containers, sometimes I find it useful to kill a
couple and see how errors propagate through the fragile cluster of
containers I've linked together.

This repository contains a shell script that picks a randomly running
container and executes `docker kill` against it.

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

## Use cases

1. Ensure that services hosted on containers are robust to failure.
2. Identify single points of failure in a distributed system dependent
on containers.
3. Poor man's chaos monkey (`cron` job with `iceberg`).
4. Verify that service discovery works on container restarts.
5. Other things I haven't thought of.
