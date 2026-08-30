---
name: fan3d-animation
description: Use when a user wants to create, inspect, edit, preview, validate, or render a local Fan3D 3D animation project with Fan3D MCP tools. For any product-introduction goal, use this alongside fan3d-product-introduction, which owns product truth and creative direction while this Skill owns safe project operations.
---

# Fan3D animation workflow

Fan3D is a deterministic local 3D editor and renderer. Fan3D owns project
transactions, scene evaluation, and rendering. The external Agent owns intent
understanding, planning, and the tool-use loop.

When a request concerns product positioning, introduction copy, a showcase,
launch visual, app demo, 3D mockup, product video, or creative review, apply
`fan3d-product-introduction` first for product grounding and direction, then
use this Skill for every Fan3D read, write, preview, validation, and render.
Use this Skill alone for an isolated exact project operation that requires no
product-introduction judgment.

The plugin launcher expects the signed `Fan3D.app` in `/Applications`. If the
server cannot start, ask the user to install or move Fan3D there. Do not search
for or execute an arbitrary replacement binary.

## Discover capabilities

Treat the connected server's `tools/list` as authoritative. Never simulate a
missing capability through UI automation, edit a `.fan3d` package directly, or
invent a tool that is not published.

Published tools are grouped by intent:

- Project: `fan3d.project.list`, `fan3d.project.create`,
  `fan3d.project.import_copy`,
  `fan3d.project.create_from_device`,
  `fan3d.project.create_from_device.inspect`,
  `fan3d.project.create_from_composition`,
  `fan3d.project.create_from_template`, and `fan3d.project.inspect`
- Installation preferences: `fan3d.preferences.inspect` and
  `fan3d.preferences.update`
- Local storage: `fan3d.storage.inspect` and
  `fan3d.storage.preview_cache.clear`
- Catalogs: `fan3d.catalog.devices.list`,
  `fan3d.catalog.project_templates.list`,
  `fan3d.catalog.compositions.list`,
  `fan3d.catalog.environment_presets.list`,
  `fan3d.catalog.gradient_presets.list`,
  `fan3d.catalog.camera_motion_presets.list`,
  `fan3d.catalog.directional_label_fonts.list`,
  `fan3d.catalog.named_system_colors.list`,
  `fan3d.catalog.render_delivery_presets.list`,
  `fan3d.catalog.render_output_presets.list`,
  `fan3d.catalog.background_audio.list`,
  `fan3d.catalog.background_audio.user_library.list`,
  `fan3d.catalog.wallpapers.list`,
  `fan3d.catalog.wallpapers.pick_random`,
  `fan3d.catalog.wallpapers.favorites.list`, and
  `fan3d.catalog.wallpapers.favorite.set`
- Resource access: `fan3d.resource_access.preflight`
- Device: `fan3d.scene.device.add`, `fan3d.scene.device.replace`,
  `fan3d.scene.device.duplicate`, `fan3d.scene.device.remove`,
  `fan3d.scene.device_transform.update`,
  `fan3d.scene.device_articulation.update`,
  `fan3d.scene.device_color.update`,
  `fan3d.scene.device_appearance.update`,
  `fan3d.scene.device_layout.apply`, and
  `fan3d.scene.composition.apply`
- Device screen: `fan3d.scene.device_screen.orientation.update`,
  `fan3d.scene.device_screen.media.import`,
  `fan3d.scene.device_screen.media.clear`,
  `fan3d.scene.device_screen.content_mode.update`,
  `fan3d.scene.device_screen.video.inspect`,
  `fan3d.scene.device_screen.video_trim.update`, and
  `fan3d.scene.device_screen.video_audio_volume.update`
- Scene: `fan3d.scene.background.update`,
  `fan3d.scene.background.media.import`,
  `fan3d.scene.background.wallpaper.apply`,
  `fan3d.scene.environment.image.import`,
  `fan3d.scene.environment.update`, `fan3d.scene.camera.update`,
  `fan3d.scene.camera.reset`, `fan3d.scene.camera.focus_blur`,
  `fan3d.scene.labels.update`, `fan3d.scene.labels.clear`, and
  `fan3d.scene.labels.reset_placement`
- Background audio: `fan3d.scene.background_audio.install`,
  `fan3d.scene.background_audio.user_library.install`,
  `fan3d.scene.background_audio.import`,
  `fan3d.scene.background_audio.update`, and
  `fan3d.scene.background_audio.remove`
