ARG BUILD_FROM=hassioaddons/debian-base:3.0.0
###############################################################################
# Get prebuild containers from Bitwarden RS
###############################################################################
ARG BITWARDEN_ARCH
# hadolint ignore=DL3006
FROM "bitwardenrs/server:1.13.1${BITWARDEN_ARCH}" as bitwarden

###############################################################################
# Build the actual add-on.
###############################################################################
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Get the Bitwarden from official images
COPY --from=bitwarden /bitwarden_rs /opt/bitwarden_rs
COPY --from=bitwarden /Rocket.toml /opt/Rocket.toml
COPY --from=bitwarden /web-vault /opt/web-vault

# add Nginx
# hadolint ignore=DL3009
RUN \
    apt-get update \
    \
    && apt-get install -y --no-install-recommends \
        nginx=1.14.2-2+deb10u1 \
    && apt-get clean \
    && rm -f -r \
        /etc/nginx \
    \
    && mkdir -p /var/log/nginx \
    && touch /var/log/nginx/error.log

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="Bitwarden RS" \
    io.hass.description="Open source password management solution." \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Franck Nijhof <frenck@addons.community>" \
    org.label-schema.description="Open source password management solution." \
    org.label-schema.build-date=${BUILD_DATE} \
    org.label-schema.name="Bitwarden RS" \
    org.label-schema.schema-version="1.0" \
    org.label-schema.url="https://community.home-assistant.io/t/community-hass-io-add-on-bitwarden-rs/115573?u=frenck" \
    org.label-schema.usage="https://github.com/hassio-addons/addon-bitwarden/tree/master/README.md" \
    org.label-schema.vcs-ref=${BUILD_REF} \
    org.label-schema.vcs-url="https://github.com/hassio-addons/addon-bitwarden" \
    org.label-schema.vendor="Home Assistant Community Add-ons"
