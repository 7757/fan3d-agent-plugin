# Quality and delivery

Read this reference when reviewing a product introduction, deciding whether it
is ready to render, or handing off a final artifact. For implemented production
this reference is mandatory. A successful mutation, valid scene, or successful
render is necessary but not sufficient for commercial readiness.

## Classify the intended finish

Use one of these labels honestly:

- **Draft:** Suitable for direction, timing, or capability exploration. It may
  use reduced output settings, placeholder media, or unresolved findings.
- **Delivery candidate:** The requested final artifact was rendered and passed
  automated checks, but human motion, audio, product, or channel acceptance is
  still pending.
- **Commercial-ready:** Every product, direction, technical, authorization, and
  license gate below passed, and the required human acceptance was completed.

Never call a draft or an unreviewed successful render commercial-ready.

## Enforce product and directing gates

Before final output, confirm all of the following:

- Every required explicitly supplied product asset was adopted, or has a stated
  rejection reason and an approved replacement plan.
- The primary product or interface is visible in the hero and proof frames. A
  screen-led introduction contains no embedded empty screen, unrelated
  placeholder, wrong orientation, accidental crop, distracting letterbox, or
  unreadable primary UI.
- Source media remains sharp at its final on-screen size. If it is visibly soft,
  request a higher-resolution source; do not disguise source blur by increasing
  only the final output dimensions.
- Relevant live catalogs were scanned to their terminal state and the chosen
  starting point, device, motion, and output have brief-based reasons. Full
  metadata comparison does not imply that unpreviewable candidates were
  visually compared.
- Every camera clip serves hook, reveal, proof, transition, or resolve. Clips do
  not overlap or truncate, proof moments include readable holds, and the final
  pose resolves intentionally.
- Blocking and material findings from the first real-media preview pass were
  corrected and the affected times were previewed again.

## Run a read-only review

When the user asks for critique, review, or readiness assessment, inspect and
preview without changing the project. Do not turn review findings into edits
unless the user also asks for implementation.

Review with evidence from explicit preview times and the inspected project:

- **Message:** Is the product identifiable quickly, and does each shot make one
  truthful point that the visible product state supports?
- **Hierarchy:** Is the product the clearest subject? Are labels brief, legible,
  and subordinate to the demonstrated UI or hardware?
- **Framing:** Are important screen regions visible, uncropped, and readable at
  the intended delivery size?
- **Motion:** Does camera motion reveal information rather than obscure it? Look
  for abrupt framing changes, overlapping intent, dead time, and an unresolved
  final pose.
- **Continuity:** Do device orientation, screen state, background, lighting, and
  copy remain intentionally consistent across the sampled times?
- **Contrast and finish:** Check silhouettes, reflections, background separation,
  safe margins, and visual noise without assuming the preview equals final
  encoding quality.
- **Audio state:** Report only the inspected track identities, mute states, and
  levels. Do not claim that balance, intelligibility, or musical fit was heard
  unless an actual audition occurred.
- **Delivery fit:** Confirm that duration, aspect ratio, transparency, output
  kind, and intended channel agree with the user's brief and the live render
  contract.

Classify findings as **blocking**, **material improvement**, or **optional
polish**. Cite the relevant time or inspected state, explain the viewer impact,
and recommend the smallest correction. Keep Fan3D-native corrections separate
from source-asset changes and post-production notes.

Treat validation failure, unsupported delivery settings, an unreadable primary
proof, missing required media or authorization, a truncated project inspection,
or an unknown animation track that affects the result as blocking. Do not claim
a complete review until the relevant state is observable.

Review opening, hero, proof, cut boundaries, clip midpoints, and closing at the
delivery aspect and the highest useful preview resolution supported by the live
tool. A small thumbnail can establish broad composition, but it cannot prove
fine UI legibility or source sharpness.

## Enforce the commercial technical gate

When the user requests a final commercial video but has not supplied a delivery
specification:

