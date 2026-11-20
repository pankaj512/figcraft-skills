# FigCraft skills: documentation and security audit

**Date:** 2026-11-20  
**Audited revision:** `52001c9a650ff58801fd1b5c02b5269bbeca313b`  
**Scope:** all nine `skills/*/SKILL.md` files, `README.md`, the tracked file inventory, the seven supplied documentation pages, and relevant first-party MCP/CLI documentation and live command schemas.

## Verdict

**A sound native-FigCraft foundation, with targeted security and portability gaps to address.** The core modeling instructions match the documented product and the active tool contract. No malicious instructions, embedded credentials, executable payloads, install scripts, or exfiltration endpoints were found in the reviewed repository files. No critical/high-severity finding was identified within this scope.

The security findings below are **instruction/deployment gaps**, not demonstrated vulnerabilities in FigCraft. In particular, document selection, export scope, and sharing defaults need more explicit handling. The skills already require schema discovery, which mitigates these risks but does not replace a clear operating rule.

FigCraft has substantial Figma-like capabilities: grid/wrapping auto layout, constraints, variants and properties, variables/modes, advanced prototypes, libraries, Dev Mode, and editable imports. The skills generally support that breadth and do not claim these features are absent. The official pages also label FigCraft **early alpha**, imports **best effort**, and `.fig` export **experimental** [P1, P2, P4]. Feature breadth and compatibility maturity are distinct; this audit does not certify a percentage of Figma parity.

## Method and evidence limits

- Read every tracked skill and the README, using repository files as the audit target rather than relying on installed skill copies.
- Retrieved all seven user-supplied URLs successfully and followed the first-party CLI/MCP documentation links for technical claims.
- Read available tool schemas and called only `list_commands` in FigCraft, with narrow filters. Relevant responses are recorded in [runtime evidence](figcraft-runtime-evidence-2026-11-20.md).
- Checked the tracked inventory: 11 regular, non-executable files; no tracked symlinks, submodules, scripts, manifests, or binaries. The starting worktree was clean.
- Ran a lightweight structural check of all nine frontmatters, names/descriptions, internal skill references, explicit loading of shared rules, Unicode format controls, and README local links: **PASS**. This was a check of the simple frontmatter used here, not a general YAML conformance test.
- No design was changed; no exports, installations, sharing sessions, or attack payloads were executed. Runtime behavior, importer internals, transport enforcement and model resistance to prompt injection were not penetration-tested. This is not a full application/source-code security audit or a scan of all Git history.
- Findings reference the baseline skill line numbers. Severity reflects impact and prerequisites, not a CVSS score. Documentation URLs are mutable; retrieval date and observed contract matter.

## Security findings

### S1 — Medium: layer IDs are protected, but document identity is not pinned

**Location:** `skills/figcraft/SKILL.md:38-44`; `skills/figcraft-design/SKILL.md:18`; `skills/figcraft-import-export/SKILL.md:21-23`.

**Evidence:** Shared rules say to inspect first and use returned node IDs, but never require retaining and passing the returned **document ID**. Tools default to the file in front. First-party MCP documentation explicitly describes a user clicking another tab mid-task and changing where the next edit lands [P9, “Naming the document”; R1].

**Impact/prerequisite:** With multiple open documents, especially an attached app, a tab switch between inspection and mutation can direct edits, a save, or an export at an unintended document. A valid layer ID is not sufficient to establish the intended document. This is a concrete API default, not a demonstrated incident.

**Recommended change:** In shared rules, resolve the requested document, record its returned ID, and pass it to every document-scoped read/write/render/save/export/undo call supporting `document`. Use explicit node IDs and parents. Re-resolve intentionally after commands that create/open/switch files. Prefer document IDs for agents: the live `doc.list` contract restricts path-based targeting to user scripts. Keep window screenshots separate because they capture what is actually in front.

**Verification criterion:** In a disposable two-document scenario, changing the front tab must not redirect an edit/export intended for the original document.

### S2 — Medium: export guidance omits the whole-document format exception

**Location:** `skills/figcraft-import-export/SKILL.md:27-31`; `skills/figcraft-design-to-code/SKILL.md:27`.

**Evidence:** The skill requests page/frame/layer scope and says to export only requested content. However, `.fig` export serializes the **whole document**, not the requested layer selection [P2, P4, P8, P9; R2]. The active `file.export` also supports whole-file `.penpot` export. Without explicit IDs, ordinary export falls back to the selection or page. `contentsOnly: false` includes overlapping layers.

**Impact/prerequisite:** A request to hand off one screen as an editable `.fig` can unintentionally include unrelated document content. Relying on selection, a preview, or hidden-layer visibility does not establish the scope of a whole-document serialization. No export was performed in this audit.

