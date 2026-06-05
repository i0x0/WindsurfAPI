## v2.0.107 - native bridge env facts and smoke controls

Native bridge remains opt-in. This release tightens the gray-test path for
Read/Bash/Grep/Glob without enabling it by default.

### Native bridge

- Native bridge Cascade requests can now carry caller environment facts
  (`Working directory`, platform, git status) into a narrow
  `additional_instructions_section`.
- This section intentionally contains only environment context. It does not
  re-inject caller tool schemas or the prompt-emulation tool protocol.
- Fixed `<cwd>...</cwd>` extraction so Codex/XML-style environment blocks lift
  a real working directory instead of being ignored.

### Smoke

- `scripts/native-bridge-smoke.mjs` now sends a realistic system environment
  block by default.
- The smoke can wait for a natural SSE finish with
  `NATIVE_BRIDGE_SMOKE_EARLY_TOOL=0`; this verifies the server returns after a
  native tool call instead of relying on client cancellation.
- The smoke reports `smokeCwd`, `smokeFile`, `includeEnv`,
  `streamEarlyTool`, and `seenDone` for each stream scenario.

### Field note

- Real VPS gray smoke on v2.0.106 confirmed `Bash`/`run_command` can surface as
  a native tool call and finish with `reason=native_tool_call`.
- `Read`, `Grep`, and `Glob` still need proto trace / allowlist-name matrix
  testing; this release gives that testing better request shape and
  observability, but does not claim those tools are production-ready.
