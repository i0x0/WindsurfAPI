## v2.0.124 - JSON-safe identity cleanup, web-step tracing, and quieter LS maintenance

- #185 follow-up: response-side Cascade identity neutralization now leaves
  parseable JSON payloads unchanged, including fenced JSON and JSON arrays. This
  prevents the cleanup layer from rewriting model-info JSON bodies that some
  clients surface during upstream failures.
- Native trajectory parsing now recognizes web tool steps emitted as
  `read_url_content` field `40` and `search_web` field `42`, surfacing the
  observed argument fields plus summary/result text for protocol tracing.
- WebSearch/WebFetch remain outside the default native bridge allowlist. Their
  trajectory steps are visible now, but the tool-config submessage fields are
  still unproven and require gated live smoke before production enablement.
- `scripts/native-bridge-smoke.mjs` can now explicitly run `WebSearch` and
  `WebFetch` scenarios for protocol experiments. `NATIVE_BRIDGE_SMOKE_TOOLS=all`
  still excludes them.
- Background credit refresh and Firebase token refresh now skip accounts that
  are currently serving chat, account maintenance, or LS maintenance by
  default. Set `WINDSURFAPI_BACKGROUND_MAINTENANCE_SKIP_BUSY=0` only if you
  want scheduled maintenance to compete with production traffic.
- `docs/native-bridge-protocol-notes.md` and `.env.example` document the new
  web-step tracing boundary and maintenance skip knob.

Verification:

- `node --check src\handlers\chat.js`
- `node --check src\windsurf.js`
- `node --check src\auth.js`
- `node --check scripts\native-bridge-smoke.mjs`
- `node --test test\identity-neutralization.test.js test\v2070-issue-fixes.test.js test\langserver-resource.test.js`
- `node --test test\cascade-native-bridge.test.js test\native-tool-routing.test.js test\native-bridge-smoke.test.js test\identity-neutralization.test.js test\v2070-issue-fixes.test.js test\langserver-resource.test.js`
- `node --test --test-timeout=120000 --test-force-exit test\*.test.js` passes: 1021/1021.
