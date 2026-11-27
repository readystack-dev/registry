[![ReadyStack](https://readystack.dev/logo.svg)](https://readystack.dev)

**Stable, Open-source Releases, Automated** · [readystack.dev](https://readystack.dev)

[![Docker Hub](https://img.shields.io/badge/Docker%20Hub-readystack%2Fregistry-2496ED?logo=docker&logoColor=white)](https://hub.docker.com/r/readystack/registry) [![Docker pulls](https://img.shields.io/docker/pulls/readystack/registry)](https://hub.docker.com/r/readystack/registry) [![Grade at release](https://img.shields.io/badge/grade%20at%20release-A%20%2894%25%29-2ea44f)](https://readystack.dev/products/registry) [![Signed: cosign](https://img.shields.io/badge/signed-cosign-2ea44f)](https://readystack.dev/keys/cosign.pub) ![SBOM: SPDX](https://img.shields.io/badge/SBOM-SPDX-2ea44f) ![Provenance: SLSA v1](https://img.shields.io/badge/provenance-SLSA%20v1-2ea44f) ![Offline rebuild: verified](https://img.shields.io/badge/offline%20rebuild-verified-2ea44f)

# Distribution Registry: ReadyStack build kit

The offline build kit for the Docker image `readystack/registry:v3.1.2-CE-ubuntu24.04-r9`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v3.1.2-CE-ubuntu24.04-r9](https://github.com/readystack-dev/registry/releases/tag/v3.1.2-CE-ubuntu24.04-r9)
- Image: [`docker.io/readystack/registry:v3.1.2-CE-ubuntu24.04-r9`](https://hub.docker.com/r/readystack/registry/tags), manifest digest `sha256:dca8a7c5e1e86d2d12c48988f307f8525f13a2fe51545f201dcee799f756ca63`
- Product page: https://readystack.dev/products/registry
- Source and build recipe: https://readystack.dev/products/registry/recipe
- Build trail: https://readystack.dev/products/registry/trail/v3.1.2-CE-ubuntu24.04-r9
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=registry

## The image

Single container, single static Go binary (`registry serve /etc/distribution/config.yml`) in the foreground. Stores image layers and manifests on the local filesystem under /var/lib/registry; no database, no web UI, no sidecar.

## Who uses Distribution Registry and why

Teams who want to store and distribute their container images on infrastructure they control.

Distribution Registry is the open-source server that stores container images and other content and serves them using the OCI Distribution Specification.

A typical use: A company's build system produces container images that must stay inside its network. It runs Distribution Registry internally: the build pipeline pushes each image there, and every server pulls from it, so image storage and distribution are fully under the company's control.

From the upstream project's own description: https://github.com/distribution/distribution, https://distribution.github.io/distribution/.

## This release

- Docker Scout policy grade at release: **A** (94%), graded 2026-11-17 with `docker scout policy` (CLI 1.26.0, docker-default configuration).
  - Docker Scout counts 12 packages with copyleft licences (GPL, LGPL, MPL), all of them operating-system packages (it counts only packages whose licence it can read; the SBOM lists every component).
- Signed with ReadyStack's release key (fingerprint `04daa92344a52dbc`): https://readystack.dev/keys/cosign.pub.
- Runs as a non-root user (`registry`).
- Attestations on the image: an SPDX SBOM and SLSA provenance v1.
- Rebuilt from this kit with network egress blocked on 2026-11-17; the rebuilt image digest is the published one.
- Base: Ubuntu 24.04.

The current grade, measured again every day, is on https://readystack.dev/products/registry.

Distribution Registry is distributed under its upstream licence (Apache-2.0); the kit contains upstream's source under that licence, and each vendored component under its own.

## Download

From the [v3.1.2-CE-ubuntu24.04-r9 release](https://github.com/readystack-dev/registry/releases/tag/v3.1.2-CE-ubuntu24.04-r9): `ReadyStack-registry-v3.1.2-CE-ubuntu24.04-r9.tar.gz` (457.1 MB), SHA-256 `0754491befcdd6c27cf4d652c90fe9e1b9469bb2fc651395b0b5325e39f8770e`.

```sh
sha256sum --check --ignore-missing SHA256SUMS
```

## Verify and rebuild

The rebuild statement is signed with the ReadyStack release key. Check the signature before you rely on it:

```sh
cosign verify-blob --key https://readystack.dev/keys/cosign.pub \
  --bundle ReadyStack-registry-v3.1.2-CE-ubuntu24.04-r9.rebuild.json.sigstore ReadyStack-registry-v3.1.2-CE-ubuntu24.04-r9.rebuild.json
```

Unpack with `-p` so file modes are kept, then follow the kit's own README.md and docs/REBUILD.md:

```sh
tar -xpzf ReadyStack-registry-v3.1.2-CE-ubuntu24.04-r9.tar.gz
cd v3.1.2-CE-ubuntu24.04-r9
less README.md docs/REBUILD.md
```

The build trail (`ReadyStack-registry-v3.1.2-CE-ubuntu24.04-r9.trail.json`, signed the same way, and its Markdown copy) records how this release was built and checked.

## This repository

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/registry/recipe. The complete kit is the release download. Each release is a tag here. How to report a vulnerability: [SECURITY.md](SECURITY.md).

## About ReadyStack

**Stable, Open-source Releases, Automated.** ReadyStack releases popular open-source apps every week with every available critical and high security fix applied, plus full documentation, reference data, tests and a build recipe that recreates each release bit-for-bit. Each release is rebuilt from its pinned inputs with the network off, checked against its published digest, signed, and scanned and graded again every day. The images and build kits are free, and ReadyStack is soon to be a nonprofit.

[readystack.dev](https://readystack.dev) · [All products](https://readystack.dev/products.html) · [Build kits on GitHub](https://github.com/readystack-dev) · [Defect log](https://readystack.dev/defects.html) · contact@readystack.dev

— ReadyStack · https://readystack.dev
