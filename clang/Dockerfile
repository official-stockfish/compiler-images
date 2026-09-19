ARG BASE_IMAGE=ubuntu:rolling
FROM ${BASE_IMAGE}

ARG CLANG_VERSION
ARG INSTALL_CLANG=false
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
    && apt-get install --no-install-recommends --yes \
        ca-certificates \
        curl \
        git \
        make \
    && if [ "${INSTALL_CLANG}" = "true" ]; then \
         apt-get install --no-install-recommends --yes clang-${CLANG_VERSION}; \
         if [ "${CLANG_VERSION}" -ge 20 ]; then \
           apt-get install --no-install-recommends --yes lld; \
         fi; \
         if [ "${CLANG_VERSION}" -gt 20 ]; then \
           apt-get install --no-install-recommends --yes \
               lld-${CLANG_VERSION} \
               llvm-${CLANG_VERSION}-dev \
               llvm-${CLANG_VERSION}-linker-tools; \
         fi; \
       fi \
    && rm -rf /var/lib/apt/lists/*
