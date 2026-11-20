ARG BUILD_FROM
FROM $BUILD_FROM

ARG BUILD_ARCH
ARG XUI_VERSION="v3.11.0"

# Home Assistant arch -> 3x-ui release arch
RUN set -eux; \
    case "$BUILD_ARCH" in \
        amd64)   XUI_ARCH=amd64 ;; \
        aarch64) XUI_ARCH=arm64 ;; \
        armv7)   XUI_ARCH=armv7 ;; \
        armhf)   XUI_ARCH=armv5 ;; \
        i386)    XUI_ARCH=386 ;; \
        *) echo "Unsupported arch: $BUILD_ARCH" >&2; exit 1 ;; \
    esac; \
    apk add --no-cache ca-certificates curl tar tzdata sqlite bash; \
    curl -fsSL "https://github.com/MHSanaei/3x-ui/releases/download/${XUI_VERSION}/x-ui-linux-${XUI_ARCH}.tar.gz" \
        -o /tmp/x-ui.tar.gz; \
    mkdir -p /app; \
    tar -xzf /tmp/x-ui.tar.gz -C /app --strip-components=1; \
    rm -f /tmp/x-ui.tar.gz; \
    chmod +x /app/x-ui /app/bin/xray-linux-* 2>/dev/null || true; \
    mkdir -p /data /etc/x-ui; \
    ln -sfn /data /etc/x-ui

ENV XUI_BIN_FOLDER=/app/bin \
    XUI_LOG_FOLDER=/app/log

COPY rootfs /