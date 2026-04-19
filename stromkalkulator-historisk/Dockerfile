# https://developers.home-assistant.io/docs/apps/configuration#app-dockerfile
ARG BUILD_FROM=ghcr.io/home-assistant/base:3.23
FROM ${BUILD_FROM}

# Execute during the build of the image
ARG TEMPIO_VERSION=2021.09.0
ARG TARGETARCH
RUN \
    if [ -z "${TARGETARCH}" ]; then \
        echo "TARGETARCH is not set, please use Docker BuildKit for the build." && exit 1; \
    fi \
    && case "${TARGETARCH}" in \
        amd64) tempio_arch="amd64" ;; \
        arm64) tempio_arch="aarch64" ;; \
        *) echo "Unsupported TARGETARCH: ${TARGETARCH}" && exit 1 ;; \
    esac \
    && curl -sSLf -o /usr/bin/tempio \
        "https://github.com/home-assistant/tempio/releases/download/${TEMPIO_VERSION}/tempio_${tempio_arch}" \
    && apk add --no-cache \
        python3 \
        nginx \
    && mkdir -p /run/nginx


# Copy root filesystem
COPY rootfs /

#Copy our conf into the nginx http.d folder.
COPY ingress.conf /etc/nginx/http.d/

#Launch nginx with debug options.
CMD [ "nginx","-g","daemon off;error_log /dev/stdout debug;" ]

LABEL \
    org.opencontainers.image.title="Home Assistant App: Strømkalkulator Historisk" \
    org.opencontainers.image.description="Populate historical data for Strømkalkulator" \
    org.opencontainers.image.source="https://github.com/Cable89/stromkalkulator-historisk" \
    org.opencontainers.image.licenses="Apache License 2.0"
