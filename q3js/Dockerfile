ARG BUILD_FROM
FROM $BUILD_FROM

# ── System dependencies ───────────────────────────────────────────────────────
# No C compiler needed — quakejs runs entirely on Node.js
RUN apk add --no-cache \
    nodejs \
    npm \
    git \
    bash \
    curl \
    python3

# ── Clone quakejs (pre-built JS binaries already in build/) ──────────────────
RUN git clone --depth 1 https://github.com/unknown-gd/quakejs.git /opt/quakejs \
    && cd /opt/quakejs \
    && npm install --production --no-audit

# ── Placeholder for pak files (copied from /share at runtime) ─────────────────
RUN mkdir -p /opt/quakejs/base/baseq3

# ── rootfs overlay ────────────────────────────────────────────────────────────
COPY rootfs /

RUN chmod a+x \
    /etc/services.d/q3js/run \
    /etc/services.d/q3js/finish \
    /usr/bin/q3js-metrics

# 27960 game server, 27961 WSS proxy, 9000 content server, 8090 metrics API
EXPOSE 27960/tcp 27961/tcp 9000/tcp 8090/tcp
