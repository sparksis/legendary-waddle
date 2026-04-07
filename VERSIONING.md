# Versioning & Mirror Strategy

This repository strictly mirrors the Tor Upstream Version available in the Alpine Linux repositories. We do not use independent semantic versioning.

## Upstream Mapping

The release tag in this repository corresponds exactly to the Tor version being wrapped.

- **Direct Mirror**: If you pull tag `0.4.8.13`, you are getting the Tor `0.4.8.13` binary from Alpine.
- **Alpine Permutations**: For every Tor version, we build across multiple Alpine release tracks (v3.22, v3.23, edge).

## Tagging Logic

OCI images in the container registry are tagged as follows:

| Upstream Tor | Alpine Track | OCI Tag | Description |
|--------------|--------------|---------|-------------|
| **X.Y.Z**    | **edge**     | `X.Y.Z-edge`, `latest`, `edge` | Most up-to-date security patches. |
| **X.Y.Z**    | **v3.23**    | `X.Y.Z-v3.23` | Stable production release. |
| **X.Y.Z**    | **v3.22**    | `X.Y.Z-v3.22` | Legacy track. |

## Immutability & Auditability

Every release is "frozen" at hydration time. The `apko.yaml` asset attached to each GitHub Release contains hard-pinned package versions (e.g., `tor=0.4.8.13-r0`). This guarantees that the OCI build is reproducible and free from accidental upstream updates between the release and deployment.
