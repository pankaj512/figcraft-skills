---
name: figcraft-design-system
description: Use when creating or auditing a FigCraft design system: reusable color/spacing/type variables, themes or modes, paint/text/effect styles, components, variants, and instances.
---

# FigCraft design systems

A design system should make the requested work more consistent and maintainable. Load shared `figcraft` rules first. Do not turn a request for one isolated screen into an unsolicited library project.

## 1. Discover before adding

1. Inspect the document and relevant pages. Identify existing variables, modes, styles, components, instances, naming conventions, and whether the system is local or library-linked.
2. Query current command schemas with `list_commands` for relevant `variable`, `style`, `component`, or `instance` actions. Exact capabilities and argument names vary; use the current registry.
3. Map the requested design needs to existing assets. Reuse appropriate existing values/components instead of duplicating them.
4. Ask before changing existing shared tokens or component behavior when downstream impact is plausible.

## 2. Define foundations

- Use semantic names that communicate purpose (for example surface, text, border, action, state) rather than only raw color values.
- Create variables for reusable values and modes only when the requested scope needs them. Keep values organized into collections/modes supported by the current schema.
- Use styles for composite treatments such as text, paint, effects, or grids when reuse is valuable.
- Keep typography, spacing, radii, and color roles coherent. For themed systems, define each mode intentionally; do not create dark mode by mechanically inverting colors.
- Bind variables/styles to relevant properties where supported; don't claim a layer is tokenized based only on matching its hex value.
- Check contrast for text and important boundaries. A token set is not automatically accessible just because it is centralized.

## 3. Build components and variants

1. Select an anchor component based on the actual task (for example, a button or input), and establish its structure before multiplying variants.
2. Use a component for genuinely reused or configurable UI. Keep semantic child names so overrides remain understandable.
3. Define only necessary variant axes (e.g. size, tone, state). Avoid a combinatorial explosion of duplicate components.
4. Use exposed text/boolean/instance-swap/slot properties only where the current engine supports them and they improve the component API.
5. Create instances for screen composition and preserve the relationship to the main component. Do not detach just to apply ordinary text or supported property overrides.
6. Inspect component property definitions, instance overrides, and variable/style bindings after edits. Confirm changes to the main component propagate as intended.

## 4. Verify and document

- Inspect component trees, variant/property definitions, bindings, and relevant instance overrides.
- Render representative variants and any requested theme modes. Include edge cases such as long labels, disabled or error states if they are in scope.
- Fix mismatches and repeat inspection/rendering.
- Report the created or reused tokens/components, supported modes/states, verification, and known limitations. Don't claim library publishing or cross-file availability unless explicitly performed and verified.
- For handoff, `variable.export` returns or writes JSON, CSS, Tailwind, or Dart from the file's own collections. Writing a path follows the destination and overwrite rule in the shared skill.
- If the system is library-linked or published, use `library.list` and `library.updates` to see enabled libraries and pending updates, and follow `figcraft-collaboration` for publishing. Do not describe a local change as available to other files until a publish was performed and verified.
