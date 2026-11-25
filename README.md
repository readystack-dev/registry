# Distribution Registry: ReadyStack build kit

The offline build kit for the Docker image `readystack/registry:v3.1.2-CE-ubuntu24.04-r9`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v3.1.2-CE-ubuntu24.04-r9](https://github.com/readystack-dev/registry/releases/tag/v3.1.2-CE-ubuntu24.04-r9)
- Image: [`docker.io/readystack/registry:v3.1.2-CE-ubuntu24.04-r9`](https://hub.docker.com/r/readystack/registry/tags), manifest digest `sha256:dca8a7c5e1e86d2d12c48988f307f8525f13a2fe51545f201dcee799f756ca63`
- Product page: https://readystack.dev/products/registry
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=registry

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

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/registry/recipe. The complete kit is the release download. Each release is a tag here.
