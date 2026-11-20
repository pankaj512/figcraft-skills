# AGENTS.md

Instructions for AI coding agents contributing to FigCraft Skills.

Claude Code, Codex, and every other AI tool must not add AI attribution to
commits or pull requests, including AI `Co-Authored-By` trailers or generated-by
signatures. Preserve genuine human attribution.

## What this is

A native-MCP-first collection of Agent Skills for designing, editing, reviewing,
prototyping, and handing off FigCraft documents. The intended audience is general
FigCraft users across compatible AI agent hosts.

The product is nine Markdown skill documents under `skills/`: one shared
`figcraft` router and eight focused skills. `README.md` is the public usage guide;
`docs/audits/` contains dated findings and runtime evidence. This repository has
no application runtime or framework.

## Proportional engineering

Build for established requirements, not hypothetical scale, threats, or future
flexibility. Reuse existing code, the standard library, native platform features,
and installed dependencies before adding machinery.

- Unknown scale or extensibility defaults to the smaller reversible design. Do
  not infer enterprise, multi-tenant, hostile-user, or compliance requirements.
- Derive trust and data-integrity boundaries from actual reachability: untrusted
  input, auth/session/ownership, shared persisted data, destructive operations,
  payments, secrets, and sensitive data.
- Ask only when an unknown materially changes behavior, architecture, persisted
  data, interoperability, a real security boundary, or cost. Otherwise choose the
  simplest repository-native implementation.
- Add an abstraction, dependency, service, configuration surface, compatibility
  layer, or security mechanism only for a current requirement.
- Simplicity never removes real trust-boundary validation, data-loss prevention,
  accessibility, explicit security requirements, configured tests, or project rules.
- Stack-specific template standards apply only when the project uses that stack.

## Contribution conventions

- Keep `skills/<skill-name>/SKILL.md`, with YAML `name` matching the directory
  and a `description` that identifies when the skill should be invoked.
- Read the shared `skills/figcraft/SKILL.md` and affected focused skills before
  changing operational guidance. Keep shared policy in the router and focused
  workflows in their own files.
- Use current FigCraft tool schemas and discovered command contracts as the
  operational authority. Preserve native document modeling, user content,
  consent requirements, and the distinction between data and instructions.
- Write concise Markdown with explicit steps and completion criteria. Check
  references and links. Add branch-specific guidance through focused pointers.
- Preserve dated audits as evidence of their stated revision and scope; record
  later remediation separately.
- Use plain punctuation in new prose: hyphens, commas, colons, or parentheses.
  Comments should explain non-obvious decisions rather than restating code.
- Keep changes focused and reviewable. Obtain permission before commits or
  external actions. Use conventional commit messages when a commit is authorized.

## Commands

No dev server, build, production server, lint, test, browser-test, or combined
Verify command is configured. There is no product manifest or lockfile.

- Whitespace check for tracked changes: `git diff --check`

Testing is opt-in and not yet set up. A declared `test` command in this section
will enable the test gate for logic-bearing changes; use that command once it
exists. Behavioral skill evaluations are planned, with no harness or run command
selected yet. Add tooling only through an explicit setup task.

For documentation changes, check frontmatter, skill references, local links,
and consistency with the current tool contract where relevant. Report the
checks actually run; whitespace checks do not prove skill behavior or inspect
untracked files. No GitHub Actions workflow is currently configured.
