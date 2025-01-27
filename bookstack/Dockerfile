ARG BUILD_FROM=ghcr.io/hassio-addons/base:14.3.3
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base
ARG BOOKSTACK_VERSION="v23.10.4"
# hadolint ignore=DL3003
RUN \
    apk add --no-cache \
        mariadb-client=10.11.5-r0 \
        nginx=1.24.0-r7 \
        php81-curl=8.1.26-r0 \
        php81-dom=8.1.26-r0 \
        php81-fileinfo=8.1.26-r0 \
        php81-fpm=8.1.26-r0 \
        php81-gd=8.1.26-r0 \
        php81-iconv=8.1.26-r0 \
        php81-ldap=8.1.26-r0 \
        php81-mbstring=8.1.26-r0 \
        php81-mysqlnd=8.1.26-r0 \
        php81-openssl=8.1.26-r0 \
        php81-pdo_mysql=8.1.26-r0 \
        php81-session=8.1.26-r0 \
        php81-simplexml=8.1.26-r0 \
        php81-tokenizer=8.1.26-r0 \
        php81-xml=8.1.26-r0 \
        php81-xmlwriter=8.1.26-r0 \
        php81=8.1.26-r0 \
    \
    && apk add --no-cache --virtual .build-dependencies \
       composer=2.6.5-r0 \
    \
    && curl -J -L -o /tmp/bookstack.tar.gz \
        "https://github.com/BookStackApp/BookStack/archive/${BOOKSTACK_VERSION}.tar.gz" \
    && mkdir -p /var/www/bookstack \
    && tar zxf /tmp/bookstack.tar.gz -C \
        /var/www/bookstack --strip-components=1 \
    && cd /var/www/bookstack \
    \
    && composer install --no-dev \
    && apk del --no-cache --purge .build-dependencies \
    \
    && find /var/www/bookstack -type f -name "*.md" -depth -exec rm -f {} \; \
    && rm -f -r \
         /root/.composer \
         /tmp/* \
         /var/www/bookstack/dev \
         /var/www/bookstack/tests

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Paul Sinclair <hello@addons.community>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Community Add-ons" \
    org.opencontainers.image.authors="Paul Sinclair <hello@addons.community>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://addons.community" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
