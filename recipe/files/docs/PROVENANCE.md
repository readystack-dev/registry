# Provenance — Distribution Registry (`v3.1.2-CE-ubuntu24.04-r9`)

Every item below is copied from a record in this archive; the record is named in brackets.

## Source

- Upstream repository: https://github.com/distribution/distribution
- Revision built: `v3.1.2` (commit `3220848f15d9279c66a41aa0a257469e6aede1e9`)
- Source tree: `b7cd43949374362404d0782d67aca2c20279b89570b5d1401291180757d4d899` over 4599 files [`docs/provenance/SOURCE_TREE_HASH.json`]
- Community Edition only: enterprise code is removed or refused before the build (CE audit at acquire).

## Build

- Method: `source-rebuild-go`; base image `scratch + a chiselled Ubuntu 24.04 runtime (vendored/chisel/slices.txt, packages.tsv)`
- Compiled with `--network=none` from vendored source, dependencies and base images; BuildKit pinned to `moby/buildkit:v0.31.2` (shipped in `toolchain/`).
- SOURCE_DATE_EPOCH: `1794448328` [`SOURCE_DATE_EPOCH.txt`]
- Frozen inputs: [`docs/provenance/FREEZE_MANIFEST.json`], build recipe: [`docs/provenance/BUILD_RECIPE.json`].

## Output

- Published image: `readystack/registry:v3.1.2-CE-ubuntu24.04-r9` — index digest `sha256:83323bc69c51013a8009350a424b083ac822101be28a35ecbfc1385ea214b515` [`docs/provenance/LOCKED_DIGEST.txt`]
- linux/amd64 image digest `sha256:dca8a7c5e1e86d2d12c48988f307f8525f13a2fe51545f201dcee799f756ca63` — reproduced from this archive before publishing [`docs/provenance/REPRO_VERIFICATION.json`].

## Attestations and signature

- SBOM + SLSA provenance attached to the image: attached: an SPDX SBOM and SLSA v1 provenance [`docs/provenance/SCOUT_VERDICT.json`]
- cosign signature by the ReadyStack release key: verified before publishing. Check it: `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/registry:v3.1.2-CE-ubuntu24.04-r9`

## Pinned inputs, and why

- 29 pinned inputs, each with where it came from, why this one, how it was checked and when it was decided: `docs/PINS.md` [`docs/provenance/PINS.json`] (upstream source 2, base and build images 3, image digests 2, dependency versions readystack sets 7, pins in the kit's dockerfiles 8, readystack's build engine 5, dependencies upstream chose 2).

## Build trail

- How this release passed through the ReadyStack harness, record by record (every chain, drive, stage, review round, defect and check, each naming the record it is copied from): `ReadyStack-registry-v3.1.2-CE-ubuntu24.04-r9.trail.json`, published beside this archive and signed with the release key (`ReadyStack-registry-v3.1.2-CE-ubuntu24.04-r9.trail.json.sigstore`), and readable at https://readystack.dev/products/registry/trail/v3.1.2-CE-ubuntu24.04-r9.

