# FreedomConnect Relay

This public repository contains signed Cloudflare Quick Tunnel endpoint records. It contains no FreedomConnect source code, VPN device tokens, origin server addresses, SSH private keys, or signing private keys.

Records live at `relays/<server_id>.json`. Clients obtain the discovery URL and Ed25519 verification key from their imported FreedomConnect profile. A key supplied by this repository must never replace that trusted profile key.

Each record authenticates the server identity, Quick Tunnel WSS URL, monotonically increasing sequence, issue time and expiry. Records are valid for at most 48 hours and the publisher renews them every 12 hours while the relay is running. Clients verify signatures and reject expired records, rollback and non-Quick-Tunnel destinations. A valid cached record may be used during a temporary directory outage.

Clients check the public GitHub Contents API for current metadata, with bounded requests and rate limiting, and use Raw/cache as fallback. Raw CDN responses can lag behind commits.

The VPS verifies its public WebSocket and pinned inner TLS path before publishing, commits only its own record, and acknowledges the sequence for new profile exports only after a successful push. Publication uses a repository-specific deploy key. GitHub does not receive VPN device credentials.

Android 0.8.39 and Windows Client 0.8.25 support this directory. Import a discovery-enabled profile once; after a Quick Tunnel domain change the same profile can reconnect to the newly published address. Direct connections to the VPS are not a fallback.
