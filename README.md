# Hardened Multi-Arch Tor Relay (Apko + K8s)

This repository provides a production-grade OCI factory for Tor relays, built with `apko` for zero-trust environments. The image is "distroless-style" (no shell) and hardened for execution in Kubernetes.

## Features

- **Minimalist:** No shell, no unnecessary packages. Built with `apko` for a tiny footprint (<15MB).
- **Hardened:** Runs as a non-root `tor` user (UID/GID 101).
- **Multi-Arch:** Supports both `x86_64` and `aarch64`.
- **Kustomize Layer:** Secure-by-default manifests with `readOnlyRootFilesystem` and explicit capability dropping.
- **Supply Chain Security:** SBOMs generated for every release and image signing via GitHub attestations.

## Quick Start (Kubernetes)

Deploy a hardened relay to your cluster using Kustomize:

```bash
# 1. Clone the repository
git clone https://github.com/your-org/tor-relay.git
cd tor-relay

# 2. Review the base configuration in kustomize/base/
# 3. Apply to your cluster
kubectl apply -k kustomize/base/
```

## Configuration

The relay can be configured using the following environment variables:

| Variable | Description | Default |
|----------|-------------|---------|
| `TOR_NICKNAME` | The nickname for the Tor relay | `ApkoRelay` |
| `TOR_BANDWIDTH_RATE` | Average bandwidth to use | `1 MByte` |
| `TOR_BANDWIDTH_BURST` | Maximum bandwidth burst | `2 MByte` |
| `TOR_CONTACT` | Contact info for the relay operator | `operator@example.com` |

## Running with Docker

To run the relay with default settings:

```bash
docker run -d --name tor-relay -p 9001:9001 ghcr.io/<your-repo>:latest
```

## Operational Excellence

- **Runbook:** Refer to [RUNBOOK.md](RUNBOOK.md) for health checks, key migration, and troubleshooting.
- **Versioning:** Refer to [VERSIONING.md](VERSIONING.md) for our tagging strategy.

## Legal & Abuse

Running a Tor relay contributes to global privacy and freedom of information. However, operators should be aware of the legal landscape.

### ISP Notice Template
If you receive an inquiry from your ISP, you may use the following template:
> To whom it may concern,
> This IP address is hosting a Tor relay. Tor is a network of volunteer-run servers that helps people improve their privacy and security on the Internet. This relay is configured as a non-exit relay, meaning it only passes encrypted traffic within the Tor network and does not connect to the public Internet on behalf of users.

---
*Built with ❤️ using [apko](https://github.com/chainguard-dev/apko)*
