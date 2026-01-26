# Baikal

This repository is a fork of [ckulka/baikal-docker](https://github.com/ckulka/baikal-docker/) who has made an amazing job providing this image and work to build it.

There are some variants from the original repository. Default images are the `*-nginx`.

[![Latest images](https://github.com/aalmenar/baikal-docker/actions/workflows/build-latest.yaml/badge.svg)](https://github.com/aalmenar/baikal-docker/actions/workflows/build-latest.yaml) [![Experimental images](https://github.com/aalmenar/baikal-docker/actions/workflows/build-experimental.yaml/badge.svg)](https://github.com/aalmenar/baikal-docker/actions/workflows/build-experimental.yaml) ![Docker Architectures](https://img.shields.io/badge/arch-amd64%20%7C%20arm32v7%20%7C%20arm64v8-informational)

These dockerfiles provide a ready-to-go [Baikal server](http://sabre.io/baikal/).

## Supported tags and respective Dockerfile links

Tags without a version are [weekly re-builds](https://github.com/aalmenar/baikal-docker/actions/workflows/build-latest.yaml) to include the latest base image with the most recent updates:

From now on latest images will be the nginx version.

- `latest` and `nginx` are re-builds of the latest `*-nginx` version

Experimental images now default to using the nginx images

- `experimental-nginx` and `experimental` are re-builds of the latest `*-nginx` version

I follow the same version naming scheme as [Baikal](http://sabre.io/baikal/) themselves.

The following tags support multiple architectures, e.g. `amd64`, `arm32v7`, `arm64v8` and `i386`.

- [`0.11.1`, `0.11.1-nginx`](nginx.dockerfile)

## Quick reference

- **Where to file issues**:
  [https://github.com/aalmenar/baikal-docker/issues](https://github.com/aalmenar/baikal-docker/issues)
- **Supported architectures** ([more info](https://github.com/docker-library/official-images#architectures-other-than-amd64)):
  `amd64`, `arm32v7`, `arm64v8`
- **Image updates**:
  [PRs for aalmenar/baikal-docker](https://github.com/aalmenar/baikal-docker/pulls)
- **Source of this description**:
  [https://github.com/aalmenar/baikal-docker](https://github.com/aalmenar/baikal-docker)

## What is Baikal?

From [sabre.io/baikal](http://sabre.io/baikal/):

> Baikal is a Cal and CardDAV server, based on sabre/dav, that includes an administrative interface for easy management.
>
> For more information, read the main website at baikal-server.com.
>
> Baikal is developed by Net Gusto and fruux.

## How to use this image

The following command will start Baikal:

```bash
docker run --rm -it -p 80:80 ghcr.io/aalmenar/baikal:nginx
```

Alternatively, use the provided [examples/docker-compose.yaml](https://github.com/aalmenar/baikal-docker/blob/master/examples/docker-compose.yaml) from the Git repository:

```bash
docker compose up
```

You can now open [http://localhost](http://localhost) or [http://host-ip](http://host-ip) in your browser and use Baikal.

### Persistent Data

The image exposes the `/var/www/baikal/Specific` and `/var/www/baikal/config` folders, which contain the persistent data. These folders should be part of a regular backup.

If you want to use local folders instead of Docker volumes, see [examples/docker-compose.localvolumes.yaml](https://github.com/aalmenar/baikal-docker/blob/master/examples/docker-compose.localvolumes.yaml) to avoid file permission issues.

When the container starts, the startup script `/docker-entrypoint.d/40-fix-baikal-file-permissions.sh` ([Apache httpd](https://github.com/aalmenar/baikal-docker/blob/master/files/docker-entrypoint.d/httpd/40-fix-baikal-file-permissions.sh), [nginx](https://github.com/aalmenar/baikal-docker/blob/master/files/docker-entrypoint.d/nginx/40-fix-baikal-file-permissions.sh)) ensures that the file permissions are correct. You can disable this behaviour by setting the environment variable `BAIKAL_SKIP_CHOWN` to any value, e.g. `FALSE`.

### Further Guides

You can find more installation and configuration guides here:

- [Email Guide](https://github.com/aalmenar/baikal-docker/blob/master/docs/email-guide.md)
- [Home Assistant Fix](https://github.com/aalmenar/baikal-docker/blob/master/docs/home-assistant-fix.md)
- [SSL Certificate Guide](https://github.com/aalmenar/baikal-docker/blob/master/docs/ssl-certificates-guide.md)
- [systemd Guide](https://github.com/aalmenar/baikal-docker/blob/master/docs/systemd-guide.md)
- [Unraid Installation Guide](https://github.com/aalmenar/baikal-docker/blob/master/docs/unraid-installation-guide.md)

## Image Variants

The `ghcr.io/aalmenar/baikal` images come in several flavors, each designed for a specific use case.

### `ghcr.io/aalmenar/baikal:experimental`

This image has the latest code from the source repository, mainly used for testing before a version is released. Use this at your own risk.

### `ghcr.io/aalmenar/baikal:latest`

This image relies on [nginx](https://www.nginx.com/) and uses the [official nginx image](https://hub.docker.com/_/nginx/).

Compared to the Apache variant, it is significantly smaller (less than half the size) and produces no warning messages out-of-the-box.
