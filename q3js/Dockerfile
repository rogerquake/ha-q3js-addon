ARG BUILD_FROM
FROM $BUILD_FROM

# ── System dependencies ───────────────────────────────────────────────────────
RUN apk add --no-cache \
    nodejs \
    npm \
    cmake \
    make \
    gcc \
    g++ \
    git \
    bash \
    curl \
    python3 \
    sdl2-dev \
    libc-dev \
    linux-headers

# ── Build ioq3ded from ioquake3 directly ─────────────────────────────────────
# Clone ioq3 directly (no submodule needed)
RUN git clone --depth 1 https://github.com/ioquake/ioq3.git /opt/ioq3

WORKDIR /opt/ioq3/build

RUN cmake .. \
    -DBUILD_SERVER=ON \
    -DBUILD_CLIENT=OFF \
    -DBUILD_RENDERER_OPENGL1=OFF \
    -DBUILD_RENDERER_OPENGL2=OFF \
    -DBUILD_GAME_LIBRARIES=ON \
    -DBUILD_GAME_QVMS=OFF \
    -DUSE_OPENAL=OFF \
    -DUSE_CODEC_VORBIS=OFF \
    -DUSE_CODEC_OPUS=OFF \
    -DUSE_MUMBLE=OFF \
    -DUSE_VOIP=OFF \
    -DUSE_FREETYPE=OFF \
    -DUSE_CURL=OFF \
    -DCMAKE_BUILD_TYPE=Release \
    && make -j$(nproc) ioq3ded

RUN mkdir -p /opt/q3js/server/build/Release/baseq3 \
    && cp /opt/ioq3/build/ioq3ded /opt/q3js/server/build/Release/ioq3ded

# ── WebSocket proxy ───────────────────────────────────────────────────────────
RUN git clone --depth 1 https://github.com/lklacar/q3js.git /opt/q3js-src

RUN cp -r /opt/q3js-src/proxy /opt/q3js/server/ws-udp-proxy \
    && cd /opt/q3js/server/ws-udp-proxy \
    && npm install --production --no-audit

# ── rootfs overlay ────────────────────────────────────────────────────────────
COPY rootfs /

RUN chmod a+x \
    /etc/services.d/q3js/run \
    /etc/services.d/q3js/finish \
    /usr/bin/q3js-metrics

EXPOSE 27960/udp 27961/tcp 8090/tcp
