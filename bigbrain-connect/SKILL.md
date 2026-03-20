---
name: bigbrain-connect
description: >
  Registers this OpenClaw instance as a remote node on BigBrain's gateway automatically
  at startup. Reads BIGBRAIN_GATEWAY_TOKEN from environment (URL is hardcoded) and
  patches gateway.remote config so this instance connects to BigBrain permanently.
  Use when asked to connect to BigBrain, re-run pairing, or check node connection status.
---

# BigBrain Connect

Connects this OpenClaw instance to BigBrain as a remote node.

## How it works

On startup, `BOOTSTRAP.md` runs automatically (via the `boot-md` hook). It reads
`BIGBRAIN_GATEWAY_TOKEN` from the container environment (the gateway URL is hardcoded
as `wss://gateway.sternfamilynyc.com`), patches `gateway.remote.url` and
`gateway.remote.token` into this instance's config, then restarts the gateway so
the connection is live. The config persists across restarts.

## Setup (on the flowtools container)

Set this one secret in the Flow Tools console before deploying:
- `BIGBRAIN_GATEWAY_TOKEN` — the gateway token for BigBrain (ask Ilan)

The gateway URL (`wss://gateway.sternfamilynyc.com`) is hardcoded in the skill.

## Manual trigger

If you need to re-run the connection (e.g. after a token rotation):

1. Check the token: `echo $BIGBRAIN_GATEWAY_TOKEN`
2. Run `gateway config.patch` with:
   ```json
   { "gateway": { "remote": { "url": "wss://gateway.sternfamilynyc.com", "token": "<BIGBRAIN_GATEWAY_TOKEN>" } } }
   ```
3. Run `gateway restart`
4. Confirm via `gateway status` — remote should show as connected

## Security

- Token is never stored in any file — only read from env at runtime
- Connection is outbound only (this container → BigBrain)
- BigBrain validates the token before accepting this node
- To rotate: update `BIGBRAIN_GATEWAY_TOKEN` env var in Flow Tools console + redeploy
