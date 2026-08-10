# Fan3D Agent Plugin

Fan3D's Agent Plugins 1.0 package for Codex. It exposes the local Fan3D MCP
server and bundled animation guidance without shipping the Fan3D application
source code.

## Requirements

- macOS
- `Fan3D.app` installed in `/Applications`

## Add to Codex

In **Add plugin marketplace**, use:

- Source: `7757/fan3d-agent-plugin`
- Git ref: `main`
- Sparse paths: leave empty

Or use the CLI:

```sh
codex plugin marketplace add 7757/fan3d-agent-plugin
codex plugin add fan3d@fan3d
```

The marketplace index points to `./Plugins/fan3d`. The plugin launcher invokes
`/Applications/Fan3D.app/Contents/Helpers/fan3d mcp serve`.
