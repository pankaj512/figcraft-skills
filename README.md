# FigCraft Skills

> **Status:** Early draft; review the current MCP tool schemas before relying on operational details. Contributions and corrections are welcome.

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
| [`figcraft-collaboration`](skills/figcraft-collaboration/SKILL.md) | Share, collaborate, publish, and manage team libraries |
| [`figcraft-responsive`](skills/figcraft-responsive/SKILL.md) | Responsive layout planning and size-specific verification |
| [`figcraft-design-to-code`](skills/figcraft-design-to-code/SKILL.md) | Translate inspected FigCraft designs into implementation code |

## Operating assumptions

- Use the active FigCraft tools when your host exposes them, as direct MCP tools or through a Code Mode wrapper such as `tools.figcraft.*`. Check their schemas and the current command catalog rather than relying on guessed or memorized parameters.
- The current integration can be headless. A rendered screenshot is useful evidence of document pixels; it does not prove a GUI opened or that prototype playback was exercised.
- Do not install FigCraft, CLI tools, fonts, skills, or other software without explicit authorization.
- Inspect the target document before changing it. Preserve user content and do not save unless requested or necessary to deliver a requested file.
- Save editable native design files as `.figcraft` when requested.

## Use

These are plain Agent Skills format directories. Configure your agent host to discover the `skills/` directory, or copy a skill into a supported skills location. Creating this repository does not install or activate the skills.

Install the complete set, or the chosen skill plus `figcraft` and the focused skills its tasks need. Every focused skill loads the shared `figcraft` rules, including its security and consent policy, so a focused skill installed alone is incomplete. When a required skill cannot be loaded, the agent should stop and ask for it to be installed rather than proceeding without its guidance.

## Confine file access

File-path rules in these skills are instructions, not an enforcement boundary. FigCraft can enforce the boundary itself; see the official [AI agents documentation](https://craft.wiki/apps/figcraft/ai-agents/) and [MCP documentation](https://github.com/storytold/figcraft/blob/main/docs/mcp.md).

- `--automation-read-root` and `--automation-write-root` restrict access to existing folders. Read and write roots can be separate, and a read-only setup is possible.
- For a headless session, pass the flags on the command that starts it. An attached session inherits the confinement the app was started with, so configure it there rather than per call.
- Without these flags, CLI subcommands read and write with the launching user's authority. Confined calls also have documented network restrictions.
- Confinement is not a sandbox. Exported bytes still reach the assistant, a network-share root reaches that file server, and the MCP documentation records a path-check/open race with other local software.
- If a call is refused, report the blocker and ask for an allowed path. Do not restart with broader roots or remove confinement to work around an error.

## License

MIT. See [`LICENSE`](LICENSE).