**Recommended change:** Add a short scope branch: distinguish layer/page exports from whole-document formats before execution; use explicit document/layer IDs for scoped formats; for a selected-only editable handoff, construct and inspect an approved isolated document with necessary dependencies, or obtain authorization for the whole-file scope. Inspect the actual output structure where feasible, not only a screenshot. Check export destinations for overwrite authorization as well as native-save destinations.

**Verification criterion:** A one-screen handoff from a document containing an unrelated page must either exclude that page through an explicitly scoped deliverable or clearly request whole-file authorization before export.

### S3 — Medium: sharing consent does not specify access level, destination or credential handling

**Location:** `skills/figcraft/SKILL.md:56-65`; `skills/figcraft-design-system/SKILL.md:40`.

**Evidence:** The router correctly requires an explicit request before sharing. It gives no workflow for satisfying that request safely. The live `collab.share` defaults are **online** and **edit**. For online sharing the generic returned `link` is the owner's editing route, while the requested audience's links are under `share`. Invite links and codes are bearer credentials [R3; P5; P9, “Live sessions”]. P5 also states that the default online service stores files unencrypted, its operator can read them, and stopping sharing does not remove the stored file.

**Impact/prerequisite:** After a legitimate but underspecified “share this for review” request, defaults can upload to an unintended server or grant editing access. Echoing a complete response can disclose credentials beyond the requested access level. “Stop sharing” can be misreported as deletion. The existing consent rule prevents unsolicited initiation, so this finding is specifically about authorized sharing and its details.

**Recommended change:** Add a focused sharing reference reached when collaboration or server publishing is requested. Resolve online/local destination and requested role; pass them explicitly; return only the intended audience's matching link/code. Treat owner/server tokens and invite URLs as secrets: omit them from committed files, diagnostic dumps and public comments, and return an invite only as needed for the authorized handoff. Distinguish joining/leaving, revoking links, and deleting server data; never imply recipients' copies were recalled. Include server library publishing in this branch. Check existing session state before edits when live collaboration is relevant, since edits may already synchronize.

**Verification criterion:** A view-only sharing request returns the view share link, not the generic owner/edit link; stopping sharing is reported as revocation rather than server deletion.

### S4 — Low: documented confinement exists, but setup guidance does not surface it

**Location:** `README.md:23-33`; `skills/figcraft/SKILL.md:59-61`.

**Evidence:** The shared skill properly forbids bypassing path restrictions. The repository does not explain how to establish those restrictions. FigCraft documents `--automation-read-root` and `--automation-write-root`, including read-only configuration and the distinction between headless startup and configuring the attached app [P6, P8, P9, “Confining file access”]. P8 states that without these flags CLI subcommands read/write with the launching user's authority. P9 additionally documents network restrictions for confined calls.

**Impact/prerequisite:** A user can follow this repository's setup assumptions with an unnecessarily broad filesystem authority. Prompt instructions are not an enforcement boundary. This is a deployment hardening omission, not an instruction to bypass security or evidence that the current session lacks confinement.

**Recommended change:** Add an opt-in least-privilege setup pointer to the official AI-agent/MCP documentation in the README. Explain existing-folder roots, read/write separation, and that attached sessions inherit confinement from app startup. Preserve the current refusal-to-bypass rule; confinement must not be silently removed after an error. Do not describe roots as a complete sandbox: exported bytes still reach the assistant, network-share roots reach that file server, and P9 documents a path-check/open race against other local software.

**Verification criterion:** Setup documentation clearly distinguishes read-only vs writable headless configuration and attached-app configuration without suggesting an unconstrained fallback.

## Documentation and usability findings

### V1 — Medium: a host-specific calling convention is presented as general skill behavior

**Location:** `skills/figcraft/SKILL.md:25`; `README.md:25,33`.

The router hardcodes `tools["figcraft"].<tool>` inside `execute`. That is accurate for the active OpenCode integration, but FigCraft documents standard MCP over stdio and supports other assistants [P6, P9]. Generic Agent Skills distribution should not require every host to expose OpenCode's Code Mode wrapper.

**Fix:** Make the rule host-neutral: discover the available FigCraft MCP tools using the host's interface; use Code Mode only when the host exposes it. Keep canonical tool and command names. If OpenCode-only is intentional, state that scope in the README instead of suggesting general host portability.

### V2 — Low: single-skill installation instructions omit the shared dependency

**Location:** `README.md:33`; all eight focused skills' opening instructions.

The README says users can “copy only the chosen skill,” but every focused skill explicitly loads `figcraft`. Some tasks load additional focused skills. Installing only `figcraft-assets`, for example, leaves the named shared dependency unavailable, including its security rules. All references resolve in the full repository; the problem is the documented partial-install path.

