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

### 3. Metric-based Health
Monitor CPU and Memory usage. Tor is sensitive to memory pressure. If it exceeds 90% of its limit, consider increasing the resource allocation to avoid OOM kills.

## Identity Migration & Persistence

The relay identity is defined by the keys in `/var/lib/tor/keys`.

### 1. Key Persistence
The relay's "Consensus Weight" is tied to its identity. If `/var/lib/tor` is lost, the relay generates a new identity and loses its reputation. 
**Always use a PersistentVolume (PV) in Kubernetes.**

### 2. Backup/Restore
To migrate or backup the identity:
1. Back up the entire `/var/lib/tor/keys` directory.
2. Specifically, protect `ed25519_master_id_secret_key` and `secret_id_key`.
3. When restoring, ensure permissions are set to `0700` and owned by UID `101`.

## Troubleshooting

### Common Issue: Out of Memory (OOM)
- **Symptoms:** Container restarts with exit code 137.
- **Fix:** Increase the memory `limit` in your Kustomize overlay. Tor's memory usage scales with traffic and the size of the network consensus.

### Common Issue: Bandwidth Throttling
- **Symptoms:** High latency or `[notice] Your BandwidthRate is low` in logs.
- **Fix:** Adjust `TOR_BANDWIDTH_RATE` and `TOR_BANDWIDTH_BURST` environment variables.

### Common Issue: Clock Skew
- **Symptoms:** `[warn] Certificate already expired, or not yet valid.`
- **Fix:** Ensure the host node uses NTP (Network Time Protocol) to keep the clock synchronized.
