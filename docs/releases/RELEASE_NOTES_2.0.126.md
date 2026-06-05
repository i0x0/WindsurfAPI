## v2.0.126 - Web native protocol fields confirmed

- Confirmed `CascadeToolConfig.search_web=13` and
  `CascadeToolConfig.read_url_content=37` from the LS protobuf descriptors and
  live gated traces. The native tool-config encoder can now emit those fields
  when WebSearch/WebFetch are explicitly allowlisted for protocol experiments.
- Proto tracing now decodes web subconfig payloads:
  `ThirdPartyWebSearchConfig.provider/model` and
  `AutoWebRequestConfig.allowlist/auto_execution_policy`. URL allowlists are
  summarized by byte length and hash, not raw URL text.
- Documented the web enum values found in the LS binary:
  `OPENAI`, `O3`, `GPT_4_1`, `O4_MINI`, and web auto-execution
  `DISABLED` / `ALLOWLIST` / `TURBO`.
- VPS canary result: loaded accounts had `cascadeWebSearchEnabled=true`, and
  direct `GetWebSearchResults` returned HTTP 200 with results, but LS-native
  WebSearch/WebFetch still produced a `permission_denied` Cascade error step
  before any `search_web` / `read_url_content` oneof was emitted. These tools
  therefore remain outside the default native bridge allowlist.

Verification:

- `node --check src\windsurf.js`
- `node --check src\proto-trace.js`
- `node --test test\proto-trace.test.js test\cascade-native-bridge.test.js test\native-tool-routing.test.js`
