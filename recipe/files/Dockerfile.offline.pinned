# GENERATED L-009 hermetic build (--network=none, SBOM+SLSA-attestation-producing).
# Pinned bases from BASE_IMAGE_DIGESTS.lock; vendored Go tree + debs from Dockerfile.vendor.
FROM golang:1.26.11-bookworm@sha256:dc40a13b61304647ed7e5d581d246b86413dfc29d63c6d018310feb93cd98fc0 AS build_registry
WORKDIR /src
COPY vendored/registry-src/ .
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOWORK=off GOFLAGS="-mod=vendor -buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1794448328
RUN go build -trimpath -ldflags='-s -w -X github.com/distribution/distribution/v3/version.version=3.1.2 -X github.com/distribution/distribution/v3/version.revision=3220848f15d9279c66a41aa0a257469e6aede1e9 -X github.com/distribution/distribution/v3/version.mainpkg=github.com/distribution/distribution/v3' -o /registry ./cmd/registry && /registry --version > /tmp/registry.version 2>&1; cat /tmp/registry.version; grep -qF 'v3 3.1.2' /tmp/registry.version || { echo 'registry --version does not report v3 3.1.2'; exit 1; } && \
    { readelf -lW /registry | sed -n 's/.*Requesting program interpreter: \(.*\)]$/\1/p'; readelf -dW /registry | sed -n 's/.*Shared library: \[\(.*\)\]$/\1/p'; } > /registry.needs
# rs-probe: the HEALTHCHECK helper (standard library only, from the kit's rs-probe.go)
FROM golang:1.26.11-bookworm@sha256:dc40a13b61304647ed7e5d581d246b86413dfc29d63c6d018310feb93cd98fc0 AS build_rs_probe
WORKDIR /probe
COPY rs-probe.go main.go
COPY rs-probe.mod go.mod
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOFLAGS="-buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1794448328
RUN go build -trimpath -ldflags='-s -w' -o /rs-probe .
# the chiselled rootfs (vendored/chisel, cut by Dockerfile.vendor), the service user, the binaries
FROM ubuntu:24.04@sha256:2198e4c65076e9cb0b3d695219df667c0e3b0072da64ca3c2648aca17dcecaf5 AS rootfs
ARG SOURCE_DATE_EPOCH=1794448328
ADD vendored/chisel/rootfs.tar /rootfs/
RUN printf 'registry:x:999:999::/var/lib/registry:/bin/sh\n' >> /rootfs/etc/passwd && printf 'registry:x:999:\n' >> /rootfs/etc/group && \
    mkdir -p /rootfs/var/lib/registry /rootfs/etc/distribution && chown -R 999:999 /rootfs/var/lib/registry /rootfs/etc/distribution
COPY --from=build_registry /registry /rootfs/usr/bin/registry
COPY --from=build_registry /registry.needs /tmp/needs/registry
COPY --from=build_rs_probe /rs-probe /rootfs/usr/local/bin/rs-probe
COPY config.yml /rootfs/etc/distribution/config.yml
RUN chmod 755 /rootfs/usr/bin/registry /rootfs/usr/local/bin/rs-probe && \
    chown 999:999 /rootfs/etc/distribution/config.yml && \
    chmod 0644 /rootfs/etc/distribution/config.yml
RUN --mount=type=bind,source=vendored/chisel,target=/chisel set -eu; \
    for n in /tmp/needs/*; do while read -r lib; do [ -n "$lib" ] || continue; \
      case "$lib" in /*) [ -e "/rootfs$lib" ] ;; *) [ -n "$(find /rootfs/usr/lib /rootfs/usr/lib64 -name "$lib" 2>/dev/null | head -1)" ] ;; esac || \
      { echo "$(basename "$n") needs $lib, which the chiselled rootfs does not have: add its slice to runtime_base.slices" >&2; exit 1; }; \
    done < "$n"; done; rm -rf /tmp/needs; \
    [ -e /rootfs/etc/os-release ] || { echo "no /etc/os-release: base-files_release-info is missing" >&2; exit 1; }; \
    [ "$(grep -c '^Package: ' /rootfs/var/lib/dpkg/status)" = "$(wc -l < /chisel/packages.tsv)" ] || \
      { echo "/var/lib/dpkg/status does not list every package of vendored/chisel/packages.tsv" >&2; exit 1; }; \
    rm -rf /rootfs/var/log/dpkg.log /rootfs/var/log/alternatives.log /rootfs/var/log/apt /rootfs/var/cache/ldconfig/aux-cache /rootfs/var/cache/fontconfig
FROM scratch
ARG SOURCE_DATE_EPOCH=1794448328
COPY --from=rootfs /rootfs/ /
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin OTEL_TRACES_EXPORTER=none
LABEL org.opencontainers.image.licenses="Apache-2.0"
LABEL org.opencontainers.image.source="https://github.com/distribution/distribution"
LABEL org.opencontainers.image.title="Distribution Registry"
LABEL org.opencontainers.image.description="Distribution is the open-source registry server behind the Docker Registry API: push, store and pull your own container images and OCI artifacts on your own infrastructure. This image requires a login from the first start, with a random password generated per installation instead of a shared default."
LABEL org.opencontainers.image.version="v3.1.2-CE-ubuntu24.04-r9"
LABEL org.opencontainers.image.revision="3220848f15d9279c66a41aa0a257469e6aede1e9"
HEALTHCHECK --interval=5s --timeout=30s --start-period=10s \
  CMD /usr/local/bin/rs-probe http://127.0.0.1:5000/ || exit 1
USER registry
EXPOSE 5000
ENTRYPOINT ["registry"]
CMD ["serve", "/etc/distribution/config.yml"]
