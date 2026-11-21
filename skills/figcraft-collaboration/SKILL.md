---
name: figcraft-collaboration
description: Use when sharing a FigCraft document, inviting collaborators, publishing to a server, or adding and checking team libraries, so destination, access level, and credentials are resolved and revocation is distinguished from deletion.
---

# FigCraft collaboration and publishing

Load the shared `figcraft` rules first; its Security and consent section governs
when sharing or publishing is allowed and how to treat remote content. This skill
adds the workflow for doing an authorized share safely.

Discover the current `collab` and `library` command contracts with your host's
tool discovery and the `list_commands` filters. Option names, defaults, and
returned fields vary by release; do not assume them from older documentation.

## 1. Resolve the request

Before any share or publish call, establish all three:

- **Destination:** local or LAN sharing versus online. They have different
  exposure: online uploads the document to a service, while local/LAN keeps it on
  the machine or network.
- **Access level:** view, comment, or edit. Confirm which one the user asked for
  and do not upgrade it.
- **Audience:** who receives the link or code, so the matching one can be
  returned.

`collab.share` defaults to an online destination and edit access. Do not let an
underspecified request inherit those defaults; if the user did not say, ask for
the missing choice instead of guessing. Query the current `collab.share` and
`collab.host` schemas to confirm how destination and access are passed in this
session.

## 2. Share and return the matching link

Call the share command with the resolved destination and access level, and the
recorded document target where the schema accepts it.

- Return only the link or code that matches the requested audience and access
  level. A view-sharing request returns the view link, not the generic owner
  link.
- Do not echo the entire response or include unrelated codes and links; the
  response can contain edit, view, and comment entries plus session and
  published-file links.
- State plainly what was shared, to whom, and at which access level.

## 3. Protect credentials

Invite links, share codes, and tokens are bearer secrets: anyone holding one can
use it.

- Keep them out of committed files, diagnostic dumps, logs, and public comments.
- Return an invite only as needed for the authorized handoff, and prefer a
  channel the user controls.
- `collab.files` hides owner tokens and the owner's own links, but
  `collab.peers` and publishing responses can expose them. Do not generalize
  that protection to commands where the schema does not promise it; inspect what
  a response actually contains before echoing any part of it.

## 4. Revoke, leave, or delete

Keep these operations distinct and report them accurately:

- **Revoke a share** (`collab.stop` for an online share) revokes shared links and
  codes and disconnects participants. Recipients keep the copies they already
  have, and the file stays on the server, still accessible to its owner.
- **Leave a session** (`collab.leave`) removes the current participant from a
  live session; it is not the same as revoking a share.
- **Delete server data** is a separate, owner-authorized operation.

Never describe revocation as deletion. Say what actually changed: links revoked,
participants disconnected, and any copy that remains.

When live collaboration is relevant, check existing session state before editing,
because edits may already synchronize to other participants.

## 5. Publish libraries

Library publishing is a collaboration variant with its own destination and
network side effects. Apply the destination, access, and credential rules above.

- `library.publish` versions a local library; save the file first so other files
  can see it.
- `library.publishToServer` can keep the document on a server and returns edit
  and view links when it does. Resolve destination and access first, and treat
  the returned links as credentials.
- `library.add` accepts a local library or a server share link.
- `library.check` contacts team-library servers and then checks for updates
  automatically. Opening a file alone does not contact a team-library server the
  user has not added or checked.

Distinguish local publishing from server publishing, and say which one was
performed and what it exposed.

## Report

Summarize the destination, access level, and audience; what was shared or
published; the verification actually performed; and any credential you returned
and to whom. State revocation, leaving, and deletion precisely, and do not claim
a share, publish, or revocation that the tool did not confirm.
