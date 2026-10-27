FROM ubuntu:26.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
      autoconf autoconf-archive automake build-essential ca-certificates clang \
      git libclang-rt-dev libcmocka-dev libcurl4-openssl-dev libftdi-dev \
      libini-config-dev libjson-c-dev libltdl-dev libssl-dev libtool \
      libtpms-dev libusb-1.0-0-dev pkg-config uuid-dev \
    && rm -rf /var/lib/apt/lists/*

COPY . /src
WORKDIR /src

RUN ./bootstrap \
    && CC=clang ./configure \
      --enable-debug \
      --enable-unit \
      --enable-fapi \
      --enable-policy \
      --enable-vendor \
      --enable-util-io \
      --enable-tcti-device \
      --enable-tcti-mssim \
      --enable-tcti-swtpm \
      --enable-tcti-pcap \
      --enable-tcti-null \
      --enable-tcti-libtpms \
      --enable-tcti-cmd \
      --enable-tcti-spi-helper \
      --enable-tcti-spi-ltt2go \
      --enable-tcti-spidev \
      --enable-tcti-spi-ftdi \
      --enable-tcti-i2c-helper \
      --enable-tcti-i2c-ftdi \
      --disable-doxygen-doc \
      --with-sanitizer=undefined,address \
    && make -j"$(nproc)" \
    && make distclean \
    && GEN_FUZZ=1 CC=clang ./configure \
      --enable-debug \
      --enable-unit \
      --with-fuzzing=libfuzzer \
      --enable-tcti-fuzzing \
      --disable-tcti-device \
      --disable-tcti-mssim \
      --disable-tcti-swtpm \
      --disable-doxygen-doc \
      --disable-shared \
      --disable-fapi \
      --disable-policy \
      --with-sanitizer=undefined,address \
    && make -j"$(nproc)" fuzz-targets
