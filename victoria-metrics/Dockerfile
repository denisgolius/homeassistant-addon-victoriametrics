ARG BUILD_FROM=ghcr.io/home-assistant/base:latest

ARG BUILD_VERSION=v1.139.0

FROM $BUILD_FROM

# LABEL \
#     io.hass.name="VictoriaMetrics" \
#     io.hass.description="VictoriaMetrics Add-on for Home Assistant OS" \
#     io.hass.arch="${BUILD_ARCH}" \
#     io.hass.type="addon" \
#     io.hass.BUILD_VERSION="${BUILD_VERSION}" \
#     io.hass.url="https://github.com/VictoriaMetrics-Community/homeassistant-addon-victoriametrics" \
#     maintainer="VictoriaMetrics-Community"

# Copy data for add-on
COPY start-victoria-metrics /
COPY prometheus.tpl /
WORKDIR /share/victoria-metrics-data

RUN /bin/bash -c 'set -ex && \
    ARCH=$(uname -m) && \
    if [ "$ARCH" == "x86_64" ]; then \
    wget https://github.com/VictoriaMetrics/VictoriaMetrics/releases/download/$BUILD_VERSION/victoria-metrics-linux-amd64-$BUILD_VERSION.tar.gz -qO- | tar xvz -C /; \
    elif [[ "$ARCH" == "arm"* ]]; then \
    wget https://github.com/VictoriaMetrics/VictoriaMetrics/releases/download/$BUILD_VERSION/victoria-metrics-linux-arm-$BUILD_VERSION.tar.gz -qO- | tar xvz -C /; \
    elif [ "$ARCH" == "aarch64" ]; then \
    wget https://github.com/VictoriaMetrics/VictoriaMetrics/releases/download/$BUILD_VERSION/victoria-metrics-linux-arm64-$BUILD_VERSION.tar.gz -qO- | tar xvz -C /; \
    else \
    echo "Unsupported architecture $ARCH"; \
    return -1; \
    fi'

RUN chmod a+x /victoria-metrics-prod
RUN chmod a+x /start-victoria-metrics

CMD [ "/start-victoria-metrics"]