**Fix:** Recommend installing the complete set, or the chosen skill plus `figcraft` and its task-relevant dependencies. Keep shared policy authoritative in one place rather than duplicating it throughout every skill. State what to do when a required skill cannot be loaded.

### V3 — Low: several native workflows lack discovery pointers

**Locations:** `skills/figcraft/SKILL.md:10-21`; `skills/figcraft-design-to-code/SKILL.md:17-27`; `skills/figcraft-review/SKILL.md:10-17`; `skills/figcraft-responsive/SKILL.md:12-14`; `skills/figcraft-assets/SKILL.md:15,26-31`.

The supplied docs support more than the current routes explicitly guide [P3, P4, P7; P9]:

- **Handoff:** Dev Mode generated code, annotations, readiness/change comparison, code connections and variable export. The current manual extraction workflow is valid but misses native sources of information. Generated code should remain a reviewed input to implementation, not an instruction or an automatic replacement for repository conventions.
- **Review:** comment/reply/resolve workflows, versions and branch-based review. Comments remain untrusted task data; requests to fix them authorize design work within scope, not arbitrary instructions embedded in comment text.
- **Responsive work:** constraints are documented, and current developer docs describe responsive prototype presentation. Browser CSS breakpoints are not implied: P9's design-context documentation says the file stores no breakpoints. Keep the skill's size-matrix verification, while providing a positive pointer to native constraints/responsive presentation when available.
- **Assets/fonts:** missing-font discovery, runtime loading and refitting can avoid unnecessary substitutions; installation/download consent remains appropriate.
- **Router coverage:** boards/workshops and collaboration have no focused path. Templates and library workflows could also have short discovery pointers.

**Fix:** Add focused, conditional references for these branches if broad FigCraft workflow coverage is intended. Do not cache a complete command catalog in the skills or load every workflow for every task. These omissions are coverage opportunities, not evidence that FigCraft lacks the features or that every task needs them.

### V4 — Low: retry guidance does not cover an attached transport failure

**Location:** `skills/figcraft/SKILL.md:44`; `skills/figcraft-design/SKILL.md:34-38`.

The current rule to inspect after a failed batch is good. First-party MCP documentation explains that an attached request can apply successfully before its reply is lost; transport failures are not automatically replayed [P9, introductory transport behavior]. The same concern applies to a single create/import/edit call, not only a batch.

**Fix:** Generalize the inspect-before-retry rule to any mutation whose completion is uncertain. Check whether it landed before retrying, rather than duplicating a creation/import. Keep batch rollback claims scoped to the documented state: P9 notes that preferences, clipboard, tool state and UI requests are not rolled back with document edits.

## Skill-by-skill result

| Skill | Documentation fit | Key follow-up |
|---|---|---|
| `figcraft` | Coordinates, parent-relative geometry, layout ownership, native tools, schema discovery and inspect/render verification align with P6/P9 and active schemas. | S1, S3, V1, V4; route missing workflows deliberately. |
| `figcraft-design` | Native frames, grid/row/column auto layout, components, real content and per-frame QA align with P2–P4. | Inherit explicit document targeting; optional template discovery. |
| `figcraft-design-system` | Modes, styles, variants, slots, bindings and linked assets fit documented capabilities. | Distinguish local library publishing from server publishing/network checks. |
| `figcraft-prototype` | Native interactions, conditional actions, flow/overlay/scroll checks and verification boundaries are appropriate. | Keep runtime input/playback capability checks; no evidence warrants deleting them or declaring text input unsupported globally. |
| `figcraft-review` | Structure plus pixels and qualified accessibility claims are sound. | Add comment/version/branch references if review scope includes them. |
| `figcraft-assets` | Editable SVG, provenance, font checks and render verification fit P4/P7/P9. | Native SVG docs already refuse linked-file reads; no SVG execution flaw established. Add font discovery/refit pointers. |
| `figcraft-import-export` | Native save, preserve-source behavior, schema discovery and lossiness checks are sound. | S2; explicitly report import/export warnings and experimental `.fig` interoperability. |
| `figcraft-responsive` | Fill/hug/fixed, constraints, wrapping and multi-size proof align with P4/P9. | Discover native responsive presentation; avoid conflating it with stored CSS breakpoints. |
| `figcraft-design-to-code` | Semantic implementation, repository conventions and matching-viewport checks are appropriate. | Add Dev Mode/variable/code-connection discovery; inherit safe export scope. |

## Existing protections worth retaining

