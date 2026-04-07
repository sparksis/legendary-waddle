# Versioning Strategy

This repository follows a strict OCI tagging strategy to ensure stability, auditability, and ease of use for production workloads.

## Tag Types

### `latest`
- **Source:** Tracked against the latest Alpine stable release (currently **v3.23**).
- **Update Frequency:** Updated on every push to the `main` branch.
- **Use Case:** General use for users who want the most recent stable release.

### `edge`
- **Source:** Tracked against **Alpine Edge**.
- **Update Frequency:** Daily cron-triggered build.
- **Use Case:** Testing and early adoption of new Tor or system package versions. **Not recommended for production relays.**

### `vX.Y.Z` (Semantic Tags)
- **Source:** Mapped to specific Tor package releases and system snapshots.
- **Update Frequency:** Created upon a GitHub/GitLab Release.
- **Use Case:** Production environments where immutability and reproducibility are critical.

## Auditability & Reproducibility

For every release (Semantic or Edge), the exact `apko.yaml` used to build the image is published as a build asset. 

- **Dynamic Locking:** Package versions are pinned at build time to ensure that "re-pulling" a specific tag results in the exact same bit-for-bit environment.
- **SBOMs:** Every image is accompanied by a Software Bill of Materials (SBOM) in SPDX/CycloneDX format, attached to the release metadata for vulnerability scanning.
