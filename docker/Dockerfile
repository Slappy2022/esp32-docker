FROM debian:bullseye

RUN apt-get update && \
  apt-get install -y \
  git gcc build-essential curl pkg-config libudev-dev

RUN curl https://sh.rustup.rs -sSf | bash -s -- \
  --default-toolchain stable --profile minimal -y

ENV PATH="$PATH:/root/.cargo/bin"
RUN cargo install espup
RUN espup install

RUN cargo install cargo-espflash
WORKDIR /
RUN git clone https://github.com/esp-rs/esp-hal
WORKDIR esp-hal/esp32-hal
RUN sed -i 's/gpio15/gpio2/g' examples/blinky.rs
RUN . /root/export-esp.sh && cargo build --example blinky

CMD . /root/export-esp.sh && cargo espflash --example blinky --speed 115200 /dev/ttyUSB0 --monitor
