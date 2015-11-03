# Deprecated

This fork is officially deprecated in favor of [Parallels Driver for Docker
Machine](https://github.com/Parallels/docker-machine-parallels/), which is
compatible with Docker Machine 0.5.0+

The code and documentation here only exist as a reference for users who have not
yet switched over and want to use Docker Machine v0.4.0 with Parallels Desktop
for Mac.

# Docker Machine + Parallels driver

This is a fork of [Docker Machine](https://github.com/docker/machine) repository
with added support of Parallels Desktop for Mac.

## Requirements
* OS X 10.9 of later
* [Parallels Desktop 11](http://www.parallels.com/products/desktop/) **Pro** or
**Business** edition.

## Installation

In order to use Docker Machine with Parallels Desktop you should install our
custom `docker-machine` binary:

```console
$ curl -L https://github.com/Parallels/docker-machine/releases/download/parallels%2F0.4.0-1/docker-machine_darwin-amd64 > /usr/local/bin/docker-machine

$ chmod +x /usr/local/bin/docker-machine
```

**NB!** This is a replacement for the official `docker-machine` binary. If you
will install or update "Docker Toolbox" in the future, it could override this
binary and you might want to re-download it again.

## Usage
Official documentation for Docker Machine [is available here](https://docs.docker.com/machine/).

To create a Parallels Desktop virtual machine for Docker purposes just run this
command:

```
$ docker-machine create --driver=parallels prl-dev
```

Available options:

 - `--parallels-boot2docker-url`: The URL of the boot2docker image.
 - `--parallels-disk-size`: Size of disk for the host VM (in MB).
 - `--parallels-memory`: Size of memory for the host VM (in MB).
 - `--parallels-cpu-count`: Number of CPUs to use to create the VM (-1 to use the number of CPUs available).

The `--parallels-boot2docker-url` flag takes a few different forms. By
default, if no value is specified for this flag, Machine will check locally for
a boot2docker ISO. If one is found, that will be used as the ISO for the
created machine. If one is not found, the latest ISO release available on
[boot2docker/boot2docker](https://github.com/boot2docker/boot2docker) will be
downloaded and stored locally for future use. Note that this means you must run
`docker-machine upgrade` deliberately on a machine if you wish to update the "cached"
boot2docker ISO.

This is the default behavior (when `--parallels-boot2docker-url=""`), but the
option also supports specifying ISOs by the `http://` and `file://` protocols.

Environment variables and default values:

| CLI option                    | Environment variable        | Default                  |
|-------------------------------|-----------------------------|--------------------------|
| `--parallels-boot2docker-url` | `PARALLELS_BOOT2DOCKER_URL` | *Latest boot2docker url* |
| `--parallels-cpu-count`       | `PARALLELS_CPU_COUNT`       | `1`                      |
| `--parallels-disk-size`       | `PARALLELS_DISK_SIZE`       | `20000`                  |
| `--parallels-memory`          | `PARALLELS_MEMORY_SIZE`     | `1024`                   |


### Example:

_Docker Client is required to run Docker containers. It is bundled to
[Docker Toolbox](https://www.docker.com/toolbox)_

```console
$ docker-machine create -d parallels prl-dev
Creating CA: /Users/legal/.docker/machine/certs/ca.pem
Creating client certificate: /Users/legal/.docker/machine/certs/cert.pem
Image cache does not exist, creating it at /Users/legal/.docker/machine/cache...
No default boot2docker iso found locally, downloading the latest release...
Downloading https://github.com/boot2docker/boot2docker/releases/download/v1.8.1/boot2docker.iso to /Users/legal/.docker/machine/cache/boot2docker.iso...
Creating SSH key...
Creating Parallels Desktop VM...
Starting Parallels Desktop VM...
Waiting for VM to come online...
To see how to connect Docker to this machine, run: docker-machine env prl-dev

$ docker-machine ls
NAME       ACTIVE   DRIVER       STATE     URL                         SWARM
prl-dev    *        parallels    Running   tcp://10.211.55.3:2376

$ eval "$(docker-machine env prl-dev)"

$ docker run busybox echo hello world
Unable to find image 'busybox:latest' locally
latest: Pulling from library/busybox
cf2616975b4a: Pull complete
6ce2e90b0bc7: Pull complete
8c2e06607696: Pull complete
Digest: sha256:df9e13f36d2d5b30c16bfbf2a6110c45ebed0bfa1ea42d357651bc6c736d5322
Status: Downloaded newer image for busybox:latest
hello world
