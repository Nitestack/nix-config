# Encrypted Secrets

Files in this directory are sops ciphertext. Read `docs/secrets.md` before
editing them.

- Edit existing values with `sops`, never by replacing an encrypted file with
  plaintext.
- Keep each file under the path scope intended by `.sops.yaml`; add recipients
  only for machines that genuinely need the secret.
- When adding or renaming a key, update the consuming host's `sops.nix` mapping
  and templates in the same change.
- When a shared secret gains another consumer, declare the mapping on every
  importing host and evaluate all of them; a recipient in `.sops.yaml` alone
  does not wire the secret into a system.
- Values containing `$`, password hashes, and similar structured credentials
  belong in file-mounted sops templates or `environmentFiles`, not generic
  Compose environment maps where interpolation can corrupt them.
- Validate and activate without printing decrypted values. Do not use
  `sops -d` in command substitution, redirect decrypted output to a persistent
  file, or put it in logs or issue comments.

Secret changes require the owning host evaluation and a consumer-level health
check. A successful Nix evaluation does not prove that a service can decrypt or
use a changed credential.
