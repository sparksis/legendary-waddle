# Hardened Multi-Arch Tor Relay (Apko)

This repository contains an `apko` configuration to build a minimal, hardened, and rootless Tor relay image based on **Alpine v3.23**.

## Features

- **Minimalist:** No shell, no unnecessary packages. Built with `apko` for a tiny footprint (<15MB).
- **Hardened:** Runs as a non-root `tor` user (UID/GID 100).
- **Multi-Arch:** Supports both `x86_64` and `aarch64`.
- **Dynamic Configuration:** Uses environment variables to configure the relay.
- **Dual Registry:** Published to GitHub Container Registry (GHCR) and GitLab Container Registry.

## Configuration

The relay can be configured using the following environment variables:

| Variable | Description | Default |
|----------|-------------|---------|
| `TOR_NICKNAME` | The nickname for the Tor relay | `ApkoRelay` |
| `TOR_BANDWIDTH_RATE` | Average bandwidth to use | `1 MByte` |
| `TOR_BANDWIDTH_BURST` | Maximum bandwidth burst | `2 MByte` |
| `TOR_CONTACT` | Contact info for the relay operator | `none` |

## Running the Relay

To run the relay with default settings:

```bash
docker run -d --name tor-relay -p 9001:9001 ghcr.io/<your-repo>:latest
```

To run with custom settings:

```bash
docker run -d \
  --name tor-relay \
  -p 9001:9001 \
  -e TOR_NICKNAME=MyRelay \
  -e TOR_CONTACT="operator@example.com" \
  ghcr.io/<your-repo>:latest
```

## CI/CD Pipelines

- **GitHub Actions:** Builds and publishes the multi-arch image to GHCR. We use industry-standard Docker actions (`metadata-action`, `login-action`) for tagging and authentication, and `actions/attest-build-provenance` for OCI attestations and image signing to ensure a secure and verifiable supply chain.
- **GitLab CI:** Configured to build and publish the multi-arch image to the GitLab registry.

## Building Locally

To build the image locally using `apko`:

```bash
apko build apko.yaml tor-relay:latest output.tar --arch x86_64,aarch64
```
