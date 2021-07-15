# Amazon Linux for Docker

A small Amazon Linux base image designed for use in containers.

All non-required packages were removed to create this small image. When
using this image you may have to install some of the packages that
usually are installed on a regular Amazon Linux image.

## Supported tags

* `8`, `latest`

## What is Amazon Linux ?

> Amazon Linux 2 is the next generation of Amazon Linux, a Linux server operating system from Amazon Web Services (AWS). It provides a secure, stable, and high performance execution environment to develop and run cloud and enterprise applications. With Amazon Linux 2, you get an application environment that offers long term support with access to the latest innovations in the Linux ecosystem.

*from* [aws.amazon.com](https://aws.amazon.com/amazon-linux-2/)

## Getting Started

There are a couple of things needed for the script to work.

### Prerequisites

Docker, either the Community Edition (CE) or Enterprise Edition (EE), needs to
be installed on your local computer.

#### Docker

Docker installation instructions can be found
[here](https://docs.docker.com/install/).

### Usage

To start a container with this image and run a shell use the following
command (the container will be deleted after exiting the shell):

```shell
docker container run --rm --interactive --tty fscm/amazonlinux bash
```

## Build

Build instructions can be found
[here](https://github.com/fscm/docker-amazonlinux/blob/master/README.build.md).

## Versioning

This project uses [SemVer](http://semver.org/) for versioning. For the versions
available, see the [tags on this repository](https://github.com/fscm/docker-amazonlinux/tags).

## Authors

* **Frederico Martins** - [fscm](https://github.com/fscm)

See also the list of [contributors](https://github.com/fscm/docker-amazonlinux/contributors)
who participated in this project.
