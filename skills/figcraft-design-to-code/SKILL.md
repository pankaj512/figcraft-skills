---
name: figcraft-design-to-code
description: Use when implementing an inspected FigCraft design in a software codebase or preparing a design handoff with measured structure, assets, and interaction details.
---

# FigCraft design-to-code handoff

This skill connects a native design document to an implementation. Load shared `figcraft` rules; use `figcraft-review` for the final design audit, and follow the codebase's own conventions and instructions before editing application code.

## 1. Confirm implementation scope

- Identify target repository/framework, route or component, supported platforms, and requested fidelity.
- Inspect existing project structure and patterns before changing code. Do not introduce a new framework or dependency without need and authorization.
- Identify which FigCraft frames/states are authoritative. Ask about ambiguous differences between references and document.

## 2. Extract design facts

Use `inspect_document` and `get_node` to capture:

- Frame dimensions, hierarchy, layout direction, gaps, padding, alignment, and child sizing.
- Typography: family, weight, size, line height, letter spacing, wrapping and truncation.
- Colors, radii, borders, shadows/effects, and variable/style bindings.
- Repeated components, variants, instance overrides, and image/vector assets.
- Interaction inventory and target states (load `figcraft-prototype` when interaction behavior is requested).

Take screenshots as visual references, but do not treat a screenshot as the sole implementation specification when editable structure exists. Verify any asset export and preserve attribution/licensing information supplied by the user.

## 3. Implement semantically

- Translate the design into the codebase's established component and token patterns.
- Preserve semantic HTML/native controls, keyboard behavior, accessible names, focus states, and platform conventions.
- Reuse existing assets and tokens. If an exact font/image is unavailable, identify the fallback rather than silently misrepresenting fidelity.
- Implement requested states and interactions only. Don't claim backend validation or API behavior unless it exists in the project.
- Keep code changes scoped; avoid refactoring unrelated areas.

## 4. Validate against the design

- Run relevant tests, type checks, or builds available in the repo; report commands and results accurately.
- Compare the implementation with FigCraft screenshots at matching viewport sizes. Check layout, text wrapping, colors, assets, and all requested states.
- Exercise interactions with available tests or browser tooling if appropriate. Distinguish automated checks from manual/runtime coverage.
- If implementation feedback requires a design change, return to FigCraft using `figcraft-design`; don't silently alter the source document during code handoff.

## Handoff report

Summarize changed files, design frames/states mapped, verification commands/results, visual differences, asset/font substitutions, and what remains unimplemented or untested. Be precise about partial fidelity.
