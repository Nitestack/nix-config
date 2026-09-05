# NixOS Hosts

The NixOS targets are:

| Host | Role |
| --- | --- |
| `nixstation` | Bare-metal interactive desktop |
| `homestation` | Bare-metal homelab server |
| `wslstation` | NixOS under WSL with Docker Desktop integration |

Host files are the right place for machine-specific imports, device and
filesystem choices, networking, and service selection. Put behaviour shared by
multiple hosts in `modules/nixos/` or `modules/shared/`, with platform guards
where necessary.

Evaluate a changed host with:

```sh
nix eval .#nixosConfigurations.<host>.config.system.build.toplevel.drvPath --no-write-lock-file
```

Use `sudo nixos-rebuild switch --flake .#<host>` for an ordinary change. Use
`boot` only for first-install flows or when the next boot must select the new
generation. The WSL host has a noninteractive verification loop documented in
`wslstation/AGENTS.md`; homelab changes have additional service and inventory
rules in `homestation/AGENTS.md`.
