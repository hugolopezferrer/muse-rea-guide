# Phase 3 — MCP bridge as a custom connector

**What it unlocks:** REA stops being just a CLI and becomes a **custom Muse connector** (`custom.rea`): its 207 MCP tools become invocable in a structured way, including stateful sessions (open a target once, query it several times).
**Rough time:** 15 minutes. Nothing to download: the MCP server already ships inside rea-agents.

## The idea in one paragraph

Muse supports custom connectors. The bridge is a stdio MCP client that starts `rea mcp` with the Phase 1+2 environment, negotiates the protocol, and offers fine-grained commands (list, describe, call, run a plan). It is **generic on purpose**: loading the schemas of all 207 tools into Muse's context would be wasteful; the bridge discovers them on demand. And no tool can be invoked unless it is on an allowlist generated from the live catalog.

## Prompt for your Muse

```text
Build Phase 3 of REA: a custom connector custom.rea bridging to REA's MCP
server (`rea mcp` over stdio) from ~/workspace/custom-connectors/custom.rea/,
with connector.yaml, CONNECTOR.md following the custom-connector template,
and a client in bin/client.py (Python, standard library only) that:
- starts the server with the rea-env.sh environment (JAVA_HOME,
  GHIDRA_INSTALL_DIR, REA_JADX_MCP_JAR, REA_WABT_BIN_DIRECTORY, PATH),
- negotiates the MCP protocol and exposes: status, list-tools,
  describe-tool, call-tool (arguments from a JSON file), run-plan (several
  calls inside ONE session, for stateful flows) and list-prompts,
- rejects any tool not present in bin/allowed_tools.json, generated from
  the live catalog (207 names on REA 6.7.0),
- keeps server logs on stderr, never mixed into the JSON output.
Add a `rea-mcp` wrapper in ~/workspace/tools/bin and a reproducible script.
Verify for me: the handshake (server rea 6.7.0, 207 tools), one harmless
single call, and a Ghidra session plan (open the test binary, decompile the
same function as Phase 1, close) whose pseudocode matches the CLI's.
```

## Verification (don't call it done without it)

- `rea-mcp status` → server `rea`, version 6.7.0, **tool_count: 207**.
- `rea-mcp call-tool` on `inspect_pcap_client` → detects your tshark.
- The session plan returns the same pseudocode as the CLI for the same function.
- Muse's connector search (`muse.connector_search`) finds `custom.rea`.

## Limits

- Each client invocation is one session: when it ends, the server closes. Stateful flows go inside a single `run-plan`.
- The bridge adds no analysis power: it delivers whatever REA and the machine give (RAM remains the ceiling for large targets).