- Timeline: `fan3d.scene.timeline.update_duration`,
  `fan3d.scene.camera_animation.create_at_time`,
  `fan3d.scene.camera_animation.update_endpoints`,
  `fan3d.scene.device_articulation_animation.update_endpoints`,
  `fan3d.scene.camera_animation.move`,
  `fan3d.scene.camera_animation.resize`,
  `fan3d.scene.camera_animation.update_timing`,
  `fan3d.scene.camera_animation.rename`,
  `fan3d.scene.camera_animation.remove`,
  `fan3d.scene.camera_animation.insert_preset`, and
  `fan3d.scene.label_animation.update_endpoints`
- Verification: `fan3d.preview.render` and `fan3d.scene.validate`
- Output jobs: `fan3d.render.delivery_preset.resolve`,
  `fan3d.render.capability.check`, `fan3d.render.submit`,
  `fan3d.render.status`, `fan3d.render.cancel`, and `fan3d.render.retry`
- Current-workspace controls, only when the in-app project-scoped server
  publishes them: `fan3d.workspace.playback.set` and
  `fan3d.workspace.history.navigate`

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

When the user requests a new project from a device, list the device catalog,
inspect the selected device's authored canvas/camera/timeline seed with
`fan3d.project.create_from_device.inspect`, then use
`fan3d.project.create_from_device` with the catalog-returned device, variant,
and color IDs. For an authored Dashboard composition, list
`fan3d.catalog.compositions.list`, preflight it with `kind=device` and
`operation=CREATE_PROJECT`, then use
`fan3d.project.create_from_composition`; this preserves its authored canvas and
seed animations. For a Dashboard project template, list
`fan3d.catalog.project_templates.list`, preflight the chosen template for
`kind=template` and `operation=CREATE_FROM_TEMPLATE`, then call
`fan3d.project.create_from_template` with its
stable ID. Use `fan3d.project.create` only for an explicitly blank project with
a user-specified canvas. All project-creation tools are global-only.

Use `fan3d.project.import_copy` only for an explicit absolute local `file:` URL
to a `.fan3d` package supplied by the user or calling client. It creates or
returns an independent library copy and never modifies the source. Never
search for, enumerate, or guess a project package path.

## Manage installation preferences and storage

- These installation-scoped tools are available only on the global server;
  they do not mutate `SceneDocument` or switch the current project.
- Inspect preferences before updating them. Pass the returned revision as
  `expectedRevision`, preserve the desired complete supported preference
  state, and use a fresh `requestID`.
- Inspect storage to report bounded aggregate counts and byte totals. It never
  returns local paths or user-media contents.
- Clear preview caches only when the user asks to reclaim or clear cache data.
  This operation may remove only Fan3D-owned regenerable previews; it must not
  be described as deleting projects, source assets, or render outputs.

## Ask for bounded choices

- When the next required step has two or more current, authoritative, finite,
  user-meaningful alternatives and the user has neither selected one nor
  delegated the choice, ask with the host's structured-choice capability when
  it is available. Keep the task active and continue it after the answer.
- This applies to project or object disambiguation, devices and other catalog
  resources, project templates, compositions, installed fonts, layout modes,
  presets, background-audio categories and tracks, output variants, and
  equivalent bounded decisions. Background audio is an example, not a special
  interaction path.
- Read the relevant project state or catalog before asking. Show concise human
  labels and differentiating descriptions while retaining the returned stable
  IDs for later tool calls. Never present guessed or stale choices.
- Ask dependent choices in order. For a large result set, narrow it with the
  published query, pagination, or a higher-level structured choice before
  presenting individual items.
- If structured choice is unavailable or cannot represent the bounded result,
  ask one brief prose question. Do not silently select the first item.
- Do not use an ordinary choice as native permission, entitlement claim,
  destructive-operation approval, or project-external file confirmation.
  Open-ended creative direction may remain conversational.

## Resolve creative choices

- Treat natural-language requests for a canvas background, lighting
  environment, background music, device appearance, screen media, camera
  treatment, animation, labels, timeline, or output as editable Fan3D intent.
  Do not require the user to know inspector labels or tool terminology.
- Read [creative choices](references/creative-choices.md) when the user asks
  what is available, names one of those broad creative areas without an exact
  value, or delegates a choice such as “random,” “any,” or “you choose.”
- Discover live candidates before answering. If the user delegates the choice,
  select from those candidates and continue the requested edit; do not ask them
  to choose again or stop after merely listing options.
