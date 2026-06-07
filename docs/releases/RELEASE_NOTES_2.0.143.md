## v2.0.143 - diagnostics and canary hardening

This release strengthens diagnostics for tool-call evidence collection
(#177/#178), hardens the WebFetch canary pipeline (#183), and completes
the SWE-1.6 special-agent POC boundary coverage (#190). No default
behavior changes; native bridge production scope remains Bash-family only.

### BridgeResult diagnostics (#177/#178)

- New `BridgeResult[reqId]` log line emitted after every tool-bearing
  request (both streaming and non-streaming paths).
- Tracks `cascadeToolCalls`, `mappedToolCalls`, `unmappedToolCalls`,
  `emulatedToolCalls`, `totalToolCalls`, `argParseFailures`, and
  `reverseFailures` per request.
- Logs tool names and Cascade kinds but never arguments or user content.
- Combined with the existing `ToolRoute[...]` pre-request log, reporters
  can now distinguish: bridge enabled but no Cascade tool call returned,
  Cascade tool call returned but reverse-mapping failed, emulation
  fallback succeeded, and the zero-tool-call scenario.

### WebFetch canary hardening (#183)

- `HandleCascadeUserInteraction` RPC call wrapped in try/catch; approval
  failures are logged with SHA-256 hashed cascade/origin identifiers and
  do not terminate the polling loop.
- `scripts/native-bridge-smoke.mjs` WebFetch scenario now enforces a
  hard verdict: only `completed_web_document` in proto trace counts as
  pass; `pending_permission`, `auto_run_decision_only`, and error states
  are explicit warn/fail with classification detail.
- WebFetch preflight warns when `WEBFETCH_AUTO_APPROVE`,
  `WEBFETCH_AUTO_APPROVE_ORIGINS`, or `POLL_AFTER_TOOL` env vars are
  missing (non-blocking).

### SWE-1.6 special-agent boundary (#190)

- `/health?verbose=1` now exposes `queueTimeoutMs`, `runTimeoutMs`, and
  `outputLimitBytes` in the `specialAgent` block when the backend is
  enabled.
- Added unit test for media content rejection (`unsupported_media` on
  `image_url` content parts).
- `scripts/special-agent-smoke.mjs` now runs two negative smoke stages
  after the positive text-only chat: tools boundary (`400
  unsupported_tool_boundary`) and media boundary (`400
  unsupported_media`).

### Validation

- 1098 tests pass (0 fail).
- Secret scan clean.
- No new dependencies.
