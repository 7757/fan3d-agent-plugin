<p align="center">
  <img src="Plugins/fan3d/assets/logo.png" width="128" height="128" alt="Fan3D logo">
</p>

# Fan3D Agent Plugin

The official Agent Plugins 1.0 package for
[Fan3D](https://3d.fandcode.com/), a native macOS 3D animation editor from
Zhenjiang Xingfan Technology Co.,Ltd.

The plugin gives Codex and WorkBuddy deterministic local tools to understand a
product, create or edit its Fan3D presentation, preview and validate the scene,
and render the result. Both hosts load the same Skills and start the same local
MCP Helper; the package does not ship the Fan3D application source code.

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

## Add to WorkBuddy

Add `https://github.com/7757/fan3d-agent-plugin` as a third-party plugin
marketplace, install `fan3d@fan3d`, then fully restart WorkBuddy and start a new
conversation. WorkBuddy discovers the packaged Skills and MCP configuration
automatically.

The equivalent public CLI flow is:

```sh
codebuddy plugin marketplace add https://github.com/7757/fan3d-agent-plugin
codebuddy plugin install fan3d@fan3d -s user
```

After a plugin update, restart the host before validating the newly installed
version. The installed `/Applications/Fan3D.app` must include a compatible MCP
Helper; updating this repository does not update the native application.

## Brand assets

The bundled Fan3D logo and app icon remain proprietary brand assets. See
[the brand-assets notice](Plugins/fan3d/BRAND_ASSETS_NOTICE.md) for details.
