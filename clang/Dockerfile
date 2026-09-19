ARG BASE_IMAGE=ubuntu:rolling
FROM ${BASE_IMAGE}

ARG CLANG_VERSION
ARG INSTALL_CLANG=false
ENV DEBIAN_FRONTEND=noninteractive

RUN if grep -q "ID=debian" /etc/os-release; then \
      . /etc/os-release; \
      case "${VERSION_CODENAME:-}" in \
        bullseye) \
          echo "Debian Bullseye detected. Using a security repository snapshot..."; \
          sed -i 's|http://deb\.debian\.org/debian-security|https://snapshot.debian.org/archive/debian-security/20260903T220410Z|g' /etc/apt/sources.list; \
          echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99-ignore-valid-until; \
          ;; \
      esac; \
    fi \
    && apt-get update \
    && apt-get install --no-install-recommends --yes \
        ca-certificates \
        curl \
        git \
        make \
    && if [ "${INSTALL_CLANG}" = "true" ]; then \
         if [ "${CLANG_VERSION}" -gt 22 ]; then \
           . /etc/os-release; \
           curl -fsSL https://apt.llvm.org/llvm-snapshot.gpg.key \
             > /etc/apt/trusted.gpg.d/apt.llvm.org.asc; \
           echo "deb http://apt.llvm.org/${VERSION_CODENAME}/ llvm-toolchain-${VERSION_CODENAME}-${CLANG_VERSION} main" \
             > /etc/apt/sources.list.d/llvm.list; \
           apt-get update; \
         fi; \
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
