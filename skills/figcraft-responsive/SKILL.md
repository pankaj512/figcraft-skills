---
name: figcraft-responsive
description: Use when designing or checking FigCraft layouts across device sizes, breakpoints, orientation changes, content lengths, or overflow conditions.
---

# Responsive FigCraft layouts

Load shared `figcraft` rules and use `figcraft-design` for initial construction. Responsive behavior must be demonstrated at relevant sizes; a single well-sized screen is not proof of responsiveness.

## 1. Establish size coverage

- Identify target platforms and the smallest/largest required viewport. Use supplied dimensions when present.
- If the task requires platform presets, discover current `frame.presets` through `list_commands`; don't rely on memorized device values.
- Determine whether the document supports true responsive constraints or whether the deliverable requires separate breakpoint frames. Don't promise browser-like reflow if only fixed frames are being constructed.
- When the deliverable is a presented prototype, discover the current presentation settings (`proto.settings`) and presentation or preview commands (`proto.present`, `proto.preview`). A responsive presentation option can resize frames to the screen so their constraints and auto layout respond. A presentation or preview is evidence only when it was actually observed; the size matrix below remains the required check.
- Include representative content stress: long title, long localized label, multiline body, empty/loading/error state when in scope.

## 2. Choose layout rules per region

- Use auto layout for content-driven stacks and rows. Define padding, gap, alignment, wrapping/sizing, and min/max behavior where supported.
- Use fill/hug/fixed sizing deliberately. Avoid fixed heights for variable text unless clipping or scrolling is intentional.
- Decide which columns collapse, wrap, hide, or become horizontally scrollable at narrow widths. Make each decision explicit rather than shrinking every control.
- Keep key actions and navigation visible and reachable. Respect safe areas or reserved system UI only if relevant to the target context.
- Use fixed/absolute positioning only for elements that should truly remain pinned or overlay content.

## 3. Verify a size matrix

For each required breakpoint/size:

1. Inspect frame dimensions, child bounds, auto-layout settings, overflow/clipping, and text properties.
2. Render a screenshot at that size and inspect wrapping, collisions, truncation, alignment, contrast, and visible action hierarchy.
3. Check the extreme content case at the narrowest relevant width.
4. Fix breakpoint-specific issues with the least disruptive rule change, then inspect and render again.

If only one viewport can be produced or observed, state that limitation. Do not infer responsive success from constraints in the layer tree alone.

## Delivery

List the viewports/states checked, key layout rules, concrete verification performed, and remaining sizes or runtime behaviors that need manual testing.
