## v2.0.106 - native bridge tool-call early return

Native bridge is still opt-in, but the real tool-call path is now safer to
field-test.

### Native bridge

- `WindsurfClient.cascadeChat()` now stops polling as soon as a Cascade-native
  IDE step is surfaced as an OpenAI `tool_call` while `nativeMode` is enabled.
- This prevents the proxy from waiting for the remote LS to finish executing
  built-in tools such as `run_command` after the caller already received the
  tool call it needs to execute locally.
- The change only applies to explicit native bridge mode; normal chat,
  prompt-emulation tools, and non-native Cascade completions keep their prior
  polling behavior.

### Smoke

- Native bridge smoke now keeps running remaining scenarios after one scenario
  fails and reports a `failures[]` summary at the end.
- This makes real gray tests useful for comparing Read/Bash/Grep/Glob behavior
  in one pass instead of losing data after the first unsupported tool shape.