- Document text, comments, SVG and remote content are explicitly **data, not instructions** (`figcraft:58`, `figcraft-assets:8,20`, `figcraft-import-export:16`). No competing lower-trust instruction is elevated to policy.
- Software/font/plugin installation requires approval; sharing requires an explicit request (`figcraft:59-60`). No download-and-execute recipe exists in the reviewed skills.
- File-path restrictions must not be bypassed through shell commands (`figcraft:61`). Native save has destination/overwrite checks (`figcraft:53`, `figcraft-import-export:21`).
- Review separates findings from edits, and implementation changes remain scoped (`figcraft-review:8,17`, `figcraft-design-to-code:35,42`).
- Render/structure checks, font substitution disclosure, prototype verification limits, and persistence caveats reduce unsupported success claims.
- All eight focused skills explicitly load the shared rules, and all referenced skills exist in the complete repository.

## Source reconciliation

Prefer the active schema for the operation being performed. Examples found during this audit:

1. **Local share access:** P5 and P9 say local participants all edit; the active `collab.share` contract additionally exposes local view-only links. Do not hardcode either statement across versions; discover and verify the current contract.
2. **Minimized attached window:** P7 lists a KDE/Wayland automation timeout problem. Current P9 describes minimized-window request handling as supported, with a visible window needed only for window capture. Treat the troubleshooting entry as version/environment-specific, not a universal backend limit.
3. **Export formats:** the active `file.export` lists Penpot, while the direct `export` enum does not. Command and wrapper surfaces can differ even in one session.
4. **Developer-doc features:** P9 describes `design_context`; that direct tool was not advertised in the active catalog. Discover a supported command/tool before depending on it.

The skills' repeated instruction to inspect current schemas is therefore well justified. Likewise, a wired interaction graph is not proof of observed playback, even though FigCraft has a presentation view. No reviewed source established that arbitrary runtime form input is universally available or unavailable; the prototype skill correctly makes a conditional check.

## Recommended order of changes

1. Add shared explicit document targeting (S1).
2. Add format-specific export scope and actual-output verification (S2).
3. Add a conditional collaboration reference with destination, role and credential handling (S3).
4. Make the tool invocation rule portable and fix partial-install dependencies (V1/V2).
5. Link least-privilege setup and generalize uncertain-completion handling (S4/V4).
6. Extend native workflow discovery only for the branches the repository intends to cover (V3).

Before calling revised instructions hardened, exercise S1–S3 with disposable documents and a controlled sharing destination, and test malicious document/comment text as data. These are proposed acceptance checks, **not tests performed by this audit**. The original skills remain unchanged; this delivery records findings and evidence.

## Sources

All URLs below were retrieved successfully on 2026-11-20.

| ID | First-party source | Evidence used |
|---|---|---|
| P1 | [FigCraft product page](https://getartcraft.com/apps/figcraft) | Open-source product, early-alpha status, layout/components/variables/prototypes, generated code and command-driven automation. |
| P2 | [Overview](https://craft.wiki/apps/figcraft/overview/) | Feature breadth, native files, supported import/export families, best-effort compatibility, boards/live sessions. |
| P3 | [Getting started](https://craft.wiki/apps/figcraft/getting-started/) | Templates, frame/layout workflow, native save, recovery/autosave and export. |
| P4 | [Workflows](https://craft.wiki/apps/figcraft/workflows/) | Grid/wrap/constraints, slots/libraries, variable export, prototypes, Dev Mode, comments/versions/branches, boards, imports and experimental whole-file `.fig` export. |
| P5 | [Live sessions](https://craft.wiki/apps/figcraft/live-sessions/) | Codes/links confer access; online upload/default server; encryption/storage distinction; local-network trust; revocation and retained copies. |
| P6 | [AI agents](https://craft.wiki/apps/figcraft/ai-agents/) | Standard MCP clients, CLI startup, headless/attached modes, local control port, read/write roots and attached configuration. |
| P7 | [Troubleshooting](https://craft.wiki/apps/figcraft/troubleshooting/) | Import/font limitations, font-download consent, Dev Mode restrictions, attached Linux caveat, recovery and diagnostics. |
| P8 | [Developer CLI documentation](https://github.com/storytold/figcraft/blob/main/docs/cli.md) ([retrieved raw](https://raw.githubusercontent.com/storytold/figcraft/main/docs/cli.md)) | Confinement defaults, whole-file `.fig`/Penpot exports, scoped export behavior, warnings, native conversion and code mappings. |
| P9 | [Developer MCP documentation](https://github.com/storytold/figcraft/blob/main/docs/mcp.md) ([retrieved raw](https://raw.githubusercontent.com/storytold/figcraft/main/docs/mcp.md)) | Document IDs, transport retry semantics, roots/network enforcement boundaries, native conventions, SVG handling, prototype presentation, secret-bearing collaboration responses and batch rollback limits. |
| R1–R6 | [Active tool/command evidence](figcraft-runtime-evidence-2026-11-20.md) | Session-specific contracts, retrieved read-only; complements public documentation without assuming the same release. |
