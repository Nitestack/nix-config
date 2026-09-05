# Reusable Modules

Modules provide behaviour shared by host configurations. Keep machine choices
in `configurations/*/<host>/` and keep each module focused on one purpose.

- `modules/shared/` is cross-platform system behaviour.
- `modules/nixos/`, `modules/darwin/`, and `modules/home/` are platform-specific
  behaviour.
- `modules/flake-parts/toplevel.nix` owns the flake formatter, check app, dev
  shell, and evaluation-time checks. Read it before changing flake outputs or
  repository tooling.
- `flake.nix` uses `nixos-unified.lib.mkFlake`; do not replace it with generic
  `flake-utils` or manual `eachDefaultSystem` wiring.
- Preserve the existing module names and explicit imports; the flake exposes
  them through `nixos-unified` auto-wiring.

Before changing a shared module, identify every host or profile that imports it
and evaluate the affected targets. Use platform guards instead of leaking
NixOS-only or Darwin-only options across configurations. Read the deeper
homelab and OpenCode guides for those module families.
