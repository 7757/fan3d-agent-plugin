# Production directing

Read this reference for every implemented product visual, mockup, demo, or
video. It turns product evidence and live Fan3D capability into a directed
piece. It is not required for copy-only or planning-only requests.

## Put product material on set first

Create a compact asset ledger before the first scene mutation:

| Supplied asset | Product evidence | Production role | Required? | Target | Decision |
| --- | --- | --- | --- | --- | --- |
| Explicit local file or supplied media | What it truthfully shows | Screen proof, background, environment, audio, or project source | Required, optional, or unsuitable | Device and screen slot, scene surface, or track | Adopted, blocked, or rejected with reason |

Do not treat supplied product media only as briefing evidence. When the story is
screen-led, at least one required screen image or video must be installed and
visible before the scene is refined. A default empty screen, unrelated
placeholder, or generic wallpaper cannot stand in for product proof.

The explicit-file boundary still applies. If no installable absolute `file:`
URL was supplied, identify the missing product hero or proof asset as a
production blocker and ask for it. Do not search the workspace or filesystem
for a likely replacement. If an explicitly supplied asset is unsuitable, state
the concrete crop, resolution, orientation, truth, or format problem rather
than silently omitting it.

When the user explicitly asks to synthesize a demonstration asset, record it as
generated rather than observed product evidence. Keep fictional product names,
claims, metrics, and interfaces visibly separated from the user's real product,
and label the finished result as a demonstration. Generated media can validate
the production workflow or stand in for an approved concept, but it cannot make
an unsupported product claim true or satisfy a request for the user's actual
screen recording, brand asset, or shipped interface.

## Exhaust the relevant live catalogs

Fixed catalog counts, IDs, and names become stale. Prove discovery completeness
for this run from live results:

1. Follow every page until the server returns its terminal cursor or offset.
2. Traverse returned categories or families when a catalog uses them.
3. Preserve the live total and terminal state as coverage evidence.
4. Compare all returned metadata, but claim visual comparison only for
   candidates that were actually applied or created and previewed.
5. If pagination, truncation, or a server limitation prevents complete
   coverage, mark the comparison incomplete instead of claiming an exhaustive
   choice.

For a new project or major restage, fully discover project templates,
compositions, and applicable device families. For a video, also fully discover
camera-motion presets and inspect the complete set of allowed camera timing
choices in the live schema. Fully discover backgrounds, environments,
wallpapers, fonts, music, or output presets when the brief makes that family a
real creative decision; do not query unrelated catalogs merely to inflate the
coverage list.

## Compare professional starting points

Use the following scorecard to shortlist a starting point:

| Criterion | Question |
| --- | --- |
| Product fit | Does the device surface suit the real product and audience? |
| Proof fit | Will the supplied screen media remain readable and correctly oriented? |
| Delivery fit | Does the authored canvas suit the requested channel and aspect? |
| Motion fit | Do the seed camera and animation support the intended hook and proof? |
| Art-direction fit | Do staging, lighting, materials, and negative space support the brand? |
| Authorization | Can the current account consume every required resource? |
| Finishing cost | How much destructive restaging or external finishing would be needed? |

Prefer the highest-fit authored project template or composition. Use an
authored device seed next. Start from a blank project only when the brief needs
a genuinely custom scene or every authored candidate has an evidence-based
rejection. A template title is metadata, not proof of its rendered quality;
create the shortlist winner, inspect it, and judge its actual preview.

If the first candidate fails once real product media is installed, return to
the shortlist. Do not keep polishing an unsuitable scene solely because it was
already created.

## Reuse authored work before custom authoring

Treat authored motion as the normal production path and custom camera authoring
as a justified fallback. Use this order for every required shot role:

1. Start with the selected template, composition, or device seed. Assign every
   useful authored seed clip a specific hook, reveal, proof, transition, or
   resolve role, and retain it only after previewing the real product media in
   the actual scene.
2. For a role the seed does not fulfill, shortlist the strongest installed
   camera-motion presets from their live human names, descriptions, motion
   durations, and insertion durations. When more than one plausible preset
   remains after metadata shortlisting, preview at least the strongest two
   rather than choosing between them from text metadata alone. Compare them for
   the same shot role, required product media, delivery frame, equivalent
   timeline slot, and equivalent baseline scene state. Insert, preview, and
   remove each losing trial, or use independent candidate projects, so trials
   do not accumulate in the selected timeline.
