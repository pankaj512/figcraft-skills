# FigCraft Skills

An original, native-MCP-first skill set for designing, editing, reviewing, prototyping, and handing off FigCraft documents.

These skills were authored from scratch after comparing publicly available FigCraft-related skills and guides. They are **not installed copies** of those packages. Third-party references informed workflow ideas only; Figma Plugin API recipes, stale schemas, and unverified CLI assumptions were excluded. The FigCraft MCP instructions and live tool schemas available in the active session take precedence over this repository's guidance.

## Skills

| Skill | Use it for |
|---|---|
| [`figcraft`](skills/figcraft/SKILL.md) | Router and shared operating rules for any FigCraft task |
| [`figcraft-design`](skills/figcraft-design/SKILL.md) | Build or revise screens, UI mockups, and wireframes |
| [`figcraft-design-system`](skills/figcraft-design-system/SKILL.md) | Variables, modes, styles, reusable components, and variants |
| [`figcraft-prototype`](skills/figcraft-prototype/SKILL.md) | Clickable flows, overlays, states, and motion |
| [`figcraft-review`](skills/figcraft-review/SKILL.md) | Structural and visual review, accessibility, and prototype audit |
| [`figcraft-assets`](skills/figcraft-assets/SKILL.md) | SVG, icons, imagery, and typography decisions |
| [`figcraft-import-export`](skills/figcraft-import-export/SKILL.md) | Import, save, export, and file-format handoff |
| [`figcraft-responsive`](skills/figcraft-responsive/SKILL.md) | Responsive layout planning and size-specific verification |
| [`figcraft-design-to-code`](skills/figcraft-design-to-code/SKILL.md) | Translate inspected FigCraft designs into implementation code |

## Operating assumptions

- Use the active FigCraft tools (`tools.figcraft.*`) when available. Check their schemas and the current command catalog rather than relying on guessed or memorized parameters.
- The current integration can be headless. A rendered screenshot is useful evidence of document pixels; it does not prove a GUI opened or that prototype playback was exercised.
- Do not install FigCraft, CLI tools, fonts, skills, or other software without explicit authorization.
- Inspect the target document before changing it. Preserve user content and do not save unless requested or necessary to deliver a requested file.
- Save editable native design files as `.figcraft` when requested.

## Installation/use

These are plain Agent Skills format directories. To use them, configure your agent host to discover the `skills/` directory or copy only the chosen skill into a supported skills location. No installation has been performed by creating this repository.
