# distil

Zero-CVE container images. Fully automated. Free and open source.

Built with [apko](https://github.com/chainguard-dev/apko) + [Wolfi](https://wolfi.dev) packages. No Dockerfiles, no shell, no package manager in the final image — just the binary and its dependencies.

## Images

| Image | Tags | Status |
|-------|------|--------|
| `ghcr.io/vhco-pro/distil/nginx` | `latest`, `1.x.x`, `YYYYMMDD` | ![Build](https://github.com/vhco-pro/distil/actions/workflows/build.yaml/badge.svg) |
| `ghcr.io/vhco-pro/distil/redis` | `latest`, `7.x.x`, `YYYYMMDD` | ![Build](https://github.com/vhco-pro/distil/actions/workflows/build.yaml/badge.svg) |

## Usage

```bash
docker pull ghcr.io/vhco-pro/distil/nginx:latest
docker pull ghcr.io/vhco-pro/distil/redis:7.4.2
```

## How it works

1. **Declarative image definitions** — each image is an `apko.yaml` file that lists only the packages needed. No `RUN apt-get`, no layers of cruft.
2. **Daily rebuilds** — images are rebuilt every day at 06:00 UTC against the latest Wolfi packages, picking up security patches automatically.
3. **Upstream release tracking** — a scheduled workflow checks upstream repos daily and creates PRs when new versions are detected.
4. **Dual scanner verification** — every build is scanned with both [Trivy](https://trivy.dev) and [Grype](https://github.com/anchore/grype). The build fails if any vulnerability is found.

## Why zero CVEs?

Most container image CVEs come from packages you don't even use — shells, package managers, documentation, dev tools. Distil images contain only the application binary and its runtime dependencies. Nothing else.

## Building locally

```bash
# Install apko
brew install apko

# Build nginx
apko build images/nginx/apko.yaml distil/nginx:dev nginx.tar
docker load < nginx.tar

# Build redis
apko build images/redis/apko.yaml distil/redis:dev redis.tar
docker load < redis.tar
```

## Adding a new image

1. Create `images/<name>/apko.yaml` with the package list
2. Add a matrix entry in `.github/workflows/build.yaml`
3. Add the upstream repo to the check-upstream workflow matrix
4. Open a PR

## License

Apache-2.0
