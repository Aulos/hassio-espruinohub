ARG BUILD_FROM=hassioaddons/base:7.1.0
FROM ${BUILD_FROM}

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ENV LANG C.UTF-8

USER root

ARG BUILD_ARCH

RUN \
    apk add --no-cache \
        nodejs>=12.15.0 \
        npm>=12.15.01 \
    && apk add --no-cache --virtual .build-dependencies \
        python>=2.7.16 \
        build-base>=0.5 \
        bluez>=5.52 \
        linux-headers>=4.19.36 \
        eudev-dev>=3.2.9 \
        git>=2.24.0 \
    \
    && npm set unsafe-perm true \
    && NODE_PATH="/usr/lib/node_modules" \
    && npm -g i "git+https://github.com/espruino/EspruinoHub.git" \
    \
    && apk del --no-cache --purge .build-dependencies

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

WORKDIR /var/espruinohub

COPY run.sh run.sh
RUN chmod +x run.sh

CMD ./run.sh

# Labels
LABEL \
    io.hass.name="EspruinoHub" \
    io.hass.description="BLE MQTT bridge" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Matias Käkelä" \
    org.label-schema.description="BLE MQTT bridge" \
    org.label-schema.build-date=${BUILD_DATE} \
    org.label-schema.name="EspruinoHub" \
    org.label-schema.schema-version="0.4.0" \
    org.label-schema.usage="https://github.com/massive/hassio-espruinohub/blob/master/README.md" \
    org.label-schema.vcs-ref=${BUILD_REF} \
    org.label-schema.vcs-url="https://github.com/massive/hassio-espruinohub.git" \
    org.label-schema.vendor="Community Hass.io Addons"
