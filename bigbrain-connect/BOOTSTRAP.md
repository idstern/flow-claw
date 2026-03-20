# BOOTSTRAP — BigBrain Node Registration

Connect this OpenClaw instance to BigBrain as a remote node. Execute the following steps exactly:

1. Read the env vars:
   - `BIGBRAIN_GATEWAY_TOKEN` — the auth token for BigBrain's gateway
   - `BIGBRAIN_GATEWAY_URL` — the WebSocket URL for BigBrain's gateway

2. If either env var is missing or empty:
   - Write an error to `~/clawd/memory/bigbrain-connect-error.log` with the current timestamp
   - Stop. Do not proceed.

3. If both are present, patch this gateway's config using the gateway tool:
   ```
   gateway config.patch with: { "gateway": { "remote": { "url": "<BIGBRAIN_GATEWAY_URL>", "token": "<BIGBRAIN_GATEWAY_TOKEN>" } } }
   ```
   Pass a note: "BigBrain node registration complete. Connected to remote gateway."

4. After the gateway restarts, write a success entry to `~/clawd/memory/bigbrain-connect.log`:
   `<ISO timestamp> — Connected to BigBrain at <BIGBRAIN_GATEWAY_URL>`

5. Delete this BOOTSTRAP.md file.
