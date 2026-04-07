# Versioning Strategy

This repository follows a standard semantic versioning (SemVer) strategy for the Tor relay image.

## Tag Types

### `latest`
- **Source:** Tracked against the current stable Alpine release (v3.23).
- **Update Frequency:** Updated on every push to the `main` branch.
- **Use Case:** General use for users who want the most recent stable release.

### `vX.Y.Z` (Semantic Tags)
- **Source:** Mapped to specific Tor package releases and project milestones.
- **Update Frequency:** Created upon a GitHub Release.
- **Use Case:** Production environments where immutability and reproducibility are critical.

## Auditability & Reproducibility

For every release, the exact `apko.yaml` used to build the image is published as a build artifact.

- **SBOMs:** Every image is accompanied by a Software Bill of Materials (SBOM) in SPDX/CycloneDX format, attached to the release metadata for vulnerability scanning.
