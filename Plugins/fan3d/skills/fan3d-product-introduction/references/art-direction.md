# Art direction

Read this reference when a product introduction needs a visual direction, a
brand mood, or a judgment about background, environment, device appearance,
labels, and camera image treatment. Treat live Fan3D catalogs and the inspected
project as authoritative; this reference explains how to choose, not which
catalog ID to use.

## Translate the brief into a visual system

Identify the following before choosing controls:

- the user's product and the one claim the introduction should make;
- the audience and the response the final frame should invite;
- two or three useful brand traits, such as precise, warm, playful, or bold;
- the proof that must remain readable, such as a UI state, a website, a visual
  identity, or a product family;
- the delivery canvas and any user-supplied brand colors or media.

The product is the user's offering. A phone, laptop, display, or other Fan3D
device is a presentation surface only when it helps communicate that offering.
Do not turn a software, service, campaign, or website introduction into a
hardware advertisement by default.

If the user delegates the direction, infer it from supplied product and brand
evidence and continue. Ask only when different answers would lead to materially
different concepts.

## Establish hierarchy

Prefer one dominant visual idea per scene:

1. Make the product proof readable at the intended output size.
2. Use the device or device group to frame that proof, not compete with it.
3. Separate the silhouette from the canvas with value, hue, shadow, or
   reflection contrast.
4. Use environment lighting to shape surfaces and the visible background to
   shape the composition. They are separate decisions.
5. Reserve labels for a short persistent promise or CTA that can remain valid
   throughout the timeline.

Negative space is useful when it clearly supports copy, movement, or an end
frame. Avoid adding visual emptiness without a compositional purpose.

## Map brand intent to Fan3D controls

Use these as decision patterns, not fixed recipes:

| Intent | Prefer | Watch for |
| --- | --- | --- |
| Clean and trustworthy | Native materials, a restrained solid or gentle gradient, clear screen contrast, simple shadows, neutral image treatment | Low-contrast devices disappearing into a pale canvas |
| Premium and precise | Native finishes, controlled reflections, a dark or tonal background, deliberate negative space, subtle focus separation and vignette | Crushed screen content, excessive blur, or effects that make the product feel synthetic |
| Warm and approachable | Soft color relationships, a welcoming gradient or supplied image, moderate shadows, open framing, restrained saturation | Brand colors contaminating the product proof or reducing text contrast |
| Playful and illustrative | Clay appearance when appropriate, confident color override, smoother or longer shadows, asymmetry, bolder framing | Applying clay when authentic material finish is part of the value proposition |
| Bold and energetic | Strong but controlled contrast, graphic backgrounds, decisive camera angles, concise labels, clear color blocking | Combining every effect at once or sacrificing screen legibility for intensity |
| Technical and capable | Crisp native materials, structured multi-device layout, cooler or neutral environment, precise framing, minimal decorative treatment | Making the composition clinical without showing a meaningful proof state |

Preserve catalog-authored device colors and materials unless the concept
requires an explicit change. Scene-wide clay, color override, shadow, and glass
reflection choices affect the whole device presentation; evaluate them as a
system rather than as isolated toggles.

## Background and environment

- The canvas background is what the viewer sees. Choose its color, gradient,
  wallpaper, supplied image or video, sky, or transparency for composition and
  brand contrast.
- The environment controls lighting and reflections. It may be visually
  different from the canvas when that produces better surfaces.
- Use a supplied background or environment file only when the caller provides
  its explicit local file URL. Do not search for brand assets or invent them.
- Keep inactive background and environment settings intact when the operational
  workflow requires complete values.
- A moving background begins with the project and loops; do not base a concept
  on changing backgrounds at later shots.

For transparent delivery, design the device silhouette and labels to survive an
unknown downstream background, then rely on the live output capability check
for the requested format.

## Screen, labels, and image treatment

- When the product proof is on a device screen, choose orientation and fit,
  fill, or stretch deliberately. Prefer readability over edge-to-edge coverage;
  preview any crop at the delivery aspect ratio.
- Directional labels are persistent scene copy. Keep them short, maintain safe
  margins, and do not plan timed headline changes that Fan3D cannot author.
- Use contrast and saturation to support the brand palette, not to repair an
  unsuitable background.
- Use focus blur to establish depth only when the proof remains sharp at its
  important moments.
- Treat motion blur, HDR, vignette, and color fringe as accents. Combine them
  only when each has a visible job and the preview still looks intentional.

## Review the direction

Preview at least the opening, the main proof moment, and the final hold. Check:

- whether the product can be identified without explanation;
- whether screen content and labels remain readable;
- whether the device silhouette separates from the background;
- whether reflections and blur reveal form rather than obscure it;
- whether the final frame can carry the intended claim or CTA;
- whether the chosen look is coherent across all devices in the scene.

Fan3D does not expose arbitrary shader editing, geometry creation, particles,
layer compositing, or time-varying art direction through the current public
controls. Describe such ideas as external finishing or unsupported instead of
claiming they were implemented. Do not claim to have visually compared catalog
resources that the connected catalog does not make previewable.
