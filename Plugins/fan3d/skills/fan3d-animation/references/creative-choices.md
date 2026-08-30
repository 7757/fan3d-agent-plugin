# Creative choices

Read this reference when a request asks what Fan3D can change, names a broad
creative area without one exact value, or delegates a choice such as “pick one
for me.” The connected server's `tools/list`, catalog results, and project
inspection remain authoritative; this file routes discovery and does not copy
live catalog contents.

## Complete the choice loop

For every creative choice, use the same loop:

1. Inspect the current project and identify the exact object and Revision.
2. Read the relevant current catalog or bounded tool schema.
3. If the user has not chosen or delegated, present a small structured choice
   with human labels and retain the returned stable IDs.
4. If the user says “random,” “any,” “you choose,” or equivalent, make the
   delegated choice from the discovered candidates and continue without asking
   them to choose again.
5. Apply one explicit value, then verify the normalized Result and new Revision.

Never end with “the choices cannot be read” before checking the relevant
catalog. For an unspecified top-level family, inspect the project and live
schema first; query the detailed catalog after that family is selected or
delegated. Do not dump a large catalog into prose: ask by category first or use
query and pagination. Do not treat a catalog category label as an operating
system command or a local-file search request.

## Canvas background

Treat “background,” “canvas background,” “backdrop,” and their natural-language
equivalents as scene-background intent unless the user explicitly says lighting
environment or background music.

Fan3D background modes include solid color, custom linear and radial gradients,
installed gradient presets, bundled wallpaper, imported image, imported video,
physical sky, and transparent output. Offer these top-level alternatives when
the user asks generally what backgrounds are available.

- Bundled wallpaper: list `fan3d.catalog.wallpapers.list`. It returns the
  current categories and stable wallpaper IDs. The `macOS` category is a
  Fan3D-bundled wallpaper category; it does not mean reading the user's current
  Mac desktop wallpaper. Apply an explicit ID with
  `fan3d.scene.background.wallpaper.apply`.
- Random wallpaper: use `fan3d.catalog.wallpapers.pick_random` after the user
  delegates the choice, then apply its returned ID. Pass an explicit seed when
  the selection itself must be repeatable.
- Wallpaper favorites: use `fan3d.catalog.wallpapers.favorites.list` when the
  user asks for their favorites or wants the choice narrowed to favorites. Use
  `fan3d.catalog.wallpapers.favorite.set` only when they explicitly ask to add
  or remove a favorite. Favorites are an installation preference shared with
  Fan3D's native wallpaper picker; they do not change the project Revision.
  The favorites result contains stable IDs only, so resolve their current
  title and category through `fan3d.catalog.wallpapers.list` before presenting
  them. For a delegated random favorite, those IDs may instead be passed as
  the random tool's explicit candidate list.
- Gradient preset: list `fan3d.catalog.gradient_presets.list`, retain the chosen
  preset ID, preserve the inspected complete background settings, and use
  `fan3d.scene.background.update`.
- Solid/custom gradient/sky/transparent: derive an explicit complete value from
  the inspected background and the live update schema. Preserve every inactive
  setting that the user did not ask to replace.
- Named colors: when the user asks to browse Apple, System, Crayons, or Web Safe
  names, query `fan3d.catalog.named_system_colors.list` with the appropriate
  light/dark appearance and group. Apply the returned explicit RGBA components;
  the catalog color ID is only a discovery identity and is not persisted.
- Local image or video: use the published background-media import tool only
  when the user or calling client supplied its absolute local `file:` URL for
  this task. Never search for a likely file. After import, use
  `fan3d.scene.background.update` for fill mode, blur, saturation, scale, or
  playback changes while preserving the returned project-owned asset ID.

If a user asks for “a wallpaper under macOS, choose any one,” query category ID
`macos`, choose or randomly resolve one returned wallpaper, apply it, and verify
the project. Do not ask them to open the Background inspector and do it by hand.

## Lighting environment

Environment changes affect SceneKit lighting and reflections, not necessarily
the visible canvas background. Available source families are custom linear
gradient, installed gradient preset, imported image, installed environment
preset, and transparent.

- List installed environments with
  `fan3d.catalog.environment_presets.list` and gradients with
  `fan3d.catalog.gradient_presets.list`.
- Apply the chosen source and lighting controls with
  `fan3d.scene.environment.update`, preserving inspected inactive values.
