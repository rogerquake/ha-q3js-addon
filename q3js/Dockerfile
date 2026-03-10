ARG BUILD_FROM
FROM $BUILD_FROM

# ── Dependencies ──────────────────────────────────────────────────────────────
RUN apk add --no-cache \
    nodejs \
    npm \
    git \
    bash \
    jq \
    openssl \
    curl

# ── Clone quakejs once — shared by all services ───────────────────────────────
WORKDIR /opt/quakejs
RUN git clone --depth 1 https://github.com/unknown-gd/quakejs.git . \
    && npm install

# ── Asset directory (pak files copied from /share at runtime) ─────────────────
RUN mkdir -p /opt/quakejs/assets \
    && mkdir -p /opt/quakejs/certificates

# ── rootfs overlay (s6 scripts + metrics server) ──────────────────────────────
COPY rootfs /

RUN chmod a+x \
    /etc/services.d/q3js/run \
    /etc/services.d/q3js/finish \
    /usr/bin/q3js-metrics

# game(internal) | WSS proxy | content | web client | metrics API
EXPOSE 27961/tcp 9000/tcp 8080/tcp 8090/tcp
