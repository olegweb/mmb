# MMB [![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=Set%20of%20Dockerfiles%20and%20assets%20related%20to%20them%20for%20building%20Docker%20images%20with%20different%20services&url=https://github.com/tolstoyevsky/mmb&via=CusDeb&hashtags=RaspberryPi,Docker,Alpine,Debian)

<p align="center">
    <img src="/logo/382x400.png" alt="MMB">
</p>

MMB is the set of Dockerfiles and assets for building Docker images with different services (such as [Nextcloud](https://nextcloud.com), [Transmission](https://transmissionbt.com), etc.). It supports both x86_64 (servers and workstations) and ARM (Raspberry Pis and other single board computers) as target platforms.

MMB stands for "Mr. Meeseeks Box". The project name was inspired by the episode ["Meeseeks and Destroy"](https://en.wikipedia.org/wiki/Meeseeks_and_Destroy) of an animated sitcom [Rick and Morty](https://en.wikipedia.org/wiki/Rick_and_Morty).

## Getting started

This instructions will help you build and run services from the project.

### Prerequisites

You will need Docker and docker-compose (>= 1.10.0).

### Build

Assuming you would like to build a Docker image with MariaDB. Execute

```
$ sudo ./mmb.sh mariadb amd64
```

or 

```
$ sudo ./mmb.sh mariadb
```

to build the Docker image for `amd64` port. If the target platform is based on ARM, substitute `amd64` for `armhf`. For example,

```
$ sudo ./mmb.sh mariadb armhf
```

By the way, you can avoid using the `mmb.sh` script and build the image in a little more challenging way. Execute

```
$ cd mariadb
$ IMAGE_NAME="$(grep "image: " docker-compose.yml | awk -F': ' '{print $2}')"
$ docker build -t "${IMAGE_NAME}" .
$ if [ -f postinst.sh ]; then ./postinst.sh; fi
```

to build the Docker image for `amd64` port. If the target platform is based on ARM, execute

```
$ cd mariadb
$ IMAGE_NAME="$(grep "image: " docker-compose-armhf.yml | awk -F': ' '{print $2}')"
$ cp Dockerfile Dockerfile-armhf
$ sed -i -e "s/amd64$/armhf/" Dockerfile-armhf
$ docker build -t "${IMAGE_NAME}" -f Dockerfile-armhf .
$ if [ -f postinst.sh ]; then ./postinst.sh; fi
```

### Run

To run the container, go to the directory of the target service and execute

```
$ docker-compose up -d
```

or

```
$ docker-compose -f docker-compose-armhf.yml up -d
```

to run a Docker container for `amd64` port.

Make sure that the container is in the RUNNING state, executing

```
$ docker ps
```

If the container is not present in the list, execute `docker-compose` without the `-d` option to figure out the reason.

## Authors

See [AUTHORS](AUTHORS.md).

## Licensing

MMB is available under the [Apache License, Version 2.0](LICENSE).
