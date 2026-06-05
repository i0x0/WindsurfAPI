## v2.0.129 - Native bridge gray-gate model alias fix

- Fixed the native bridge model gray gate so it checks both the internal routing
  model key and the caller's requested model alias. A real v2.0.128 canary with
  `WINDSURFAPI_NATIVE_TOOL_BRIDGE_MODELS=claude-haiku-4.5` was incorrectly
  rejected after chat routing normalized the request to `claude-4.5-haiku`.
- Native bridge decision telemetry now includes `requestedModel` alongside
  `modelKey`, making dashboard/health output explain whether a gray gate matched
  the request alias or the internal routing key.
- README native-bridge defaults now match the code: production default allowlist
  is still the mature Bash/run_command path only. Read/Grep/Glob and
  WebSearch/WebFetch remain explicit gray-canary tools until their live protocol
  matrix is proven.

Verification:

- `node --check src\cascade-native-bridge.js`
- `node --check src\native-bridge-stats.js`
- `node --check src\handlers\chat.js`
- `node --test test\native-tool-routing.test.js test\native-bridge-stats.test.js test\cascade-native-bridge.test.js test\dashboard-api.test.js`
- `node --test --test-timeout=120000 --test-force-exit test\*.test.js`
