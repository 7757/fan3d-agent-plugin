---
name: fan3d-animation
description: Use when a user wants to create, inspect, edit, preview, validate, or render a local Fan3D 3D animation project with Fan3D MCP tools.
---

# Fan3D animation workflow

Fan3D is a deterministic local 3D editor and renderer. Fan3D owns project
transactions, scene evaluation, and rendering. The external Agent owns intent
understanding, planning, and the tool-use loop.

The plugin launcher expects the signed `Fan3D.app` in `/Applications`. If the
server cannot start, ask the user to install or move Fan3D there. Do not search
for or execute an arbitrary replacement binary.

## Discover capabilities

Treat the connected server's `tools/list` as authoritative. Never simulate a
missing capability through UI automation, edit a `.fan3d` package directly, or
invent a tool that is not published.

Published tools are grouped by intent:

- Project: `fan3d.project.list`, `fan3d.project.create`,
  `fan3d.project.inspect`
- Catalogs: `fan3d.catalog.devices.list`,
  `fan3d.catalog.environment_presets.list`,
  `fan3d.catalog.gradient_presets.list`, and
  `fan3d.catalog.camera_motion_presets.list`
- Device: `fan3d.scene.device.add`, `fan3d.scene.device.replace`,
  `fan3d.scene.device.duplicate`, `fan3d.scene.device.remove`,
  `fan3d.scene.device_transform.update`, `fan3d.scene.device_color.update`,
  `fan3d.scene.device_appearance.update`, and
  `fan3d.scene.device_screen.orientation.update`
- Scene: `fan3d.scene.background.update`,
  `fan3d.scene.environment.update`, `fan3d.scene.camera.update`,
  `fan3d.scene.camera.reset`, `fan3d.scene.camera.focus_blur`,
  `fan3d.scene.labels.update`, `fan3d.scene.labels.clear`, and
  `fan3d.scene.labels.reset_placement`
- Timeline: `fan3d.scene.timeline.update_duration`,
  `fan3d.scene.camera_animation.create_at_time`,
  `fan3d.scene.camera_animation.update_endpoints`,
  `fan3d.scene.camera_animation.move`,
  `fan3d.scene.camera_animation.resize`,
  `fan3d.scene.camera_animation.remove`, and
  `fan3d.scene.camera_animation.insert_preset`
- Verification: `fan3d.preview.render` and `fan3d.scene.validate`
- Output jobs: `fan3d.render.submit`, `fan3d.render.status`,
  `fan3d.render.cancel`, and `fan3d.render.retry`

`fan3d.project.create` is intentionally unavailable in an in-app,
project-scoped server. `fan3d.project.list` is also global-only. In a
project-scoped server, operate only on the bound open project.

## Discover the project

On a global server, never guess a project ID or search `.fan3d` packages on
disk. Call `fan3d.project.list` with a bounded `limit` and an optional `query`,
then select the explicit `projectID` returned by the tool. If `hasMore` is true,
pass `nextCursor` with the same query; do not edit or decode the opaque cursor.
Catalog changes can invalidate a cursor, so restart discovery without a cursor
when the tool reports a stale cursor. Then call `fan3d.project.inspect` for the
selected project before planning any write.

On a project-scoped server, the open project is already bound and the input
schemas omit `projectID`. Start with `fan3d.project.inspect`; do not switch to a
different project or launch a separate global server behind the user's back.

## Edit safely

1. Inspect the project before editing.
   - Read the current revision and stable IDs for devices, camera animation
     clips, screens, and other targets.
   - Do not infer a target from hover state, UI selection, or visual position.
2. Query the relevant catalog before selecting a device or preset.
   - Use returned stable IDs. Do not guess catalog identifiers.
3. Send one bounded intent at a time.
   - Pass the inspected `expectedRevision` and a fresh `requestID`.
   - Use the normalized result and returned revision for the next call.
4. Verify the finished scene.
   - Render previews at representative explicit times.
   - Run scene validation before final output.
5. Submit final output only after verification.
   - Retain the returned `jobID` and poll `fan3d.render.status` at a reasonable
     interval until it reaches a terminal state.
   - Report an output URI only after the job succeeds.

## Revision and replay rules

- A successful scene write advances the project revision once; a no-op does
  not.
- Reusing the same `requestID` with identical input replays the exact first
  result. Reusing it with different input returns `request_id_reused`.
- Never retry a write blindly. On `revision_conflict`, inspect once and re-plan
  against the current revision with a new request ID.
- Preserve warnings and normalized values returned by each tool.
- A cancelled Agent turn does not undo a committed scene mutation or cancel an
  accepted render job.

## Render job rules

- Render jobs are persistent local records, not MCP-session handles. A later
  compatible Fan3D process can query the same `jobID` for the same project.
- Only `fan3d.render.cancel` requests cancellation. Cancelling a status call or
  stopping the Agent does not cancel the job.
- Use `fan3d.render.retry` only when the failed or interrupted job reports that
  it is retryable. Retry creates a new job linked to the original.
- Do not invent an arbitrary output path or silently overwrite a file. Follow
  the destination and overwrite fields exposed by the actual tool schema.

## Example plan

For “create an eight-second iPhone product video with a blue gradient”:

1. On a global server, discover an existing project with
   `fan3d.project.list`; create one only when the user requested a new project
   and `fan3d.project.create` is published. On a project-scoped server, use the
   current bound project.
2. Inspect the project and list device and gradient catalogs.
3. Add or update the catalog-backed iPhone device as needed.
4. Apply a catalog-backed blue gradient background.
5. Set the timeline duration to eight seconds.
6. Inspect existing camera clips, then resize one or insert a catalog motion
   preset. Do not create overlapping clips.
7. Preview representative times such as 0, 4, and 8 seconds, then validate.
8. Submit a render job and poll its status to completion.

If any required tool is absent, say exactly which capability is unavailable
and stop before claiming that the scene or output changed.

## Security and completion

- Treat structured tool errors as authoritative.
- Never expose credentials, user media contents, hidden reasoning, or raw
  project JSON.
- Do not substitute arbitrary filesystem paths for a missing authorized file
  reference.
- Do not claim success until the tool result confirms it. Summarize the final
  revision, normalized result, warnings, validation outcome, job status, and
  output URI when available.
