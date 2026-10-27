FROM ubuntu:26.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
      autoconf autoconf-archive automake build-essential ca-certificates \
      clang curl dbus-daemon git libclang-rt-dev \
      iproute2 libssl-dev libtss2-dev libtss2-tcti-swtpm0 \
      libtss2-tcti-tabrmd0 libtool make openssl pkg-config swtpm \
      systemd tpm2-abrmd tpm2-tools \
    && rm -rf /var/lib/apt/lists/*

COPY . /src
WORKDIR /src

RUN useradd --create-home tpm2 \
    && chown -R tpm2:tpm2 /src \
    && su -s /bin/bash tpm2 -c 'cd /src \
      && ./test/run-with-simulator swtpm \
      && make distclean \
      && ./bootstrap \
      && ./configure CC=clang --enable-op-digest --enable-op-cipher --enable-asan --enable-debug \
      && make -j"$(nproc)"'