- When the user asks generally what can be changed, proactively present the
  relevant top-level families from the reference, then query only the family
  they choose. Do not require knowledge of Fan3D inspector names.
- For a named GUI color or a request to browse Apple, System, Crayons, or Web
  Safe colors, query `fan3d.catalog.named_system_colors.list` for the desired
  light/dark appearance and group. Apply its explicit normalized RGBA values
  through the relevant scene mutation; the catalog color ID itself is not
  persistent scene state.
- “macOS” under Fan3D's Wallpaper interface is a bundled wallpaper category,
  not a request to inspect the user's Mac desktop settings. A request such as
  “choose any macOS wallpaper for the canvas” must list that category, resolve
  one returned wallpaper ID, apply it, and verify the project.

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
   - Preflight a project template with `kind=template` and
     `operation=CREATE_FROM_TEMPLATE`. For an authored composition use
     `kind=device` with `operation=CREATE_PROJECT` when creating a new project,
     or `operation=APPLY_RESOURCE` when replacing the current scene.
   - For any other catalog family, use explicit preflight only when the live
     descriptor requires it and its schema exposes a legal kind/operation
     pair. Otherwise rely on the consuming mutation's own final validation and
     authorization instead of inventing unsupported preflight enums.
   - Use explicit preflight when a workflow requires an operation-specific
     recheck or after the user has resolved an `UNVERIFIED` state.
   - Follow the access-state branches below before writing.
4. Send one bounded intent at a time.
   - Pass the inspected `expectedRevision` and a fresh `requestID`.
   - Use the normalized result and returned revision for the next call.
   - `fan3d.scene.device_appearance.update` replaces the complete scene-wide
     device appearance, not one device instance. Inspect and preserve every
     unrequested appearance value; never route a per-device color or material
     request through this operation.
   - `fan3d.scene.camera.focus_blur` is not a field-only toggle. It resets the
     authored camera and supported active-camera animation endpoints to
     canonical initial focus values. Inspect that camera and its active clips,
     plan against the reset, and do not promise that unrelated authored focus
     state will remain unchanged.
5. Verify the finished scene.
   - Render previews at representative explicit times.
   - Run scene validation before final output.
6. Submit final output only after verification.
   - Check the exact intended output settings with
     `fan3d.render.capability.check` at the inspected Revision before submit.
   - Retain the returned `jobID` and poll `fan3d.render.status` at a reasonable
     interval until it reaches a terminal state.
   - Report an output URI only after the job succeeds.

## Edit a device screen

- Inspect the project before every device-screen workflow. Use the returned
  `instanceID`, `deviceID`, screen-slot ID, and current media asset ID; never
  infer a target from UI selection or substitute an ID from another device.
- Use `fan3d.scene.device_screen.media.import` to add or replace an image or
  video. Pass `sourceFileURL` as an absolute local `file:` URL only when the
  user or calling client explicitly provided that file for the current task.
  Preserve the inspected orientation policy and resolved orientation as the
  automatic fallback when replacing media. Never guess a path, browse the
  filesystem for a likely file, or silently reuse an unrelated path.
- Use `fan3d.scene.device_screen.orientation.update` for automatic, portrait,
  or landscape orientation. Use
  `fan3d.scene.device_screen.content_mode.update` for `fit`, `fill`, or
  `stretch`; preserve the inspected media asset ID when changing an existing
  screen.
- Before editing a video, call
  `fan3d.scene.device_screen.video.inspect` with the explicit device, slot,
  and media identities. Use its source duration and canonical trim state when
  calculating an in/out range. Apply that range with
  `fan3d.scene.device_screen.video_trim.update`; clear the trim only when the
  user wants the complete source video restored.
- A device-screen video always starts at project time zero. Trimming selects
  source time but cannot move the video later on the project timeline or leave
  an empty lead-in.
- Use `fan3d.scene.device_screen.video_audio_volume.update` only for the
  imported video's original audio. Its linear range is zero through one:
  `0` is silent and `1` is the original level. Fan3D does not amplify this
  audio above its original level.
- Use `fan3d.scene.device_screen.media.clear` with the inspected media identity
  to remove the imported screen asset and restore the device's embedded empty
  screen. Do not use it to remove project background music.
- Device-video original audio and project background music are independent.
  The `fan3d.scene.background_audio.*` tools manage the separate project-wide
  music track; changing either one must not be described as changing the
  other.

