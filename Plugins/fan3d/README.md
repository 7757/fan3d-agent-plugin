# Fan3D Agent Plugin

Create, inspect, edit, preview, validate, and render local Fan3D projects from
Codex or WorkBuddy. Both hosts load the same Fan3D Skills and start the same
local stdio MCP server; all project mutations and rendering remain owned by the
native Fan3D application.

## Requirements

- macOS with `Fan3D.app` installed at `/Applications/Fan3D.app`.
- A supported Agent host with the Fan3D plugin installed and enabled.
- Fan3D authentication and resource access when a requested operation requires
  them. Agent-host credentials never replace Fan3D authorization.

## WorkBuddy

Add the `7757/fan3d-agent-plugin` GitHub repository as a third-party WorkBuddy
marketplace, install `fan3d@fan3d`, and start a new conversation after
installation or update. WorkBuddy discovers the shared Skills and starts the
bundled `fan3d` MCP entry automatically.

For local development, load the repository copy without installing it:

```bash
codebuddy --plugin-dir /absolute/path/to/Plugins/fan3d
```

The development command does not replace the normal marketplace installation
flow used by released versions.

## Usage

Ask the Agent to create a product introduction, list or inspect Fan3D projects,
edit a project, render a preview, validate a scene, or submit a final output.
The Agent must use the connected server's live tool catalog and follow the
Revision, idempotency, authorization, cancellation, and verification rules in
the packaged Skills.

The plugin does not bundle a model, Agent loop, model credentials, or a copy of
Fan3D. It only connects the host to the signed local Helper shipped inside the
installed Fan3D app.
