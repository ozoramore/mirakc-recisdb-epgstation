# Dockerfile
FROM rust:slim AS builder
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y git curl wget ninja-build build-essential cmake g++ autoconf pkg-config libtool libclang-dev cmake libdvbv5-dev libpcsclite-dev libudev-dev pkg-config
WORKDIR /home/rust/recisdb-rs
COPY ./recisdb-rs/ .
RUN cargo build --release

WORKDIR /home/rust/mirakc
COPY ./mirakc/ .
RUN cargo build --release

WORKDIR /home/rust/mirakc-arib
RUN git clone --recursive https://github.com/mirakc/mirakc-arib.git .
RUN cmake -S . -B build -G Ninja -D CMAKE_BUILD_TYPE=Release
RUN ninja -C build vendor
RUN ninja -C build

FROM debian:stable-slim
RUN apt update ; apt upgrade -y ; apt install -y libpcsclite-dev
WORKDIR /usr/local/bin
COPY --from=builder /home/rust/recisdb-rs/target/release/recisdb .
COPY --from=builder /home/rust/recisdb-rs/target/release/libb25_sys.rlib .
COPY --from=builder /home/rust/mirakc/target/release/mirakc .
COPY --from=builder /home/rust/mirakc/resources/strings.yml /etc/mirakc/strings.yml
COPY --from=builder /home/rust/mirakc/target/release/mirakc-timeshift-fs .
COPY --from=builder /home/rust/mirakc-arib/build/bin/mirakc-arib .

ENV MIRAKC_CONFIG=/etc/mirakc/config.yml
ENV MIRAKC_BUILD_PROFILE=$PROFILE
EXPOSE 40772
ENTRYPOINT ["/usr/local/bin/mirakc"]
CMD []
