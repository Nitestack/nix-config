# Documentation

`docs/` contains maintained operator documentation. Keep commands copyable,
name the host and working directory they require, and never include secrets or
local credentials.

- `docs/agents/` contains workflow references for agents, not homelab service
  documentation.
- `docs/operations.md` is the generic activation and recovery runbook.
- `docs/homestation-operations.md` covers deployed service diagnosis.
- `docs/homelab-services.md` is the API reference for the reusable homelab
  module.
- `docs/homestation-services.md` is the current deployed service inventory.

Before adding a document, search for an existing source of truth and link from
the README when the document is broadly useful. When source code changes the
behaviour described by an operator document, update that document in the same
patch and remove stale commands rather than keeping historical alternatives.
