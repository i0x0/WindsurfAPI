# v2.0.133

- Added redacted semantic summaries for Cascade error trajectory steps. Proto
  traces now expose `semantic.steps[].errorStep` for `error_message` field `24`
  and `error` field `31`, including field numbers, byte lengths, hashes,
  nested string paths, and safe classification flags such as
  `permissionDenied`, `failedPrecondition`, `modelNotAvailable`, and
  `internalError`.
- Added a dedicated lab-only preview switch:
  `WINDSURFAPI_PROTO_TRACE_ERROR_STRINGS=1`. It is narrower than global string
  tracing and still redacts emails and token-like values.
- Documented the v2.0.132 VPS WebSearch/WebFetch canary. Direct
  `GetWebSearchResults` remains confirmed, but LS-native WebSearch/WebFetch
  still emitted no `search_web` / `read_url_content` oneof even though both
  native configs were sent. WebFetch direct still has no descriptor-backed
  endpoint and is not implemented from guesswork.

Verification:

- `node --check src/proto-trace.js`
- `node --test test/proto-trace.test.js`
