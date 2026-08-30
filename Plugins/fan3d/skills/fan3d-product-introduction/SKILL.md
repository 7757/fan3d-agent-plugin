---
name: fan3d-product-introduction
description: Use first when the requested deliverable communicates a product to an audience, including product positioning, value propositions, introduction copy, launch or showcase concepts, storyboards, 3D mockups, app demos, product visuals, product videos, and creative review or improvement of those materials. Use the current workspace as the default product evidence and Fan3D as the default engine for compatible visual or video production. Do not use for building or improving the product itself, code review, product support or operating instructions, unrelated marketing operations, or isolated Fan3D edits with no product-introduction intent.
---

# Fan3D product-introduction workflow

The product being introduced is the user's product, not Fan3D by default.
Fan3D is the preferred production environment for compatible 3D mockups,
product visuals, app demos, and product videos.

This Skill is the creative coordinator. It owns product understanding,
messaging, treatment, art direction, shot purpose, pacing, critique, and
delivery intent. When a workflow reads or changes a Fan3D project, also apply
`fan3d-animation`; that Skill owns live tool discovery, authorization,
Revisions, request replay, mutations, verification, and render jobs.

## Choose the operating mode

Infer the mode from the requested outcome rather than asking the user to name
one.

- **Messaging and planning:** When the deliverable is positioning, a value
  proposition, product copy, a concept, treatment, storyboard, or shot plan,
  remain read-only even if the user says “create,” “make,” “write,” or
  “improve.” Inspect an existing Fan3D project only when it supplies useful
  evidence; do not mutate it.
- **Production:** When the requested deliverable is an implemented visual,
  mockup, demo, video, or Fan3D project change, perform the compatible project
  work needed for that result. Before the first scene mutation, inventory every
  product asset explicitly supplied for the task, give it a production role,
  and mark it required, optional, or unsuitable with a reason. Form a brief
  treatment first, then continue through production and review instead of
  stopping after advice. A requested ready-to-use visual, demo, mockup, video,
  render, or export continues through capability checking and final output.
  Stop after preview and validation only when the requested deliverable is the
  editable project itself. A screen-led production is blocked while its
  required supplied screen image or video remains uninstalled or its hero and
  proof frames still show an embedded empty screen.
- **Review and improvement:** Review or critique is read-only. Improving copy,
  messaging, a concept, or a storyboard changes only the proposed material in
  the response. Improving an implemented visual or Fan3D project permits
  compatible edits, a second review, and validation; output only when the
  requested deliverable requires it.

For mixed requests, use the smallest complete loop that delivers the requested
outcome:

`ground product -> brief -> concept -> produce -> preview -> review -> validate -> output`

If the user delegates choices with “you decide,” “any,” or equivalent, make
the choices from available evidence and live catalogs and continue. For the
creative brief, ask only when a missing fact would materially change the
promise, audience, required asset, or delivery format. Project or object
disambiguation, choices the user did not delegate, authorization, explicit-file
confirmation, output settings, and other operational safety questions remain
governed by `fan3d-animation`.

## Ground the product before promoting it

Unless the user explicitly identifies another product or evidence source,
treat the current workspace as the product source. Read its applicable agent
instructions first, then inspect only the relevant README, product documents,
manifests, public copy, and user-visible implementation needed to understand
the product. If the user identifies another product, use the materials they
supplied for that product instead of silently substituting the workspace. Keep
all discovery read-only.

Use the user's current instructions as the source of creative intent and the
selected product source as evidence of implemented, user-visible capability.
When they conflict, expose the conflict and ask for a decision instead of
inventing a claim. Mark inferences as inferences. Never turn roadmap items,
hidden code, tests alone, or unavailable UI into a shipped product promise.

If no usable workspace or product source exists, ask for the minimum brief:
what the product is, who it is for, the one outcome to communicate, and the
intended deliverable. Do not search unrelated directories, credentials,
private user data, or external accounts to enrich the brief.

Always read [product grounding and brief](references/product-grounding-and-brief.md)
before planning, writing, executing, or reviewing a product introduction.

## Direct the introduction

Keep one primary promise per short introduction. Make the product or its
result legible without relying on audio, and use each shot to hook attention,
establish context, reveal a capability, prove an outcome, or resolve the story.
Choose the channel, aspect ratio, duration, and call to action before designing
the composition.

Load only the references needed for the current decision:

