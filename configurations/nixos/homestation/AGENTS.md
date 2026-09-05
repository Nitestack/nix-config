# homestation

`homestation` is the NixOS server and the only host wiring the reusable
`modules/nixos/homelab/` API. App definitions live under `homelab/`; the
generated Arion projects, Caddy ingress, DNS rewrites, and Cloudflare Tunnel
configuration are outputs of that source.

## Service Changes

- Add or change an app in its focused file under `homelab/`, not in generated
  Compose state or a running container.
- When an app is added, removed, renamed, or its exposure changes, update
  `docs/homestation-services.md` in the same change.
- When the reusable module API or its validation/networking behaviour changes,
  read `modules/nixos/homelab/AGENTS.md` and update
  `docs/homelab-services.md`.
- Keep persistent data and recovery assumptions explicit. Relative bind mounts
  resolve under `/var/lib/homelab/<app>/`; absolute mounts and named volumes
  need service-specific care.

## Verification

From the repository root, run the generic check and exact host evaluation before
activating:

```sh
nix run .#check
nix eval .#nixosConfigurations.homestation.config.system.build.toplevel.drvPath --no-write-lock-file
```

After reviewing the result, activate on `homestation` with
`sudo nixos-rebuild switch --flake .#homestation`, then follow
`docs/homestation-operations.md` for systemd, container, ingress, and storage
checks. Cloudflare DNS and zone changes belong to
`opentofu/cloudflare/`, not to a manual edit on the server.