- resolve a channel-appropriate aspect and dimensions from the live output
  catalog; use a final raster whose short edge is at least 1080 pixels;
- use at least 24 fps, and normally 30 fps or higher for UI or social motion;
- use the highest reasonable supported final quality, enable antialiasing, and
  make an explicit jittering choice based on motion review rather than inheriting
  a low-cost test setting;
- when jittering or another temporal-sampling option is enabled, compare encoded
  representative frames with the same-Revision preview or a no-jitter diagnostic
  for global exposure, color, sharpness, and motion-edge integrity; a material
  shift is blocking even when the render job reports success;
- capability-check the exact width, height, frame rate, quality, antialiasing,
  jittering, codec, and container that will be submitted;
- submit the normalized supported settings at the final reviewed Revision.

Lower settings are appropriate for drafts and diagnostic previews, but the
artifact and handoff must say so. Never present preview media, an upscaled
low-resolution render, or a fast test preset as a commercial master.

License is a blocking commercial gate. If the final job reports a Free or
offline fallback, visible watermark, personal-non-commercial restriction, or
equivalent warning, preserve it and label the artifact non-commercial. Ask the
user to complete the native Creator or authorization flow, or to explicitly
accept a non-commercial draft. Never hide the warning or route around it with a
different output kind.

## Inspect the encoded artifact

After a final movie succeeds, inspect the artifact itself rather than treating
the job record as the entire quality check:

- confirm encoded width, height, constant or intended frame rate, duration,
  codec, pixel format, audio presence, and color metadata match the normalized
  delivery plan;
- decode the complete file and fail the delivery check on decode errors, black
  frames, unintended freezes, broken timestamps, or truncated duration;
- inspect full-resolution frames near 0%, 25%, 50%, 75%, and 100%, plus any
  known cut or fastest-motion point, for screen sharpness, placeholder content,
  crop, safe areas, banding, compression noise, and color shifts;
- when an audible track is intended, listen to the final movie and measure it
  against the target channel. For ordinary web or social delivery without a
  supplied audio specification, roughly `-16 ± 2 LUFS` integrated and no peak
  above `-1 dBTP` is a useful review target. A near-silent track is not a
  substitute for an intentional silent export;
- when a platform will transcode the movie, review that transcode as a separate
  delivery candidate. Wide-gamut or unusual transfer metadata must not be
  assumed to survive a generic web pipeline correctly.

These checks can disqualify a technically successful render. If the current
environment cannot decode, measure, listen to, or platform-test the artifact,
state the unverified item and keep the result at delivery-candidate status.

## Verify before final output

1. Inspect the final Revision after the last edit.
2. Render four to eight representative preview frames when the duration and
   structure justify them: opening, proof moments, clip midpoints, immediately
   around intentional cuts, and closing. Use fewer for a simpler piece. Preview
   frames are diagnostic evidence, not the final deliverable.
3. Correct blocking and material findings, then render the affected times a
   second time at the final Revision.
4. Run scene validation at that same Revision and resolve blocking failures.
5. Check the exact intended output settings with the read-only render capability
   operation. If unsupported, change the plan or report the boundary; do not
   submit the same settings blindly.
6. Submit only the verified settings. Retain the render job identity and read its
   status at a reasonable interval until it reaches a terminal state.

Only a successful final job result establishes a deliverable. Do not report a
preview location, guessed filesystem path, or in-progress job as final output.
Fan3D owns the managed, non-overwriting destination.

## Hand off truthfully

Report the final project Revision, normalized output choice, validation result,
job status, artifact URI when successful, and all relevant warnings. Preserve
license or watermark fallback warnings. Also report required supplied assets
adopted or rejected, live catalog families scanned to terminal state, the
selected starting point and motion rationale, first-pass findings, second-pass
corrections, and exact preview times. End with a separate list of source assets
still needed, unsupported requests, deliberate post-production work, and human
motion/audio/product acceptance still pending so the recipient knows exactly
what Fan3D did and did not produce.
