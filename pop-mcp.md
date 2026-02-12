---
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: false
---

# Pop MCP

Pop MCP is an MCP server that exposes Pop CLI as structured tools for AI clients (contract + chain workflows).
Pop CLI must be installed and working:

```bash
pop --version
```

If needed: [Install Pop CLI](welcome/install-pop-cli.md)

## Install

Build from source:

```bash
git clone https://github.com/r0gue-io/pop-mcp.git
cd pop-mcp
cargo build --release
```

Server binary:
`target/release/pop-mcp-server`

## Connect A Client

Pop MCP runs over stdio. You do not run the server manually. Your AI client launches it.

Pop MCP can sign transactions via `PRIVATE_KEY`. Read-only calls work without it. Use dev keys like `//Alice` and `//Bob` for local networks only.

### Claude Code (CLI)

Project scope (writes `.mcp.json` in the repo):

```bash
claude mcp add pop-mcp --scope project --env PRIVATE_KEY=//Alice -- /absolute/path/to/pop-mcp/target/release/pop-mcp-server
```

Verify:

```bash
claude mcp get pop-mcp
```

### Codex CLI

Config is stored in `~/.codex/config.toml` (or project `.codex/config.toml`).

```bash
codex mcp add pop-mcp --env PRIVATE_KEY=//Alice -- /absolute/path/to/pop-mcp/target/release/pop-mcp-server
```

Verify:

```bash
codex mcp list
```

## Try It (Prompt Workflows)

Once Pop MCP is configured, ask your AI client to do real work. You should see it call tools like `create_contract`, `up_ink_node`, `deploy_contract`, and `call_contract` on your behalf.

### Example 1: Contract quickstart (local node)

Ask:

> Create a new ink! contract project, start a local ink node, build and deploy the contract, then call a read method to show its initial state. After that, call a write method to change state and read it back.

### Example 2: Local network quickstart (relay + Asset Hub)

Ask:

> Start a local Paseo network with Asset Hub, query a couple of storage values (like balances constants and system events), then submit a small balance transfer and show the resulting events.

### Example 3: Clean shutdown

Ask:

> Clean up any nodes and networks you started.

## References

- Pop MCP repo: `https://github.com/r0gue-io/pop-mcp`
- MCP spec: `https://modelcontextprotocol.io`
