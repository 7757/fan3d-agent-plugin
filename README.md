<p align="center">
  <img src="Plugins/fan3d/assets/logo.png" width="128" height="128" alt="Fan3D logo">
</p>

# Fan3D Agent Plugin

The official Agent Plugins 1.0 package for
[Fan3D](https://3d.fandcode.com/), a native macOS 3D animation editor from
Zhenjiang Xingfan Technology Co.,Ltd.

The plugin gives Codex deterministic local tools to inspect, edit, preview,
validate, and render Fan3D projects. It contains the MCP launcher and animation
workflow guidance without shipping the Fan3D application source code.

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

## Brand assets

The bundled Fan3D logo and app icon remain proprietary brand assets. See
[the brand-assets notice](Plugins/fan3d/BRAND_ASSETS_NOTICE.md) for details.
