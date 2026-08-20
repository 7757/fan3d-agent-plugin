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
  `fan3d.catalog.camera_motion_presets.list`, and
  `fan3d.catalog.background_audio.list`
- Resource access: `fan3d.resource_access.preflight`
- Device: `fan3d.scene.device.add`, `fan3d.scene.device.replace`,
  `fan3d.scene.device.duplicate`, `fan3d.scene.device.remove`,
  `fan3d.scene.device_transform.update`,
  `fan3d.scene.device_articulation.update`,
  `fan3d.scene.device_color.update`, `fan3d.scene.device_appearance.update`, and
  `fan3d.scene.device_screen.orientation.update`
- Scene: `fan3d.scene.background.update`,
  `fan3d.scene.environment.update`, `fan3d.scene.camera.update`,
  `fan3d.scene.camera.reset`, `fan3d.scene.camera.focus_blur`,
  `fan3d.scene.labels.update`, `fan3d.scene.labels.clear`, and
  `fan3d.scene.labels.reset_placement`
- Background audio: `fan3d.scene.background_audio.install`,
  `fan3d.scene.background_audio.update`, and
  `fan3d.scene.background_audio.remove`
- Timeline: `fan3d.scene.timeline.update_duration`,
  `fan3d.scene.camera_animation.create_at_time`,
  `fan3d.scene.camera_animation.update_endpoints`,
  `fan3d.scene.device_articulation_animation.update_endpoints`,
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

On a project-scoped server, the open project is already bound and `projectID`
is optional. Omit it normally; if supplied, it must match the bound project.
Start with `fan3d.project.inspect`; do not switch to a different project or
launch a separate global server behind the user's back.

## Edit safely

1. Inspect the project before editing.
   - Read the current revision and stable IDs for devices, camera animation
     clips, device-articulation animation clips, screens, and other targets.
   - For an articulation edit, read the target device's current
     `articulationValues` and any matching
     `deviceArticulationAnimationClips`.
   - Do not infer a target from hover state, UI selection, or visual position.
2. Query the relevant catalog before selecting a device or preset.
   - Use returned stable IDs. Do not guess catalog identifiers.
   - Each device catalog entry includes an account-scoped `access` snapshot.
     Follow that state before writing; catalog discovery and access planning
     complete in the same read.
   - For an articulation edit, use the selected device catalog resource's
     `articulations` entry to discover its ID, kind, minimum, maximum, and
     default value. Values use the catalog's declared unit; for example,
     `rotationRadians` values are radians.
3. Resolve resource access before a consuming write.
   - For a device, use the account-scoped `access` object returned on its
     catalog entry; do not issue a redundant preflight when that state is
     current and well formed.
   - Templates and other catalog-backed resources without embedded access must
     still use `fan3d.resource_access.preflight` with the exact operation.
   - Use explicit preflight when a workflow requires an operation-specific
     recheck or after the user has resolved an `UNVERIFIED` state.
   - Follow the access-state branches below before writing.
4. Send one bounded intent at a time.
   - Pass the inspected `expectedRevision` and a fresh `requestID`.
   - Use the normalized result and returned revision for the next call.
5. Verify the finished scene.
   - Render previews at representative explicit times.
   - Run scene validation before final output.
6. Submit final output only after verification.
   - Retain the returned `jobID` and poll `fan3d.render.status` at a reasonable
     interval until it reaches a terminal state.
   - Report an output URI only after the job succeeds.

## Resource access

The connected Fan3D server, not this Skill, decides whether the current user
may consume a resource. Device catalog access is an account-scoped,
point-in-time planning snapshot. Never hard-code free device IDs, Creator
rules, or availability assumptions into a plan.

Follow the `access.eligibility` on a device entry, or `eligibility` from an
explicit preflight, as follows:

- `AUTHORIZED`: proceed with the intended write using the catalog-returned ID.
- `CLAIMABLE`: stop before the consuming write. A conversational request, user
  statement, generic tool approval, or client user-input response is not the
  native account-entitlement confirmation. Ask the user briefly to choose the
  named resource in Fan3D and complete its native claim flow. Afterward,
  re-query the catalog; proceed only when Fan3D returns `AUTHORIZED` or an
  existing permanent grant. Never claim through an unrelated operation.
- `MEMBERSHIP_REQUIRED`: stop before the project write. If `recoveryAction` is
  `UPGRADE_CREATOR`, say only that the resource requires Creator and ask the
  user to upgrade in Fan3D. If it is `RENEW_CREATOR`, say only that Creator has
  expired and ask the user to renew in Fan3D. Do not recite internal policy,
  retry, downgrade, or substitute unless the user asks for alternatives.
- `UNAVAILABLE`: stop before the project write. Re-query the catalog only when
  the result recommends it; otherwise ask the user to choose another resource.
