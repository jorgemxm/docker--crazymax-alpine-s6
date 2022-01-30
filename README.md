<p align="center">
  <a href="https://github.com/crazy-max/docker-alpine-s6/actions?workflow=build"><img src="https://img.shields.io/github/workflow/status/crazy-max/docker-alpine-s6/build?label=build&logo=github&style=flat-square" alt="Build Status"></a>
  <a href="https://hub.docker.com/r/crazymax/alpine-s6/"><img src="https://img.shields.io/docker/stars/crazymax/alpine-s6.svg?style=flat-square&logo=docker" alt="Docker Stars"></a>
  <a href="https://hub.docker.com/r/crazymax/alpine-s6/"><img src="https://img.shields.io/docker/pulls/crazymax/alpine-s6.svg?style=flat-square&logo=docker" alt="Docker Pulls"></a>
  <br /><a href="https://github.com/sponsors/crazy-max"><img src="https://img.shields.io/badge/sponsor-crazy--max-181717.svg?logo=github&style=flat-square" alt="Become a sponsor"></a>
  <a href="https://www.paypal.me/crazyws"><img src="https://img.shields.io/badge/donate-paypal-00457c.svg?logo=paypal&style=flat-square" alt="Donate Paypal"></a>
</p>

## About

Alpine Linux with [s6 overlay](https://github.com/just-containers/s6-overlay/).<br />
If you are interested, [check out](https://hub.docker.com/r/crazymax/) my other Docker images!

💡 Want to be notified of new releases? Check out 🔔 [Diun (Docker Image Update Notifier)](https://github.com/crazy-max/diun) project!

___

* [Features](#features)
* [Usage](#usage)
* [Alpine image](#alpine-image)
* [Dist image](#dist-image)
* [Supported tags](#supported-tags)
* [Build](#build)
* [Contributing](#contributing)
* [License](#license)

## Features

* Multi-platform [alpine based](#alpine-image) and [distribution](#dist-image) images
* Artifacts provided on [releases page](https://github.com/crazy-max/docker-alpine-s6/releases)

## Usage

This repository provides two images. The first one is built on top of alpine
so, you can use it as a base image for your own images:

```dockerfile
FROM crazymax/alpine-s6:3.15
RUN apk add --no-cache nginx
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
CMD ["/usr/sbin/nginx"]
```

> Note: `ENTRYPOINT ["/init"]` is already defined in the base image so no need
> to add this command.

The second one is a [distribution image](#dist-image). This is a
multi-platform scratch image that only contains all the scripts and binaries
needed to run s6-overlay. This way you can use any base image and use the
`COPY --from` command to copy the assets inside your image:

```dockerfile
FROM ubuntu
COPY --from=crazymax/alpine-s6-dist:3.15 / /
RUN apt-get update && apt-get install -y nginx
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
CMD ["/usr/sbin/nginx"]
ENTRYPOINT ["/init"]
```

## Alpine image

| Registry                                                                                         | Image                           |
|--------------------------------------------------------------------------------------------------|---------------------------------|
| [Docker Hub](https://hub.docker.com/r/crazymax/alpine-s6/)                                            | `crazymax/alpine-s6`                 |
| [GitHub Container Registry](https://github.com/users/crazy-max/packages/container/package/alpine-s6)  | `ghcr.io/crazy-max/alpine-s6`        |

```
$ docker run --rm mplatform/mquery crazymax/alpine-s6:latest
Image: crazymax/alpine-s6:latest
 * Manifest List: Yes
 * Supported platforms:
   - linux/amd64
   - linux/arm/v6
   - linux/arm/v7
   - linux/arm64
   - linux/386
   - linux/ppc64le
   - linux/s390x
```

## Dist image

| Registry                                                                                         | Image                           |
|--------------------------------------------------------------------------------------------------|---------------------------------|
| [Docker Hub](https://hub.docker.com/r/crazymax/alpine-s6-dist/)                                            | `crazymax/alpine-s6-dist`                 |
| [GitHub Container Registry](https://github.com/users/crazy-max/packages/container/package/alpine-s6-dist)  | `ghcr.io/crazy-max/alpine-s6-dist`        |

```
$ docker run --rm mplatform/mquery crazymax/alpine-s6-dist:latest
Image: crazymax/alpine-s6-dist:latest
 * Manifest List: Yes
 * Supported platforms:
   - linux/amd64
   - linux/arm/v6
   - linux/arm/v7
   - linux/arm64
   - linux/386
   - linux/ppc64le
   - linux/s390x
```

## Supported tags

* `edge`, `edge-x.x.x.x`
* `latest-edge`, `3.15-edge`
* `latest`, `latest-x.x.x.x`, `3.15`, `3.15-x.x.x.x`
* `3.14-edge`
* `3.14`, `3.14-x.x.x.x`
* `3.13-edge`
* `3.13`, `3.13-x.x.x.x`
* `3.12-edge`
* `3.12`, `3.12-x.x.x.x`

> `x.x.x.x` has to be replaced with one of the s6-overlay releases available (e.g. `2.2.0.3`).

## Build

```shell
git clone https://github.com/crazy-max/docker-alpine-s6.git
cd docker-alpine-s6

# Build image and output to docker (default)
docker buildx bake

# Build tarballs to ./dist
docker buildx bake artifact-all

# Build multi-platform image
docker buildx bake image-all

# Build multi-platform dist image
docker buildx bake image-dist-all
```

## Contributing

Want to contribute? Awesome! The most basic way to show your support is to star the project, or to raise issues. You
can also support this project by [**becoming a sponsor on GitHub**](https://github.com/sponsors/crazy-max) or by making
a [Paypal donation](https://www.paypal.me/crazyws) to ensure this journey continues indefinitely!

Thanks again for your support, it is much appreciated! :pray:

## License

MIT. See `LICENSE` for more details.
