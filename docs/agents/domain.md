# Domain Sources

This repository currently has no root `CONTEXT.md`, `CONTEXT-MAP.md`, or
`docs/adr/` directory. Do not invent one merely because a generic workflow
mentions it. Establish a glossary or ADR only when a domain decision actually
needs one.

## Before editing

Trace the concept to its source of truth:

- Host behaviour and wiring: `configurations/` and the nearest host guide.
- Reusable homelab API: `modules/nixos/homelab/` and
  `docs/homelab-services.md`.
- Deployed homestation services and exposure:
  `docs/homestation-services.md` and `docs/homestation-operations.md`.
- Activation, recovery, and secret handling: `docs/operations.md` and
  `docs/secrets.md`.

Use the names already present in the Nix options and operator docs, especially
the distinction between an app, its containerized services, local ingress, and
public Cloudflare Tunnel ingress. If a proposed change contradicts one of
these sources, surface the conflict before editing instead of silently creating
a second vocabulary.
