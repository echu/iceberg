# Iceberg

This repository contains a single shell script that picks a randomly running container and executes `docker kill` against it.

## Dependencies
This uses the GNU coreutils `shuf` command to pick a random running container.
