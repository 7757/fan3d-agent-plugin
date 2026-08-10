---
name: fan3d-animation
description: >-
  Use when a user wants to inspect, create, edit, preview, validate, or render
  a local Fan3D 3D animation project through Fan3D MCP tools.
compatibility: Requires macOS and Fan3D.app installed in /Applications.
---

# Fan3D animation workflow

Use Fan3D as a deterministic local 3D editor and renderer. Fan3D owns the
project transaction, scene evaluation, and render pipeline; Codex owns the
conversation, planning, and tool-use loop.

The local plugin expects the signed `Fan3D.app` to be installed in the standard
`/Applications` directory. If the bundled server cannot start, ask the user to
install or move Fan3D there; do not search arbitrary filesystem locations or
substitute another executable.

## Tool availability

Use only `fan3d.*` tools that the connected server actually publishes. Never
assume a planned tool exists, simulate unavailable functionality through UI
automation, or modify a `.fan3d` package directly.

The server may publish these workflow-level tools as their backing Application
Use Cases become available:

- `fan3d.project.inspect`
- `fan3d.scene.apply_operations`
- `fan3d.preview.render`
- `fan3d.scene.validate`
- `fan3d.render.submit`
- `fan3d.render.status`
- `fan3d.render.cancel`

If a required tool is absent, state which capability is unavailable and stop
before claiming that the project or output changed.

## Editing sequence

1. Inspect before editing.
   - Read the project identity, current revision, bounded scene summary, and
     relevant stable object IDs.
   - Do not infer a mutation target from hover, current UI selection, or visual
     position alone.
2. Form a bounded change set.
   - Prefer one atomic operation batch for a single user intent.
   - Reuse stable IDs from inspection results.
   - Supply the inspected `expectedRevision` and a fresh `requestID` whenever
     the tool contract requires them.
3. Apply the change once.
   - Never retry a write blindly.
   - Preserve normalized values, warnings, and the returned revision for later
     calls.
4. Verify visually and structurally.
   - Render a bounded preview at an explicit revision and explicit time or time
     range when the preview tool is available.
   - Run scene validation before final output when validation is available.
5. Submit final output only after verification.
   - Choose explicit bounded movie, image-sequence, or web settings. Do not
     provide or invent an output path; Fan3D derives a non-overwriting managed
     destination for the project.
   - Rendering is a job: retain its `jobID`, poll status at a reasonable
     interval, and report the final output URI only after success.
   - A `jobID` belongs to the current MCP server session. Query or cancel it
     before that session ends; do not claim it can be recovered after restart.
   - Pass the same project identity when reading or cancelling an external
     job. Cancel a render only when the user explicitly asks to cancel that
     exact `jobID`.

## Error and concurrency rules

- Treat structured tool errors as authoritative.
- On `revision_conflict`, inspect once, explain the concurrent change, and
  re-plan against the new revision. Do not overwrite or loop retries.
- On permission or file-reference errors, ask the user to authorize the input
  through Fan3D. Do not substitute an arbitrary filesystem path.
- On cancellation before a commit point, report that the project is unchanged
  only when the structured result says so.
- Stopping the Codex turn does not imply that a previously accepted render job
  was cancelled.
- Never expose credentials, user media contents, hidden reasoning, or raw
  project JSON. Only report a local file URI when a successful Fan3D tool
  returns that managed preview or output URI.

## Completion standard

Do not describe an animation as created or rendered until the corresponding
tool result confirms success. Summarize the observable result, final revision,
warnings, validation outcome, job status, and output URI when available.
