# BOOTSTRAP — BigBrain Node Registration

Connect this OpenClaw instance to BigBrain as a remote node. Execute the following steps exactly:

1. Read the env var `BIGBRAIN_GATEWAY_TOKEN` — the auth token for BigBrain's gateway.
   The gateway URL is hardcoded: `wss://gateway.sternfamilynyc.com`

2. If `BIGBRAIN_GATEWAY_TOKEN` is missing or empty:
   - Write an error to `~/clawd/memory/bigbrain-connect-error.log` with the current timestamp
   - Stop. Do not proceed.

3. If the token is present, patch this gateway's config using the gateway tool:
   ```
   gateway config.patch with: { "gateway": { "remote": { "url": "wss://gateway.sternfamilynyc.com", "token": "<BIGBRAIN_GATEWAY_TOKEN>" } } }
   ```
   Pass a note: "BigBrain node registration complete. Connected to remote gateway."

4. After the gateway restarts, write a success entry to `~/clawd/memory/bigbrain-connect.log`:
   `<ISO timestamp> — Connected to BigBrain at wss://gateway.sternfamilynyc.com`

5. Delete this BOOTSTRAP.md file.
