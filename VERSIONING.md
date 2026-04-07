# Versioning & Matrix Strategy (v2)

This repository utilizes a build matrix to provide multiple permutations of Tor and Alpine versions.

## The Tagging Matrix

| Alpine Branch | Tor Package | Primary Tag | Aliases / Floating Tags | Stability |
|---------------|-------------|-------------|-------------------------|-----------|
| **edge**      | latest      | `latest`    | `edge`, `edge-alpine-edge` | **Bleeding Edge** |
| **v3.23**     | 0.4.9.x     | `v3.23`     | `stable-alpine3.23`     | Stable |
| **v3.22**     | 0.4.8.x     | `v3.22`     | `oldstable-alpine3.22`  | Legacy |

## ⚠️ Critical Warning: `latest` Tracks Alpine Edge

By default, the `latest` tag points to the **Alpine Edge** build. This ensures that users receive the most up-to-date security patches and Tor features.

- **For Production:** We strongly recommend pinning to a specific Alpine version tag (e.g., `ghcr.io/sparksis/tor-relay:v3.23`) if you require long-term stability and predictable behavior.
- **For Testing:** Use `latest` or `edge` to stay on the absolute front line of development.

## Auditability

Every image in the matrix has its specific `apko.yaml` and SBOM published as build artifacts. You can verify the exact package versions used in any given tag by inspecting the attached SBOM in the GitHub/GitLab Release objects.
