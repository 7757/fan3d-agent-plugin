# Capability boundaries

Read this reference while translating a creative treatment into an executable
Fan3D plan or when a requested effect may exceed the connected server.

## Establish the current boundary

- Treat the connected server's `tools/list`, the inspected project, live tool
  schemas, and current catalog results as authoritative.
- Discover devices, templates, compositions, backgrounds, environments, music,
  fonts, and motion presets at runtime. Do not copy fixed catalog counts, names,
  IDs, availability, or parameter ranges into the treatment.
- Avoiding fixed counts does not permit a partial delegated search. Prove
  completeness for each relevant family from the current pagination, category,
  total, and terminal-state results described in `production-directing.md`.
- Use catalog-returned stable identities and current resource-access results.
  A planning check does not replace the consuming operation's final
  authorization.
- If a required tool is absent, do not simulate it with UI automation, edit a
  project package, or invent a result.

## Classify every requested production step

Use one of these labels in the internal plan and in any handoff where the
distinction matters:

- **Fan3D-native:** The current server can perform the step deterministically,
  such as arranging catalog devices, setting the scene, editing supported camera
  or timeline state, previewing, validating, or rendering.
- **Catalog-driven:** The step is native only after selecting a current device,
  template, composition, background, environment, font, music track, motion, or
  output choice returned by a live catalog. Do not promise a remembered option.
- **Source-dependent:** Fan3D can install or present the result, but the caller
  must provide the source asset. Local image, video, and audio imports require an
  explicitly supplied absolute `file:` URL; Fan3D does not discover or guess
  files.
- **Authorization-dependent:** The required resource or output exists, but the
  current account state requires a native claim, sign-in, entitlement, or other
  user-owned resolution before the consuming write can proceed.
- **Unsupported in the current connection:** The requested state cannot be
  represented by the published tools. State the specific missing capability and
  stop before claiming it changed.
- **Post-production:** The step is intentionally reserved for another editing
  stage, such as unsupported compositing, complex typography, narration,
  sound-effect design, multi-track mixing, or editorial cuts. Describe the
  required handoff asset or timing note without implying that Fan3D completed it.

Do not collapse “unsupported” and “post-production.” Unsupported is a current
capability fact; post-production is a deliberate workflow decision. When a
Fan3D-native alternative preserves the user's intent, offer it explicitly, but
do not silently substitute it.

## Recognize common non-native requests

Treat these as current boundaries unless live discovery publishes a more
specific capability:

- Fan3D presents installed catalog devices and authored compositions; it does
  not import or construct an arbitrary physical product as a new 3D model. A
  product outside that catalog needs prepared screen/background media, another
  3D workflow, or external compositing.
- Camera clips can create multiple views of one persistent scene. They do not
  create independent shots with different device sets, backgrounds,
  environments, screen bindings, or static label copy. Combining separately
  authored scenes is an external editorial step.
- Camera motion must not be described as device motion. Do not promise a new
  device transform animation, and edit articulation motion only when the
  inspected project already has a compatible authored clip and the live server
  exposes the requested endpoint operation.
- Screen videos begin at project time zero. Source trimming does not schedule a
  later start, change playback speed, animate a crop, or swap media midway.
- Directional-label content and style are persistent scene state. Existing
  camera clips may expose bounded label-position endpoints, but they do not
  provide timed copy replacement or kinetic typography.
- Crossfades, masks, particles, layered compositing, motion graphics, narration,
  voice generation, sound effects, multi-track mixing, ducking, fades, music
  editing, and automatic beat analysis are not implied by camera, label, or
  audio controls.
- A bounded set of still preview frames can evaluate composition at selected
  times. It cannot establish full-motion smoothness, audio quality, mix balance,
  or seamless looping without a final render and human review.
- Text-only catalog metadata does not prove that every template, composition,
  wallpaper, or music choice was visually compared or auditioned. Create or
  apply a selected candidate and preview what the connection can actually
  render before judging it.

For a landing page, presentation, or another non-Fan3D carrier, keep product
grounding and story direction in this Skill. Use Fan3D for compatible product
visuals, and leave construction of the carrier to its dedicated workflow.

## Preserve truthful scope

- A preview proves only the rendered visual frames requested at the inspected
  Revision. It does not prove audio quality, final encoding, or an unrendered
  transition.
- A successful scene mutation does not prove that final output is supported.
  Capability-check the exact delivery settings before submitting a render.
- Preserve entitlement, license, watermark, normalization, and fallback warnings
  returned by Fan3D. Never route around them through a different output kind or
  path.
- Keep an explicit post-production list throughout the task so unsupported
  requests do not disappear from the final handoff.
