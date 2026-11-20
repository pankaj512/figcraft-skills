# FigCraft runtime contract evidence

Audit date: 2026-11-20. Repository baseline: `52001c9a650ff58801fd1b5c02b5269bbeca313b`.

This is a focused record of the FigCraft MCP tool schemas and read-only command discovery returned in the audit session. It describes this integration, not a version-independent product specification. The server did not supply a version identifier in these responses. No document inspection, modification, export, save, network collaboration, or installation was performed to obtain this evidence.

## Retrieval

- Discovered FigCraft tools through the host's tool catalog.
- Called `list_commands` with filters `doc.`, `file.export`, `file.import`, `session`, `library`, and `proto.interaction`.
- Commands quoted below were listed, not executed.

## R1 — Document targeting

The `document` parameter on inspection, mutation, screenshot, save and export tools is optional. Its schema says:

> The open file to act on: its `id` (from run_command doc.list, or the `document` open_file and inspect_document return) or its path. Default: the file in front.

`doc.list` adds a more restrictive agent-specific contract:

> `id` names the file in a control or MCP call's `document`; `path` only for the user's own scripts, not AI agents or plugins

It returns `{index, id, name, path?, dirty, active}` records. For agents, pin the returned **document ID**, rather than using a path or relying on whichever tab is in front. Layer IDs and document IDs serve distinct purposes.

`set_properties.ids` defaults to the selection. `create_node.parent` defaults to the current page. These defaults further motivate explicit targets.

## R2 — Export scope and output

The direct `export` tool describes the default scope as:

> the selection, else every top-level layer of the current page

Both the direct tool and `file.export` distinguish the `.fig` option:

> the whole file … experimental … with `warnings` for what was left out; other apps may not open every file

The `file.export` command also supports:

> penpot: the whole file as a Penpot 2.x archive, with warnings for unsupported content

`penpot` was **not** in the direct `export.format` enum returned in this session. Direct tool and engine command capabilities therefore differ; discovery is necessary.

Other relevant options:

- `contentsOnly` defaults to true; false also draws other layers in the exported area.
- `file.exportFramesPdf` exports every top-level frame of the requested/current page.
- `file.exportLayers` names files from layer names and suffixes; “slashes in a layer name make folders.” This establishes a naming behavior, not evidence of a path-traversal vulnerability.
- Direct export without a path returns image content, text, or base64 data depending on format. Omitting a filesystem destination does not mean no design content leaves the engine.

## R3 — Collaboration secrets, defaults and revocation

`collab.host` describes its returned `link` and `url` as:

> invites: they carry the secret token

It defaults to binding locally; `collab.share` with local sharing defaults to LAN binding instead.

`collab.share` defaults to `via: online` and `access: edit`. Its contract distinguishes the chosen code from returned links:

> online: your own link to the file, which edits: `share` has the link that goes with each code

The response can contain edit, view and comment codes and links. `collab.peers` can expose these alongside current session and published-file links. Consequently a view-sharing task must select the matching view link explicitly rather than echoing a generic `link` or the entire response.

`collab.stop` for an online shared file revokes shared links/codes and disconnects participants, but:

> they keep their copy

and the file stays on the server, accessible to its owner. Removal is a separate owner-authorized operation. `collab.leave` is also distinct from revocation.

`collab.files` explicitly says owner tokens and the owner's own links are not returned; do not generalize this protection to `collab.peers` or publishing responses.

## R4 — Libraries and network activity

- `library.publish` versions a local library; the file must be saved for other files to see it.
- `library.publishToServer` can keep the document on a server and returns edit/view links when it does so.
- `library.add` accepts a local `.figcraft` library or a server share link.
- `library.check` contacts team-library servers and subsequently checks for updates automatically. Its contract states that merely opening a file does not contact a team-library server the user has not added or checked.

These operations have different side effects despite all belonging to design-system work.

## R5 — Import and native save

- `open_file` supports native/JSON documents, `.fig`, `.sketch`, SVG and images; the documented imports are best effort where indicated and return warnings.
- `file.import` appends pages from `.figcraft`, `.penpot`, REST API file JSON, `.fig`, and `.sketch`, with variables/flows as supported. SVG becomes an editable frame and images become image layers.
- `save_file` uses `.figcraft` for native persistence. A `.fig` path produces an experimental export copy and leaves the native document's own path and unsaved changes alone.

These observations support explicit handling of import warnings and format-specific persistence semantics.

## R6 — Supported modeling/prototype breadth

- `set_properties` lists horizontal, vertical and grid layout; wrap; fill/hug/fixed sizing; constraints; gradients; effects; and text properties.
- `proto.interaction.add` lists navigation, back, scrolling, overlays, URL opening, component changes, variables, variable modes and conditional actions, plus multiple triggers, transitions and custom easing.
- The library registry includes publishing, linked components/styles/variables, updates, swapping, aliases and collection extension.

This confirms broad capability in the active integration. It does not certify Figma parity or establish runtime text-input/player behavior. No prototype was played during this audit.
