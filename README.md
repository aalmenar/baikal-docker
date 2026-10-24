# Baikal

This repository is a fork of [ckulka/baikal-docker](https://github.com/ckulka/baikal-docker/) who has made an amazing job providing this image and work to build it.

There are some variants from the original repository. Default images are the `*-nginx`.

[![Latest images](https://github.com/aalmenar/baikal-docker/actions/workflows/build-latest.yaml/badge.svg)](https://github.com/aalmenar/baikal-docker/actions/workflows/build-latest.yaml) [![Experimental images](https://github.com/aalmenar/baikal-docker/actions/workflows/build-experimental.yaml/badge.svg)](https://github.com/aalmenar/baikal-docker/actions/workflows/build-experimental.yaml) ![Docker Architectures](https://img.shields.io/badge/arch-amd64%20%7C%20arm32v7%20%7C%20arm64v8-informational)

These dockerfiles provide a ready-to-go [Baikal server](http://sabre.io/baikal/).

## Supported tags and respective Dockerfile links

| Tag(s)                               | Description                                                                                                                                                                     | Mutable?                     |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- |
| `<V>`, `<V>-nginx`                   | Moving pointer to the newest image revision of Baikal `<V>`                                                                                                                     | Yes (by design, documented)  |
| `<V>-rN`, `<V>-rN-nginx`             | Pinned image revision of Baikal `<V>`                                                                                                                                           | No (immutable by convention) |
| `latest`, `nginx`                    | Newest release; [weekly re-builds](https://github.com/aalmenar/baikal-docker/actions/workflows/build-latest.yaml) to include the latest base image with the most recent updates | Yes                          |
| `experimental`, `experimental-nginx` | Experimental builds                                                                                                                                                             | Yes                          |

`<V>` tags are moving pointers to the newest image revision of that Baikal version; pin `<V>-rN` or a digest for reproducibility. Note that `0.13.1` never exists as an image tag unless real Baikal 0.13.1 ships.

The version part of the tags follows [Baikal](http://sabre.io/baikal/)'s own releases; an optional `-r<N>` suffix marks an image-only rebuild of the same Baikal release.

The following tags support multiple architectures: `amd64`, `arm32v7` and `arm64v8`.

- [`0.13.0`, `0.13.0-nginx`](nginx.dockerfile) — future rebuilds add e.g. `0.13.0-r1`

To re-build an image without a Baikal version change, bump the base images/scripts on `main` and publish a release tagged `<V>-r<N>`, e.g.: `gh release create 0.13.0-r1 --target main --title "Baikal 0.13.0 — image rebuild 1" --notes "Base image refresh; no Baikal change; upgrade wizard not required."`

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

When the container starts, the startup script `/docker-entrypoint.d/40-fix-baikal-file-permissions.sh` ([source](https://github.com/aalmenar/baikal-docker/blob/master/files/docker-entrypoint.d/nginx/40-fix-baikal-file-permissions.sh)) ensures that the file permissions are correct. You can disable this behaviour by setting the environment variable `BAIKAL_SKIP_CHOWN` to any value, e.g. `FALSE`.

### Upgrading

Before upgrading, back up your database and the persistent `Specific` and `config` folders.

> [!IMPORTANT]
> After updating the image and recreating the container, open Baikal's administration panel at `/admin/` (relative to your Baikal URL) and complete the upgrade wizard if prompted. Updating the container alone does not complete the Baikal upgrade; any required database migrations must be applied through the wizard before normal use can resume.

See the [Baikal upgrade guide](https://sabre.io/baikal/upgrade/) for more information.

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

This image relies on [nginx](https://www.nginx.com/) and uses the [official nginx image](https://hub.docker.com/_/nginx/). It is based on Debian and produces no warning messages out-of-the-box.

> The former Apache httpd image variant is discontinued: it is no longer built or published, and the existing `:apache` tags on ghcr.io are frozen at their last versions. The nginx variant is the only image going forward.
