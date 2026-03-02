ARG BUILD_FROM
FROM $BUILD_FROM

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
    python3

WORKDIR /opt/q3js

# Clone q3js (server + WebSocket proxy only)
RUN git clone --depth 1 https://github.com/lklacar/q3js.git . \
    && git submodule update --init --recursive ioq3

# Build native dedicated server
RUN cd server && chmod +x build.sh && ./build.sh

# Install WebSocket proxy dependencies
RUN cd server/ws-udp-proxy && npm install --production --no-audit

# Placeholder for pak files
RUN mkdir -p /opt/q3js/server/build/Release/baseq3

# Copy rootfs overlay
COPY rootfs /

RUN chmod a+x \
    /etc/services.d/q3js/run \
    /etc/services.d/q3js/finish \
    /usr/bin/q3js-metrics

EXPOSE 27960/udp 27961/tcp 8090/tcp