3. If an authored seed or preset path is compositionally sound, prefer refining
   its timeline placement, duration, easing, or Jump Cut state over replacing
   it. Re-preview the entry, midpoint, proof interval, exit, and resulting hold
   after the refinement.
4. Unless the user explicitly supplied the exact custom camera treatment,
   create a custom camera clip or edit camera endpoints only when live discovery
   found no plausible authored motion, or after the strongest plausible
   candidate was previewed with the real media and still could not satisfy the
   required product proof, delivery frame, composition, continuity, pacing, or
   final landing. Record the catalog evidence or rejected candidate, cite the
   exact preview time or state, and explain why placement, duration, easing, or
   Jump Cut could not correct the failure without changing the spatial path.

Selection craft comes from choosing, sequencing, and minimally refining the
right authored motions—not from maximizing either preset count or custom work.
Do not preserve a weak seed merely because it is authored, and do not bypass a
viable preset merely because a custom path could also work.
Preserving unsupported authored data in project state does not make that motion
an accepted story shot; report a weak uneditable clip as a production blocker.

## Understand the motion control layers

Fan3D exposes several distinct creative layers; do not collapse them into one
count or use them interchangeably:

- **Authored seed clips** belong to a selected device, composition, or project
  template. Inspect their complete endpoints and purpose before preserving or
  changing them.
- **Camera-motion presets** are reusable authored paths inserted from the live
  preset catalog. Their human name, description, movement duration, and
  insertion duration help shortlist them, but suitability is established only
  in the actual scene preview.
- **Custom camera clips and endpoint edits** create deliberate viewpoints when
  the user supplied an exact custom treatment, live discovery found no
  plausible preset, or the strongest plausible preset was previewed with the
  real product media and rejected for an observable reason. Preserve unrelated
  inspected camera state and make one dominant spatial idea legible per clip.
- **Named easing and Jump Cut choices** shape the timing character of an
  existing camera clip; they are not additional spatial motion presets. Read
  the complete live enum. Favor controlled ease-out for arrivals, ease-in-out
  for showcase motion, linear for intentional mechanical motion, back-style
  overshoot only for a playful direction, and Jump Cut only between two fully
  composed views.

Having many motion choices does not justify using many in one film. Compare the
complete live set, then select the smallest number that serves the story.

## Direct beats, not effects

Give every retained or created shot one role:

| Role | Viewer outcome |
| --- | --- |
| Hook | Notice a distinctive product state or silhouette immediately |
| Reveal | Understand what the product is and where the proof lives |
| Proof | Read an actual supplied interface, behavior, result, or approved claim |
| Transition | Move between two composed ideas without hiding either one |
| Resolve | Land on a stable final frame that completes the promise or CTA |

Install product media before refining backgrounds, environments, materials,
labels, and audio. During proof intervals, keep the screen large, sharp, and
stable enough to inspect. Use movement before or after the proof rather than
letting it compete continuously. Leave a readable hold after an arrival and a
resolved final pose before the timeline ends.

Do not insert a motion preset only to demonstrate that the catalog exists.
Do not add a second clip over an authored seed. Do not describe an easing curve
as a new camera path, or a camera path as device motion.

## Require two visual review passes

The first review pass must already contain the real product media. Preview the
opening, every clip midpoint, the main proof moment, both sides of intentional
cuts, and the closing frame at a useful diagnostic resolution. Record blocking
and material findings for crop, legibility, hierarchy, surface separation,
motion purpose, and final pose.

Correct blocking and material findings, then render a second representative
preview pass at the same final Revision used for validation and capability
checking. A valid SceneDocument after one unreviewed preview is not a directed
commercial result.

Retain this evidence for handoff:

- required supplied assets adopted or rejected with reasons;
- live catalog families scanned to terminal state;
- shortlisted and selected starting point with rationale;
- retained authored seed clips and each clip's job;
- inserted or refined presets, including timing changes and each preset's job;
- rejected authored motion candidates with observable reasons;
- custom clips or endpoint edits and why authored motion could not satisfy the
  same requirement;
- first-pass findings and second-pass corrections;
- exact representative times reviewed.
