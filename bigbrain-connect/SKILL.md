---
name: bigbrain-connect
description: >
  Registers this OpenClaw instance as a remote node on BigBrain's gateway automatically
  at startup. Reads BIGBRAIN_GATEWAY_TOKEN and BIGBRAIN_GATEWAY_URL from environment
  and patches gateway.remote config so this instance connects to BigBrain permanently.
  Use when asked to connect to BigBrain, re-run pairing, or check node connection status.
---

# BigBrain Connect

Connects this OpenClaw instance to BigBrain as a remote node.

## How it works

On startup, `BOOTSTRAP.md` runs automatically (via the `boot-md` hook). It reads
`BIGBRAIN_GATEWAY_TOKEN` and `BIGBRAIN_GATEWAY_URL` from the container environment,
patches `gateway.remote.url` and `gateway.remote.token` into this instance's config,
then restarts the gateway so the connection is live. The config persists across restarts.

## Manual trigger

If you need to re-run the connection (e.g. after a token rotation):

1. Check env vars are set: `echo $BIGBRAIN_GATEWAY_TOKEN && echo $BIGBRAIN_GATEWAY_URL`
2. If set, run `gateway config.patch` with:
   ```json
   { "gateway": { "remote": { "url": "<BIGBRAIN_GATEWAY_URL>", "token": "<BIGBRAIN_GATEWAY_TOKEN>" } } }
   ```
3. Run `gateway restart`
4. Confirm via `gateway status` — remote should show as connected

## Security

- Token is never stored in any file — only read from env at runtime
- Connection is outbound only (this container → BigBrain)
- BigBrain validates the token before accepting this node
- To rotate: update `BIGBRAIN_GATEWAY_TOKEN` env var in Flow Tools console + redeploy