- Use the published environment-image import tool only for an explicit local
  `file:` URL supplied for the current task. Continue with the returned asset
  identity rather than inventing one.
- `simpleLightingEnabled`, reflection-probe enablement, and reflection intensity
  are explicit environment parameters. Read current values and the live schema
  range before proposing or changing them.

When “environment” could mean the visible background or the lighting source,
offer those two meanings as a structured choice instead of guessing.

## Background music and video audio

Project background music and device-screen video original audio are separate
tracks.

- For bundled music, list `fan3d.catalog.background_audio.list`. If neither a
  category nor a track was selected or delegated, ask for category first and
  track second. Install the returned track ID with
  `fan3d.scene.background_audio.install`.
- For music already placed in Fan3D's managed User Library, list
  `fan3d.catalog.background_audio.user_library.list`, retain the returned
  stable audio ID, and install it with
  `fan3d.scene.background_audio.user_library.install`. These tools never reveal
  the private library path.
- Import local music only from a user-supplied absolute `file:` URL with
  `fan3d.scene.background_audio.import`.
- Use `fan3d.scene.background_audio.update` for the project's music volume and
  mute state, and `fan3d.scene.background_audio.remove` to remove that track.
- For an imported device-screen video, inspect it first and use
  `fan3d.scene.device_screen.video_audio_volume.update`. Its range is `0...1`:
  silent through original level, never amplification.

Do not present background-music choices when the user asks to mute a device
video, and do not change video original audio when they ask about project music.
For a broad delegated request such as “pick some background music,” prefer the
bundled catalog; consult the User Library when the user mentions their own
library or the installed catalog has no suitable candidate. Choose only from
returned text metadata and never claim to have auditioned a track when no audio
preview capability was used.

## Devices and screen media

- List `fan3d.catalog.devices.list` before choosing a device. Device, variant,
  color, screen slot, and articulation are dependent choices. Present them in
  that order and retain their stable IDs and access state.
- Before creating a project from the selected device, call
  `fan3d.project.create_from_device.inspect` so the user can see the same
  authored canvas, camera, and seed-animation summary shown by the Dashboard.
- For broad device requests, narrow by catalog query before presenting items.
  If the user delegates the exact device or color, choose only from returned
  authorized candidates.
- Treat device appearance as a scene-wide choice, not a property of one device.
  Before applying it, follow the complete-replacement rule in the parent
  Skill's safe-edit workflow; use the dedicated per-device color operation for
  an individual device color or material-restoration request.
- Imported screen media supports automatic, portrait, or landscape orientation
  and fit, fill, or stretch content mode. Use the current tool schema for the
  exact wire values.
- Video trim uses source in/out points and remains reversible. The screen video
  always begins at project time zero; it cannot be moved later on the project
  timeline.

## Project templates, compositions, and device layout

These are three different choices:

- A Dashboard project template creates a new library project. On a global
  connection, list `fan3d.catalog.project_templates.list`, preflight the
  selected template with `kind=template` and
  `operation=CREATE_FROM_TEMPLATE`, then use
  `fan3d.project.create_from_template` with its stable ID and a project name.
- An authored composition can create a new project or replace the current
  scene. For new-project intent on a global connection, preflight its stable ID
  as `kind=device`, `operation=CREATE_PROJECT`, then use
  `fan3d.project.create_from_composition`; this preserves the authored canvas
  and seed animations. For current-project replacement, preflight with
  `operation=APPLY_RESOURCE` and use `fan3d.scene.composition.apply`; explain
  that superseded device-transform and camera-animation clips are removed.
- A native device layout preserves current device identities and changes their
  transforms. `fan3d.scene.device_layout.apply` offers Across, Down, Grid, and
  Radial; Grid and Radial parameters come from the live discriminated schema.

When the user says only “template” or “layout,” offer these meanings instead
of silently choosing a destructive composition or creating a different
project.

For an explicit user-selected `.fan3d` package, use
`fan3d.project.import_copy` only on a global connection and only with the
absolute local `file:` URL supplied for the task. It imports an independent
library copy; never search for project packages or describe the operation as
opening or modifying the source package.

## Camera and animation

Separate two kinds of intent:

- Camera appearance/pose: inspect the complete authored camera, then use
  `fan3d.scene.camera.update` or the dedicated reset/focus-blur operation. Before
  using focus blur, apply the parent Skill's camera-and-animation reset rule; do
  not present it as a field-only toggle.
  Meaningful groups include position and orbit rotation, field of view,
  clipping, contrast/saturation/vignette, focus blur, motion blur, HDR, and
  color fringe. Numeric bounds and enum values come from the live schema.
- Camera motion: list `fan3d.catalog.camera_motion_presets.list` for installed
  motion presets. For existing clips, inspect their IDs and ranges before move,
  resize, endpoint, timing, rename, or removal operations. Inspection returns
  complete start/end camera and label-placement state; preserve every
  unrequested endpoint field. Camera endpoints and label X/Y endpoints use
  their separate published tools.

When a user asks for a “camera effect” without enough detail, offer a small
choice between framing/pose, focus blur, motion blur, HDR/color treatment, and
an animation preset. These are intent categories, not guessed preset IDs.

## Proactive capability overview

When the user asks broadly what Fan3D can set, give a concise overview before
asking them to choose a direction. Cover only relevant current families:

- canvas background: color, custom or preset gradient, bundled wallpaper,
  local image/video, sky, or transparency;
- lighting environment: custom or preset gradient, environment preset, local
  image, reflection controls, or transparency;
- devices: catalog item, variant, color, transform, articulation, appearance,
  screen media, orientation, fill mode, video trim, original-audio level,
  authored compositions, and Across/Down/Grid/Radial layouts;
- camera: pose/framing, lens and clipping, focus and motion blur, image
  treatment, and camera-motion presets with timing;
- audio: bundled music, managed User Library, explicit local import, volume,
  mute, and removal;
- scene structure and delivery: labels, project duration, animation clips,
  installed label fonts, preview, validation, and final output;
- new-project starting points: blank canvas, a catalog device, or an installed
  Dashboard project template or authored composition when the connection is
  global; an explicit user-supplied `.fan3d` package can be imported as a copy;

This overview is navigation, not a static substitute for live discovery. After
the user selects a family, inspect and query its current catalog or tool schema
before presenting exact values.

## Labels, timeline, and output

- Directional labels are eight explicit slots. Inspect all slots and preserve
  unchanged text, style, and placement when updating one part. Query
  `fan3d.catalog.directional_label_fonts.list` and pass its returned exact
  `postScriptName` as `fontPostScriptName` before changing a font.
- Timeline duration and each animation range share project time in seconds.
  Inspect existing clips before proposing a duration that could overlap or
  truncate them.
- For a friendly final movie, query
  `fan3d.catalog.render_delivery_presets.list`, choose a shared purpose/layout,
  clarity, and returned FPS, then call `fan3d.render.delivery_preset.resolve`
  with all three IDs/values at the inspected Revision. Equivalent platform
  names belong to the same purpose; do not offer separate Douyin and TikTok
  choices. If the choice is delegated, use the target default layout,
  `defaultClarityID` (`ultra_hd_4k`), and `defaultFrameRateFPS`. Clarity controls
  output pixels; FPS only controls motion sampling and cannot replace clarity.
  Use the returned movie settings unchanged for capability and submit.
- For advanced sizing or non-movie output, query
  `fan3d.catalog.render_output_presets.list` for the same named sizes shown by
  the GUI, such as 16:9, 1080p, Mac, Apple TV, and Apple Watch. Resolve
  `fixed`, `aspect_ratio`, and `custom` sizing through the parent Skill's render
  job rules; preset and resolution IDs are discovery identities, not submit
  parameters. Output kinds include a static image at an explicit project time,
  a movie, an image sequence, and Web output.
  Static image formats are PNG, JPG, GIF, and TIFF; only PNG/TIFF retain
  transparency. When output kind, size, or format is unspecified and materially
  affects the result, present a bounded choice.
- Call `fan3d.render.capability.check` with the exact intended Revision and
  settings before `fan3d.render.submit`. MCP renders use Fan3D's managed,
  non-overwriting destination; there is no caller-supplied destination or
  overwrite field to preserve.

## Capability gaps

If a live connection omits a tool named in this reference, say that the current
Fan3D connection cannot perform that specific operation. Still offer any
available alternatives from current catalogs instead of claiming the entire
creative area is unavailable. Never fall back to UI automation or direct
project-package edits.
