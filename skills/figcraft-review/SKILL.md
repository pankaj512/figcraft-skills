---
name: figcraft-review
description: Use to audit a FigCraft document or in-progress design for requirement coverage, layer structure, auto layout, visual quality, accessibility, font issues, assets, and prototype graph correctness.
---

# FigCraft review

Review the actual document as well as its rendered pixels. Load shared `figcraft` rules. Separate findings from edits; do not silently change product direction, content, navigation, or behavior while reviewing.

## Review sequence

1. **Establish scope.** Read the brief and reference manifest if available. Identify pages, screens, states, overlays, responsive frames, and required interactions.
2. **Inspect structure.** Use `inspect_document`; if output is truncated, target relevant subtrees with `get_node`. Check names, parentage, dimensions, bounds, text, fills, sizing/layout, reusable components, variables, and interactions.
3. **Inspect pixels.** Use `screenshot` for every in-scope frame/state. Examine alignment, hierarchy, text wrapping/clipping, contrast, visible states, asset rendering, and correspondence to references.
4. **Accessibility and content.** Check readable type, contrast, touch target bounds appropriate to the platform, meaningful state labels, and that status isn't conveyed by color alone. Don't claim a formal WCAG audit unless contrast and criteria were actually measured.
5. **Prototype graph.** Check entry points, valid destinations, reachability, back/close paths, overlays, component state targets, and hotspots. Distinguish structural graph inspection from actual runtime testing.
6. **Report evidence and fixes.** Cite layer names/IDs and concrete observations. If asked to fix, discover the command schemas, make targeted edits, then inspect and render again.

## Severity

- **Blocker:** a required screen/element is missing, critical content is clipped, wrong destination, or important flow cannot be reached.
- **Major:** clear visual mismatch, broken layout, inconsistent token/component behavior, inaccessible key control, or missing required state.
- **Minor:** naming, spacing, polish, or non-blocking consistency issue.

Use PASS only when every in-scope requirement has evidence. Otherwise use FAIL or NEEDS MANUAL CHECK. A successful tool response is not evidence of a visually correct design, and a successful headless preview is not proof of interactive playback.

## Report template

```markdown
# FigCraft Review
**Document:** <name/path if known>
**Overall:** PASS | FAIL | NEEDS MANUAL CHECK
**Coverage:** PASS | FAIL | N/A
**Structure/layout:** PASS | FAIL
**Visual/accessibility:** PASS | FAIL | PARTIAL
**Prototype:** PASS | FAIL | N/A | MANUAL CHECK

## Findings
- [severity] `Layer name` (id): evidence and recommended change.

## Verification limits
- <what could not be observed or measured>
```
