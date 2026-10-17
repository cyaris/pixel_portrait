# Repository Guidance

## Auto Transition Timing

- Keep every Auto Transition mode's full-set completion duration and inter-set delay independently configurable. Define
  what completes a set per mode—for example, one perimeter for Frames and the full photo for Diagonal—without deriving
  cadence from transition lifecycle. Keep the effective gap constant across completed sets, treat the configured delay
  as a minimum, and add a constant safety floor when needed so a new set cannot reach pixels still transitioning.
- Keep mode-specific timing invariants local. Calculate the built-in Diagonal delay from the transition reuse lifecycle
  at the baseline 30-slice cadence, then apply the default timing multiplier independently. Do not change pixel movement
  or fade durations to create the gap.
- Keep `transitionReuseDuration`'s extra `transitionDuration` beyond full visual restoration. It reproduces the
  pre-Canvas D3 rewrite's separate post-restore stroke-width settle transition before a pixel became reusable, and the
  Diagonal delay default derives from it.

## Data Generation

- Regenerate and commit `frontend/src/lib/static/pixels.json` after changing the source image or a pixel-generation
  setting.

## Pixel Grid Rendering

- Draw each pixel-grid boundary once, per `../shared-automation/agents/charts.md`, in `drawPixelSeparators` rather
  than as a per-pixel `strokeRect` in a pixel display function. Derive its alpha from the more-visible neighboring
  cell and its position from the device-pixel-snapped `columnPositions`/`rowPositions` lookup tables
  `getSeparatorBuffers` builds once per geometry.
- Keep `separatorAlpha` at `0.31` unless deliberately changing the cross-device design. It reproduces the original
  `0.075` CSS-pixel SVG hairline at 2x density and must remain the same on desktop and mobile.
- Laser-eye circles intentionally begin as solid dots and open into rings as they grow, through `strokeOrFillCircle`.
- Scale the laser-eye starting radius and stroke width from the rendered portrait width, preserving the original values
  at the 400 px source-image width so responsive layouts retain the same proportions.

## Rollup Delivery

- Project-specific Rollup inputs include the S3 prefix, bundle file list, and a local dependency ref for `fireworks`.
