# Production-Hardened Tor Relay (Apko Mirror)

This repository provides a production-grade OCI factory for Tor relays, built with `apko` for zero-trust environments. The image is "distroless-style" (no shell) and mirrors the upstream Tor versions available in Alpine Linux.

## Features

- **Zero-Trust Footprint:** No shell (`sh`/`bash`), no package manager, non-root execution (UID 101).
- **Upstream Mirroring:** Release tags directly mirror the Tor version in Alpine repositories.
- **Multi-Arch Automation:** Automated builds for `x86_64` and `aarch64`.
- **Supply Chain Security:** SBOMs generated for every release and image signing via GitHub attestations.

## Configuration (Environment Variables)

The relay entrypoint supports the following variables:

| Variable | Description | Default |
|----------|-------------|---------|
| `TOR_NICKNAME` | Public nickname for the relay | `ApkoRelay` |
| `TOR_BANDWIDTH_RATE` | Sustained bandwidth limit | `1 MByte` |
| `TOR_BANDWIDTH_BURST` | Maximum bandwidth burst | `2 MByte` |
| `TOR_CONTACT` | Contact information for the operator | `operator@example.com` |

## Supported Tags (OCI Registry)

Tags are dynamically generated based on the Alpine Linux APK index.

- **Pinned Releases**: `<tor-version>-alpine<alpine-version>` (e.g., `0.4.8.13-v3.23`)
- **Floating Tags**: `latest` and `edge` (both tracking Alpine Edge for security patches).

## Operational Excellence

- **Versioning:** See [VERSIONING.md](VERSIONING.md) for our tagging strategy.
- **Runbook:** Refer to [RUNBOOK.md](RUNBOOK.md) for health checks, key migration, and troubleshooting.

## Legal & Abuse

Running a Tor relay contributes to global privacy and freedom of information. However, operators should be aware of the legal landscape.

### ISP Notice Template
If you receive an inquiry from your ISP, you may use the following template:
> To whom it may concern,
> This IP address is hosting a Tor relay. Tor is a network of volunteer-run servers that helps people improve their privacy and security on the Internet. This relay is configured as a non-exit relay, meaning it only passes encrypted traffic within the Tor network and does not connect to the public Internet on behalf of users.

---
*Built with ❤️ using [apko](https://github.com/chainguard-dev/apko)*
