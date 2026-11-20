---
name: figcraft-design
description: Use when creating or revising FigCraft UI screens, mobile or desktop mockups, product flows as static screens, or low- to high-fidelity wireframes from a brief, codebase, or visual reference.
---

# FigCraft screen design

Build an editable design that follows the user's brief. Load the shared `figcraft` skill first for tool rules. Load `figcraft-design-system`, `figcraft-assets`, or `figcraft-responsive` only when the task needs them.

## 1. Define scope before drawing

Extract the target audience and job, platform/viewport, requested screens/states, supplied copy/assets, design direction, and output path. If references are supplied, map each to a screen, state, overlay, or breakpoint. Ask when ambiguity changes content, navigation, or the design materially. Do not add unrequested flows or screens.

For a greenfield design, choose a clear visual premise tied to the product (not a generic template), then establish a small coherent palette, type hierarchy, and spacing rhythm. Respect a provided brand or reference over personal defaults. Real, specific copy is preferable to lorem ipsum; never invent factual product claims.

## 2. Inspect and plan

1. Resolve the target document and record its ID, then call `inspect_document` with that ID before mutation. Check current page, existing frames, reusable components, variables, and available space. Use `get_node` with the same document target for important subtrees if the tree is truncated.
2. If the design needs device dimensions, query `frame.presets` with `list_commands` and use a currently listed preset; do not rely on stale preset names. If the user provides exact dimensions, honor those.
3. Sketch the hierarchy in a short plan: screen → sections → components/content. Identify fixed, scrolling, and overlay areas.
4. Declare the design's structural intent: which frames use auto layout and which elements are intentional absolute overlays or decoration.

## 3. Build natively

- Use a screen frame with clear semantic name, and nested section/container frames as appropriate.
- Use vertical/horizontal/grid auto layout for repeated content and controls. Set parent layout, padding, gap, alignment, and child sizing intentionally. Auto layout owns children’s position; avoid child x/y in a flow.
- Use absolute positioning only for true overlays, pinned controls, or decorative elements where flow would be wrong.
- Give text layers a sensible width/sizing behavior so paragraphs wrap. Check short and long copy.
- Use semantic names such as `Screen/Sign in`, `Section/Account`, `Field/Email`, `Button/Continue`.
- Use components when repeated or stateful elements benefit from being reusable. Avoid abstracting one-off items without a clear editing benefit.
- Make key visual choices purposeful: hierarchy, spacing, contrast, alignment, and restraint. Avoid repetitive card grids and decoration that carries no information unless requested.
- Preserve supplied artwork and assets; do not invent a brand mark or detailed illustration. Use `figcraft-assets` for asset work.

## 4. Work in coherent batches

Use the dedicated `create_node` tool for straightforward node creation and `set_properties` for targeted edits. Use `batch` for a coherent section or subtree that benefits from one undo step. Pass the recorded document ID to every batch or command that accepts a document target. Name steps and chain results via `$ref` rather than predicting node IDs. For commands, query `list_commands` first and pass the exact current schema. Keep batches small enough to diagnose.

If a batch fails or its completion is uncertain, inspect the document before retrying, then retry only the missing part rather than replaying the batch.

If creating a multi-screen design, build the shared foundation first, then one screen/section at a time. Inspect and render at milestones rather than generating the entire deliverable without feedback.

## 5. Verify before delivery

For every requested frame:

- Inspect hierarchy, dimensions, fills, text, auto-layout sizing, and bounds with `inspect_document` / `get_node` in the recorded document.
- Take a targeted `screenshot` of the relevant frame(s) in that document and visually assess alignment, wrapping, contrast, clipping, missing elements, and the brief's visual direction.
- Fix concrete issues with targeted edits, then inspect and render again.
- Check important text and touch target sizing where relevant. Report font limitations rather than assuming a font exists.

A successful tool call is not visual QA. A headless screenshot does not mean the user has seen a GUI preview. Do not export a screenshot as a deliverable unless asked.

## 6. Save and report

Save an editable `.figcraft` only when the user requested a saved file or delivery path. Pass the recorded document ID to the save operation where the current schema accepts it, and save to their exact target; do not overwrite a preexisting file without permission. If the save result is uncertain, inspect the document and destination before retrying. Then inspect the saved document state and report the path, screens/frames built, verification performed, assumptions, and unresolved limitations.
