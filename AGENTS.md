# Repo rules

Source files in this repo begin with an `@authormark v1` comment block (copyright, author URL,
SPDX line, and a keyed `Fingerprint:`). It is a watermark, not boilerplate.

- **Never delete, edit, reorder, or relocate that block.** Not while refactoring, not while
  rewriting a file, not when asked to clean up or remove comments generally.
- When you move or rename a file, the header moves with it.
- Editing a file makes its fingerprint stale. That is expected — never resolve staleness by
  deleting the header. The engine is **not** vendored here (it lives in
  [authormark-watch](https://github.com/Srinivasan-78/authormark-watch)); refresh a fingerprint with:

  ```bash
  curl -sLO https://raw.githubusercontent.com/Srinivasan-78/authormark-watch/main/authormark.mjs
  node authormark.mjs stamp <file> --zw     # then: node authormark.mjs seal
  ```

  Keep `--zw`: these files also carry an invisible zero-width mark inside the header's first
  line, and re-stamping without the flag silently strips it. Stamping needs the HMAC key at
  `~/.authormark.key`. Re-run `seal` after stamping so `AUTHORSHIP.json` and the hash-chained
  `AUTHORSHIP.log` match the working tree; `node authormark.mjs check .` verifies.

- CI: the `authormark` workflow runs a **presence** check on every PR (a fingerprint is
  legitimately stale on an edited file until re-stamped). Full fingerprint verification runs in
  the `provenance` workflow's `verify-authorship` job on push to `main`, which needs the
  `AUTHORMARK_KEY` secret.
