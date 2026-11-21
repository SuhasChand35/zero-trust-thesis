# Pinned versions

Record image digests, not just tags. Recheck JWT settings (require_expiration, clock_skew_seconds) after any version change. Pins below are **candidates** looked up on 21 Nov 2026; they become final at the protocol freeze (Gate 3). Confirm each digest on the lab host with `docker buildx imagetools inspect <image>:<tag>` before the first build, because the values were read from registry web APIs, not pulled.

| Component | Image / tag | Index digest | amd64 digest | Looked up | Notes |
|---|---|---|---|---|---|
| Envoy | `envoyproxy/envoy:v1.39.3` | `sha256:dd85940439de19a0b6ae8419610363ea0ad351d9a994ea007161c206ec1e1865` | `sha256:e877a955cc8fde1d54b6b4141b95240511fe1494a7d98759c71a0cf43573ae33` | 2026-11-21 (Docker Hub, tag updated 2026-10-06) | version from the proposal |
| OPA with Envoy plugin | `openpolicyagent/opa:1.22.0-envoy` | `sha256:d4357f863f005c31ff5dee7ceafa0b95ef2d22f2ee34f140c5d13cd2f076cbb0` | `sha256:2a8bd899fe8dd034a0e812fbc7d70e8e0a0da0d9e7ceab0299e10f7e5183e52e` | 2026-11-21 (Docker Hub, tag updated 2026-10-29) | latest opa-envoy-plugin release (v1.22.0-envoy) at lookup |
| Keycloak | `quay.io/keycloak/keycloak:26.8.2` | [pin on host] | [pin on host] | 2026-11-21 (GitHub releases: 26.8.2 marked Latest, 5 Nov 2026) | quay.io digest could not be read remotely |
| Host OS / kernel | [version] | n/a | n/a | [date] | |
| Docker Engine / Compose | [version] | n/a | n/a | [date] | |
| Python | [version] | n/a | n/a | [date] | request scripts |
