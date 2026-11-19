---
name: figcraft-prototype
description: Use when wiring or revising native FigCraft prototype interactions, navigation, overlays, scroll behavior, component state changes, conditional actions, or motion.
---

# FigCraft prototyping

Wire behavior only after the needed screens, overlays, and component states exist. Load shared `figcraft` rules first. If the task is visual-only, do not add behavior without request.

## 1. Define expected behavior

Create an interaction inventory before wiring:

| Source | Trigger | Action | Destination/state | Close/back | Expected result |
|---|---|---|---|---|---|

Include every requested control and relevant terminal/error/cancel path. Ask if navigation, validation, dismissal, or outcome is unclear. Do not invent screens or backend responses.

## 2. Discover exact capabilities

Use `list_commands` with focused filters such as `proto.interaction`, `proto.flow`, `overflow`, `variable`, or `component`. Use only the current returned ids and schemas. Do not transplant parameter examples from Figma plugins or older FigCraft versions.

The engine may accept a syntactically valid interaction that is semantically wrong. Independently verify that destinations exist, state changes target real variants, and required flows are reachable.

## 3. Wire deliberately

- Attach a hotspot to the visible control container whenever possible, not only its label or icon.
- Set explicit flow start points when a prototype flow is requested.
- For navigation, confirm the target frame and expected back behavior.
- For overlays, wire the open path and the intended close/dismiss path. Outside dismissal should match the task, especially for destructive or required decisions.
- For component states, prefer supported variants and state changes over unrelated duplicate frames.
- For scrolling, create a viewport and oversized content track with the intended clipping and direction. Verify actual viewport/track dimensions; do not infer scrolling merely because an overflow property was set.
- Choose motion that explains the state change. Keep transitions restrained and consistent. Discover transition names and values at runtime.

## 4. Editable input boundary

An agent changing design text with `type_text` is not evidence that a person can type into a field during prototype playback. Recheck the current command/tool surface before promising runtime text entry. If arbitrary typing is unavailable, explain that and ask whether a clearly labeled simulated flow with representative states is acceptable. Never describe simulated values as a functional backend form.

## 5. Verify honestly

1. Inspect interaction records and page flows after wiring. Confirm sources, target IDs, actions, transition settings, and that targets are still valid after layout changes.
2. Trace required paths as a graph: entry, success, error/cancel, back, close, and terminal states as applicable.
3. Inspect final hotspots and bounds; ensure they align to the visible controls and do not cover unrelated controls.
4. Take screenshots of relevant screens/overlays to verify visual states.
5. Distinguish stored interaction wiring from runtime playback. In a headless session, a successful command or preview call may not exercise a visible prototype player. List user click-through checks that remain.

Report what is wired, what was structurally inspected, whether playback was observable, and any manual checks or simulated behaviors.
