FROM ubuntu:26.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
      acl autoconf autoconf-archive automake build-essential ca-certificates \
      expect git iproute2 libcmocka-dev libcurl4-openssl-dev libftdi-dev \
      libefivar-dev libini-config-dev libjson-c-dev libltdl-dev libssl-dev \
      libtool libtpms-dev libusb-1.0-0-dev libyaml-dev openssl pandoc \
      pkg-config python3-yaml swtpm tpm2-abrmd uuid-dev xxd \
    && rm -rf /var/lib/apt/lists/*

RUN git clone --depth=1 --branch 4.2.0 \
      https://github.com/tpm2-software/tpm2-tss.git /tmp/tpm2-tss \
    && cd /tmp/tpm2-tss \
    && ./bootstrap \
    && ./configure --disable-doxygen-doc \
    && make -j"$(nproc)" \
    && make install \
    && ldconfig \
    && rm -rf /tmp/tpm2-tss

COPY . /src
WORKDIR /src

RUN ./bootstrap \
    && ./configure \
      --enable-unit \
      --enable-fapi \
      --enable-persistent \
      --with-efivar=yes \
    && make -j"$(nproc)"
