ARG BUILD_FROM
FROM $BUILD_FROM

RUN apk add --no-cache \
    bash \
    curl \
    git \
    docker-cli \
    openssl \
    nodejs

# Install docker compose v2 plugin
RUN mkdir -p /usr/lib/docker/cli-plugins \
    && ARCH=$(uname -m) \
    && if [ "$ARCH" = "x86_64" ]; then ARCH="x86_64"; elif [ "$ARCH" = "aarch64" ]; then ARCH="aarch64"; fi \
    && curl -SL "https://github.com/docker/compose/releases/download/v2.24.5/docker-compose-linux-${ARCH}" \
       -o /usr/lib/docker/cli-plugins/docker-compose \
    && chmod +x /usr/lib/docker/cli-plugins/docker-compose

COPY rootfs /

RUN chmod a+x \
    /etc/services.d/q3js/run \
    /etc/services.d/q3js/finish \
    /usr/bin/q3js-metrics

EXPOSE 27961/tcp 9000/tcp 443/tcp 8090/tcp