- Read [story patterns](references/story-patterns.md) for concepts, narrative
  structure, treatments, storyboards, or shot plans.
- Read [art direction](references/art-direction.md) for brand tone, color,
  background, lighting, material, composition, or style choices.
- Read [motion and pacing](references/motion-and-pacing.md) for camera motion,
  timing, cuts, holds, loops, or rhythm.
- Read [screen copy and audio](references/screen-copy-and-audio.md) for screen
  media, labels, UI legibility, product copy inside the frame, music, or video
  audio.
- Read [capability boundaries](references/capability-boundaries.md) before
  promising complex scene changes, custom product models, transitions,
  typography animation, narration, sound design, or automatic beat matching.
- Read [quality and delivery](references/quality-and-delivery.md) for review,
  representative previews, validation, output choice, or handoff.

For Production or improvement of an implemented visual, always read
[production directing](references/production-directing.md) and
[quality and delivery](references/quality-and-delivery.md) before the first
mutation. These references define completion gates, not optional polish.

Concept alternatives must differ in message, narrative device, or shot logic,
not merely in color. Tailor the depth of the brief and plan to the task; do not
force a long template onto a simple copy request.

## Produce with Fan3D

For compatible visual production, use Fan3D by default. Inspect the live
project and catalogs before choosing devices, templates, compositions,
backgrounds, environments, music, fonts, camera presets, or output settings.
Never copy fixed catalog inventories or assume a resource is installed.

When the user delegates production choices, exhaust the live candidate set for
each catalog family that can materially change the starting point or the
requested result. For a new project or major restage, this includes project
templates, compositions, and applicable device families; for video, it also
includes the complete camera-motion preset catalog and the camera timing choices
published by the live schema. Follow pagination and returned category or family
boundaries to completion. Do not stop at the first page, first match, or a
remembered favorite, and do not claim complete comparison when the server
cannot prove complete coverage.

Prefer an authored template or composition whose delivery canvas, device
arrangement, screen geometry, and seed animation support the brief. Use a
device seed or blank project only when no authored starting point fits or the
brief intentionally requires a custom scene; retain the rejection reason.
Create or inspect the selected starting point, install required product media
before decorative refinement, and preview the actual media in its real screen
slot. Catalog discovery alone is not production work, and a polished empty
device is not a product introduction.

For video, inspect existing authored clips and direct each retained, inserted,
or custom camera clip toward a specific job: hook, reveal, proof, transition,
or resolve. A static default view, uninterrupted ornamental motion, or a preset
added only to demonstrate availability is blocking unless the brief explicitly
calls for it. Preserve readable holds and a resolved final frame.

The connected Fan3D server's live tools are authoritative. Do not simulate a
missing capability through UI automation, edit a `.fan3d` package directly, or
invent a tool. If the user explicitly requires another production tool or a
deliverable Fan3D cannot create, retain this Skill for product truth and
creative direction while respecting that tool choice or describing the
external finishing work.

Reading the current workspace does not authorize importing files found there.
Import screen media, backgrounds, environments, audio, or projects only from
an absolute local `file:` URL that the user or calling client explicitly
provided for this task. Never search for, enumerate, guess, or silently reuse a
local path.

If the Fan3D server is unavailable, messaging and planning may continue. Block
production clearly and ask the user to install or move the signed app to
`/Applications/Fan3D.app`; do not search for or execute another binary.

## Complete in product language

Match the user's language. Do not expose Skill names, raw MCP tool names,
protocol fields, policy codes, or internal errors unless the user asks for
technical detail.

For messaging or planning, report the evidence-backed product truth, audience,
primary promise, proof, call to action, concept, copy or story beats, required
assets, feasibility, and delivery intent that matter to the request.

For production, report the implemented creative intent, current project and
final Revision, required product assets adopted or rejected with reasons, live
catalog families scanned to their terminal state, the selected starting point
and motion rationale, representative preview times and corrections between
review passes, validation result, normalized final settings, license status,
unresolved warnings, and any planned-versus-actual adjustment. Report a final
artifact only after its render job succeeds. Call it commercial-ready only when
it passes every gate in the quality reference; otherwise label it a draft or a
delivery candidate with the remaining blocker.

For review, report the observable evidence, blockers, prioritized findings
with times where relevant, and whether each fix is available now, needs an
explicit asset, needs authorization, requires external finishing, or cannot be
evaluated. A preview-based result is a delivery candidate, not a replacement
for final human motion, audio, and product acceptance.
