# Tor Relay Operations Runbook

This document provides guidance for human operators managing the Tor relay in production.

## Health Checks

### 1. Visual Verification (nyx)
If you have access to the pod/container:
```bash
# Note: This image is distroless. To use nyx, you must connect to the ControlPort 
# from an external management pod or use a sidecar.
```

### 2. Log-based Health
Monitor stdout for the following success markers:
- `Self-testing indicates your ORPort is reachable from the outside. Excellent.`
- `Bootstrapped 100% (done): Done`

## Identity Migration & Persistence

The relay identity is defined by the keys in `/var/lib/tor/keys`.

### 1. Key Persistence
The relay's "Consensus Weight" is tied to its identity. If `/var/lib/tor` is lost, the relay generates a new identity and loses its reputation. 

### 2. Backup/Restore
To migrate or backup the identity:
1. Back up the entire `/var/lib/tor/keys` directory.
2. Specifically, protect `ed25519_master_id_secret_key` and `secret_id_key`.
3. When restoring, ensure permissions are set to `0700` and owned by UID `101`.

## Troubleshooting

### Switching Tracks (Edge vs. Stable)

If you are using the `latest` tag (which tracks Alpine Edge) and encounter a breaking change or instability, follow these steps to switch to a stable track:

1. **Update Image Reference:** Change your deployment or docker-compose file to use a pinned version, such as `:0.4.8.13-v3.23`.
2. **Apply Changes:** 
   - Docker: `docker pull ghcr.io/your-org/tor-relay:0.4.8.13-v3.23 && docker-compose up -d`
3. **Verify Identity:** Ensure your `/var/lib/tor` persistence is intact. Switching tracks does not require new keys, as the Tor configuration remains compatible across Alpine versions.