## Use templates, compositions, and layouts

- A Dashboard project template creates a new independent library project. List
  `fan3d.catalog.project_templates.list`, retain the stable template ID,
  preflight it with `kind=template` and `operation=CREATE_FROM_TEMPLATE`, and use
  `fan3d.project.create_from_template` only on a global server. Never describe
  this as changing the currently open project.
- An authored composition has two distinct consuming actions. To create a new
  project on a global server, preflight it as a `device` resource for
  `CREATE_PROJECT` and use `fan3d.project.create_from_composition`; this keeps
  the authored canvas and seed animations. To replace the current project's
  complete device arrangement and active camera, preflight it for
  `APPLY_RESOURCE` and use `fan3d.scene.composition.apply`. Applying to an
  existing project also removes superseded device-transform and
  camera-animation clips, so do not infer that destructive choice from a vague
  request for “a layout.”
- A device layout keeps the current devices and resolves only their transforms.
  Use `fan3d.scene.device_layout.apply` for Across, Down, Grid, or Radial. Read
  the live discriminated schema before constructing Grid or Radial settings;
  preserve complete vectors and use the schema's units and ranges.
- If the user says “template,” distinguish a new-project Dashboard template
  from an in-project authored composition. If they say “arrange these devices,”
  prefer layout unless they explicitly ask to replace the scene with a catalog
  composition.

## Edit labels and label animation

- Directional labels have eight explicit slots. Before selecting a font, query
  `fan3d.catalog.directional_label_fonts.list` and pass its returned exact
  `postScriptName` as `fontPostScriptName`; do not guess an installed font or
  infer one from a file.
- Preserve the complete inspected label set when changing text, font, color,
  style, or static placement. Use the dedicated clear/reset operation only for
  the matching explicit intent.
- Existing camera clips carry complete start/end label placements. Use
  `fan3d.scene.label_animation.update_endpoints` only for one or two explicit
  X/Y endpoint changes on one or two inspected clips for the same camera; each
  `(clipID, endpoint)` must be unique. It does not create a clip or change label
  content, style, levitation, camera state, or timing.

## Adjust camera-animation timing

- Inspect the target camera clip first. Its summary reports complete start/end
  camera state, start/end label placement, `durationSeconds`, named `easing`,
  and `jumpCut`; never reconstruct omitted endpoints from the current playhead.
- Use `fan3d.scene.camera_animation.rename` for a display-name-only change. Use
  `fan3d.scene.camera_animation.update_endpoints` for camera state, and the
  separate label endpoint tool for animated label X/Y placement. Preserve the
  inspected complete endpoint values that the user did not ask to change.
- Use `fan3d.scene.camera_animation.update_timing` to change one or more of
  those values. Duration is bounded to 0.1 through 10 seconds. Omitted fields
  remain unchanged.
- Named easing values are `jump`, `default`, `linear`, `in`, `out`, `in_out`,
  and the `in_*`, `out_*`, or `in_out_*` variants for `sine`, `quad`, `cubic`,
  `quart`, `quint`, `expo`, `circ`, and `back`.
- The named `jump` easing and `jumpCut` are synchronized with Fan3D's timing
  semantics.
  Selecting Jump or enabling Jump Cut produces no interpolated motion.
  Selecting another easing clears Jump Cut; disabling Jump Cut restores
  Default when Jump was selected.
- A duration increase may expand the canvas to contain the clip. Fan3D rejects
  a timing update that would overlap another camera clip; inspect and re-plan
  instead of moving another clip without the user's intent.

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
  the Agent to choose. Category and track are dependent bounded choices under
  the general policy above; never select the first category or track by catalog
  order.
- Install one catalog track with
  `fan3d.scene.background_audio.install`. Fan3D copies it into the project,
  starts it at zero, and loops it through Movie output.
- Prefer catalog discovery and installation for Fan3D's bundled tracks. Use
  `fan3d.scene.background_audio.import` for a local track only when the user or
  calling client explicitly supplied its absolute `file:` URL for the current
  task. Set its initial `volume` and `muted` state explicitly. Never search,
  guess, enumerate, or derive local paths to find audio.
- Use `fan3d.scene.background_audio.update` for the bounded `volume` and
  `muted` state. Use `fan3d.scene.background_audio.remove` to remove it.
- Fan3D currently supports one background-music track. Local import does not
  provide system-audio discovery, recording, narration, or TTS.
