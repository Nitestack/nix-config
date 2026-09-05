# Homelab Module

This directory is the reusable API and renderer for self-hosted services. Its
public contract is documented in `docs/homelab-services.md`.

## Source Roles

- `options.nix` defines the public option schema.
- `normalize.nix` and `lib.nix` derive enabled apps, effective hosts, project
  names, container names, paths, and helper URLs.
- `validation.nix` contains evaluation-time assertions.
- `state.nix` creates persistent directories and permissions with tmpfiles.
- `arion.nix`, `networking.nix`, and `caddy.nix` render container and ingress
  behaviour; `cloudflared.nix` and `adguard-home.nix` integrate native services.

When changing the API, an assertion, or networking/Caddy/DNS behaviour:

1. Update the matching option, validation, and recipe sections in
   `docs/homelab-services.md`.
2. Add or adjust an evaluation regression in `checks/` when the behaviour has a
   stable invariant worth protecting.
3. Run `git diff --check`, `nix run .#check`, and evaluate the exact
   `homestation` target.

Keep app-specific choices in `configurations/nixos/homestation/homelab/`.
Relative bind sources must remain inside the configured app data directory;
absolute paths and named volumes represent existing state and must not be
silently moved. Pass credentials through sops paths, templates, or
`environmentFiles`, never literal Nix strings.

## Security-Sensitive Invariants

- `expose.mode = "private"` is local-only. Its local HTTPS route may proxy, but
  the Cloudflare Tunnel origin listener must return `403`.
- Public apps require Cloudflare Tunnel configuration and currently use the
  dedicated loopback HTTP listener at `127.0.0.1:<caddy.tunnelPort>`. Do not
  restore the historical HTTPS origin or pinned-SNI approach without a new
  design and regression coverage.
- Fixed `containerName` values, raw `targetUpstream` sidecars, device and socket
  mounts, image pins, and absolute host paths are often deliberate integration
  exceptions. Inspect their consumer and upstream contract before normalizing
  or removing them.
