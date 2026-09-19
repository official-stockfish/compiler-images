ARG BASE_IMAGE=gcc:15
FROM ${BASE_IMAGE}

ENV DEBIAN_FRONTEND=noninteractive

RUN set -eux; \
    if [ -f /usr/lib/x86_64-linux-gnu/libstdc++.so.6 ]; then \
      export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libstdc++.so.6; \
    fi; \
    apt-get update \
    && apt-get install --no-install-recommends --yes \
        ca-certificates \
        curl \
        git \
        make \
    && rm -rf /var/lib/apt/lists/*