- Still images and image sequences do not contain audio. Movie output includes
  the current audible background track.

## Control the current workspace

- Treat requests to play, pause, or preview the already-open workspace as
  playback intent. Use `fan3d.workspace.playback.set` only when it is published.
  Never submit a Render Job merely to start workspace playback.
- Treat undo, revert the last step, and redo as History intent. Inspect the
  current Revision, then use `fan3d.workspace.history.navigate` only when it is
  published. Report a no-op result honestly when no matching History step
  exists.
- These controls are App-session capabilities and may be absent from a global
  or external Plugin connection. If absent, state that the current connection
  cannot perform the control and stop; do not simulate a click, press a key,
  edit the project package, or export as a workaround.

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
- For a friendly movie-delivery request, first query
  `fan3d.catalog.render_delivery_presets.list`. Treat equivalent platform names
  as hints for one shared purpose instead of presenting duplicate platforms;
  for example, Douyin and TikTok both select the short-video purpose. Retain a
  returned preset ID, clarity ID, and frame rate. Layout controls aspect ratio;
  clarity controls output pixels by setting the shorter edge to 720, 1,080, or
  2,160 pixels. Frame rate controls motion sampling and does not make each
  frame sharper. If the user delegated the choice, use the target's default
  preset, `defaultClarityID` (`ultra_hd_4k`), and the catalog's default frame rate
  without asking again.
- Resolve that choice with `fan3d.render.delivery_preset.resolve` at the
  inspected Revision. It reads the project's canvas, duration, and transparency
  itself. Pass the selected `presetID`, `clarityID`, and `frameRateFPS`; then
  pass its returned movie settings unchanged, and its returned Revision, to
  capability and submit. Listing and resolving are read-only and do not create
  a Render Job. If transparent output is rejected, do not hide the transparency
  or retry with a guessed value; move to an explicit advanced output choice
  instead.
- Before submission, call `fan3d.render.capability.check` with the inspected
  Revision and the exact intended settings. It performs no destination write
  and creates no job. If `isSupported` is false, retain its normalized output
  and reason while asking for or resolving a different bounded choice; do not
  submit the same unsupported settings blindly.
- When the user asks for an exact custom size, a device-named preset such as
  Mac, Apple TV, or Apple Watch, or non-movie output such as a static image,
  image sequence, or Web, query
  `fan3d.catalog.render_output_presets.list`. Retain its human label, read the
  selected preset's live `sizingMode`, and resolve dimensions as follows:
  - For `fixed`, use the returned `fixedSize.width` and `fixedSize.height`.
  - For `aspect_ratio`, make the dependent resolution choice and use the
    matching `resolvedSizes` entry's width and height.
  - For `custom`, obtain explicit supported width and height from the user or
    their delegated delivery choice.
  Pass only the resolved width and height—not preset or resolution IDs—to
  capability and submit settings.
- Final still output uses settings kind `image` with an explicit project-time
  `timeSeconds` and `stillImageFormat` of `png`, `jpg`, `gif`, or `tiff`.
  PNG/TIFF can retain transparent output; JPG/GIF are rejected for a
  transparent scene by the shared capability validation. Still images do not
  use frame rate or jittering and contain no audio.
- Final animation watermarking is owned by the render Application Use Case. It
  freezes the account entitlement and output kind at submission. Movie output
  uses a digital watermark; Free or offline-fallback movies also carry the
  visible `fan3d` mark. Image-sequence and Web output do not use the digital
  watermark: active Creator output has no watermark, while Free or offline
  fallback bakes the visible `fan3d` mark into every output frame and remains
  personal-non-commercial. Never ask for, promise, or attempt a watermark
  bypass through project edits, output kinds, or output paths.
- A `resource_fallback_applied` warning at `render_output.license` means the
  server grant was unavailable and Fan3D safely used the local Free license.
  Preserve and report that warning without exposing account or watermark IDs.
- Only `fan3d.render.cancel` requests cancellation. Cancelling a status call or
  stopping the Agent does not cancel the job.
- Use `fan3d.render.retry` only when the failed or interrupted job reports that
  it is retryable. Retry creates a new job linked to the original.
- MCP output uses Fan3D's managed, non-overwriting destination. Do not invent
  an arbitrary output path, claim that submit accepts one, or expose a managed
  local path. Report the final artifact URI only after status succeeds.

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
9. Check the intended output capability, submit the supported render job, and
   poll its status to completion.

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
