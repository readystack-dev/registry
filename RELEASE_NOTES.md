# Distribution Registry v3.1.2 — ReadyStack release `v3.1.2-CE-ubuntu24.04-r9`

Generated 2026-11-17 from this release's build records and strategy. Statements that come from a single record name it in brackets; the others come from this release's strategy and build log.

## Release

- **Image:** `readystack/registry:v3.1.2-CE-ubuntu24.04-r9` (linux/amd64). Index digest `sha256:83323bc69c51013a8009350a424b083ac822101be28a35ecbfc1385ea214b515` [`docs/provenance/LOCKED_DIGEST.txt`].
- **Image manifest digest:** `sha256:dca8a7c5e1e86d2d12c48988f307f8525f13a2fe51545f201dcee799f756ca63` [`TARGET_DIGEST.txt`].
- **Upstream:** Distribution Registry v3.1.2 built from source at `v3.1.2` of https://github.com/distribution/distribution — upstream notes: https://github.com/distribution/distribution/releases/tag/v3.1.2 [`docs/provenance/BUILD_RECIPE.json`].
- **Base image:** none (`FROM scratch`): a chiselled Ubuntu 24.04 runtime, the files Canonical's chisel cut from Ubuntu's own packages (`vendored/chisel/slices.txt`, `vendored/chisel/packages.tsv`); runtime user `registry`.
- **Build:** `source-rebuild-go`, compiled offline with `--network=none`.
- **Supersedes:** ReadyStack release `v3.1.2-CE-ubuntu24.04-r8`.

## What changed in this release

- Upstream version unchanged (v3.1.2); ReadyStack rebuild.
- Minimum dependency versions ReadyStack enforces on top of upstream (raised only where upstream's own version was lower); shipped versions from the SBOM [`docs/sbom/SBOM.json`]:
  - `github.com/klauspost/compress` raised from upstream's `v1.18.4` to at least `v1.20.2` (shipped: `v1.20.2`)
  - `golang.org/x/net` raised from upstream's `v0.59.0` to at least `v0.62.0` (shipped: `v0.62.0`)
  - `github.com/go-jose/go-jose/v4` raised from upstream's `v4.1.4` to at least `v4.1.6` (shipped: `v4.1.6`)
- Distribution Registry v3.1.2 is unchanged from the release this supersedes (v3.1.2-CE-ubuntu24.04-r8); this release rebuilds it. Changes: the image is rebuilt on the current ubuntu:24.04 image (sha256:2198e4c65076…), which its maintainers updated after v3.1.2-CE-ubuntu24.04-r8 was built on sha256:fe81560990fa…. Settings, ports and data volumes are those of v3.1.2-CE-ubuntu24.04-r8: replace the container with this image on the same volumes.

## Security status at release (measured 2026-11-17)

- **Docker Scout:** 0 fixable critical/high vulnerabilities; runs as `registry` (non-root); SBOM + provenance attestations present (SPDX SBOM + SLSA v1) [`docs/provenance/SCOUT_VERDICT.json`].
- **OSV cross-check:** no fixable critical/high vulnerability beyond Docker Scout's findings.
- **Signature:** cosign signature verified by ReadyStack's release gate before publishing; check it yourself with `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/registry:v3.1.2-CE-ubuntu24.04-r9` (signatures are recorded in the public Sigstore transparency log).
- **ReadyStack release check at release:** passed (ReadyStack's own test, not a Docker Scout grade). Vulnerabilities are disclosed every day and the image never changes, so it is re-checked daily, beside Docker Scout's own dated grade, at https://readystack.dev/queue.
- **Before exposing this service:** The registry serves plain HTTP and uses basic authentication, which sends the password with every request: keep port 5000 on localhost or a private network until TLS is configured (REGISTRY_HTTP_TLS_CERTIFICATE / REGISTRY_HTTP_TLS_KEY, or a TLS-terminating proxy).
- **Before exposing this service:** The generated password for the user docker is printed once in the container log on the first start; if logs leave the host, replace /var/lib/registry/auth/htpasswd with your own bcrypt entries after the first login.

## Reproducibility

Rebuilds offline from this archive to the identical image digest `sha256:dca8a7c5e1e86d2d12c48988f307f8525f13a2fe51545f201dcee799f756ca63`: run `./BUILD_OFFLINE.sh`, then `./VERIFY_OFFLINE.sh` (add `--online` to confirm the Docker Hub tag holds the same digest). Before publishing, ReadyStack rebuilt the kit with `./BUILD_OFFLINE.sh` and compared the result with the published digest [`docs/provenance/REPRO_VERIFICATION.json`], then ran `./BUILD_OFFLINE.sh` and `./VERIFY_OFFLINE.sh` from this archive (without `--online`).

## Validation

- 3 of 3 use cases passed (4 authored, 3 kept in the suite; 1 not exercised, see below) against the running image [`docs/validation/test-results.json`].
- The cases are `GET /` (200), `GET /v2/` (200), `GET /v2/_catalog` (200); the suite checks nothing else.
- 1 further authored check was not exercised and is not counted above: it targeted port 5001, which the build's test container does not publish, so it never reached the product (listed under Not exercised in `docs/validation/FAILED_TESTS.md`).

## Known issues

- **low** (ReadyStack package): The comment at the top of the shipped config.yml does not mention that upload purging differs from upstream [`RSD-registry-db413d5fbf`] (carried from `v3.1.2-CE-ubuntu24.04-r1`, recorded there as `RSD-registry-d7c9d25f07`)

## Defects fixed

- 44 problems found before release and fixed before it shipped (44 by review); each is listed in `docs/defects/defects.json`.

## Verify this archive

```
sha256sum -c CHECKSUMS.sha256
./BUILD_OFFLINE.sh && ./VERIFY_OFFLINE.sh --online
cosign verify --key https://readystack.dev/keys/cosign.pub readystack/registry:v3.1.2-CE-ubuntu24.04-r9
```

ReadyStack records every signature in the public Sigstore transparency log (Rekor), so the command needs no extra flags.