- `UNVERIFIED`: stop before the project write. Explain that Fan3D could not
  verify the current account or catalog state, and follow the returned reason
  or recovery guidance. Never describe this state as free, paid, or eligible.
- Any unknown or malformed state: fail closed and report that authorization
  could not be verified.

A catalog access snapshot or preflight result is a point-in-time check. The
resource-consuming write must authorize again and its structured result is
final if policy, membership, or catalog state changed between calls. Never
bypass either result through UI automation, direct package edits, a guessed
local resource path, or a user's unsupported assertion that a local download
or purchase proves authorization.

## Add background music

- List `fan3d.catalog.background_audio.list` before choosing a built-in track;
  never guess an audio ID.
- Background-music selection belongs to the user unless they explicitly ask
  the Agent to choose. If neither a category nor track was specified, present
  the returned categories and ask the user to choose one; never select the
  first category by catalog order. Once a category is known, present its
  returned tracks and ask which one to use unless the user already named a
  track or delegated the choice; never silently select the first track.
- Install one catalog track with
  `fan3d.scene.background_audio.install`. Fan3D copies it into the project,
  starts it at zero, and loops it through Movie output.
- Use `fan3d.scene.background_audio.update` for the bounded `volume` and
  `muted` state. Use `fan3d.scene.background_audio.remove` to remove it.
- Fan3D currently supports one background-music track. It does not expose
  system audio, recording, narration, TTS, or arbitrary local-file paths to
  MCP. Ask the user to use Fan3D's native file picker for their own audio.
- Still images and image sequences do not contain audio. Movie output includes
  the current audible background track.

## Edit device articulations

Fan3D exposes bounded scalar articulations declared by an installed device,
such as a laptop `lid`. The tools do not expose model nodes or accept guessed
articulation ranges.

- To change a static authored value, call
  `fan3d.scene.device_articulation.update` with the inspected instance ID,
  its expected device ID, a catalog-returned articulation ID, and a value
  inside the catalog range.
- If the inspected project contains an existing matching articulation clip,
  use `fan3d.scene.device_articulation_animation.update_endpoints` to change
  one endpoint, or two endpoints that share the same timeline boundary. Each
  update must repeat the inspected clip ID, target instance ID, articulation
  ID, endpoint (`start` or `end`), and new value.
- The endpoint tool only edits existing clips. It cannot create, move, resize,
  or remove an articulation clip. If no suitable clip exists, report that
  animated articulation authoring is unavailable through the published MCP
  tools; do not simulate it, edit the project package, or substitute a camera
  animation tool.
- A static articulation update changes the device's authored base value; it
  does not create an animation or replace values evaluated from an existing
  articulation clip. Inspect the timeline first and use the endpoint tool when
  the user's intent targets an existing animation boundary.

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

## Structured error handling

- After any tool error, stop the current sequence and read its `code`,
  `retryable`, `projectUnchanged`, `currentRevision`, and
  `recoverySuggestion` fields when present.
- On `membership_required`, leave the project unchanged and wait for the user
  to activate Creator or select another resource. Do not loop retries.
- On `resource_access_denied`, retry only when `retryable` is true and only
  after following the recovery suggestion and re-running catalog discovery and
  access resolution. Use explicit preflight only when the refreshed state or
  operation requires it. A re-planned write uses a fresh `requestID`.
- If `projectUnchanged` is false or absent, inspect the project before doing
  anything else. If it is true, do not claim a mutation occurred.
- Never turn an authorization or capability error into a filesystem edit, UI
  automation fallback, or silent resource substitution.

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
3. Read the selected iPhone's account-scoped catalog access and follow its
   access-state branch; use explicit preflight only when the state requires a
   recheck.
4. Add or update the authorized catalog-backed iPhone device as needed.
5. Apply a catalog-backed blue gradient background.
6. Set the timeline duration to eight seconds.
7. Inspect existing camera clips, then resize one or insert a catalog motion
   preset. Do not create overlapping clips.
8. Preview representative times such as 0, 4, and 8 seconds, then validate.
9. Submit a render job and poll its status to completion.

If any required tool is absent, say exactly which capability is unavailable
and stop before claiming that the scene or output changed.

## Security and completion

- Treat structured tool errors as authoritative.
- In routine progress and final responses, use product language such as
  “reading available devices” and “adding the device.” Do not expose this
  Skill or Plugin, an MCP server, raw tool names, protocol fields, policy
  codes, or error codes unless the user explicitly asks for technical
  diagnostics.
- Never ask for, read, expose, or log Fan3D session credentials. The signed
  Helper reuses its Keychain session only inside the trusted Fan3D process.
- Never expose credentials, user media contents, hidden reasoning, or raw
  project JSON.
- Do not substitute arbitrary filesystem paths for a missing authorized file
  reference.
- Do not claim success until the tool result confirms it. Summarize the final
  revision, normalized result, warnings, validation outcome, job status, and
  output URI when available.
