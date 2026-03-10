# ── Stage 1: install quakejs using node:12-alpine (matches original repo) ─────
FROM node:12-alpine AS quakejs-builder

RUN apk add --no-cache git bash jq

WORKDIR /opt/quakejs
RUN git clone --depth 1 https://github.com/unknown-gd/quakejs.git . \
    && npm install --legacy-peer-deps \
    && mkdir -p assets certificates

# ── Stage 2: HA addon base with quakejs copied in ────────────────────────────
ARG BUILD_FROM
FROM $BUILD_FROM

RUN apk add --no-cache \
    nodejs \
    npm \
    bash \
    jq \
    openssl \
    curl

COPY --from=quakejs-builder /opt/quakejs /opt/quakejs

COPY rootfs /

RUN chmod a+x \
    /etc/services.d/q3js/run \
    /etc/services.d/q3js/finish \
    /usr/bin/q3js-metrics

# WSS proxy | content server | web client | metrics API
EXPOSE 27961/tcp 9000/tcp 8080/tcp 8090/tcp
