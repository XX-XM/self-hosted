FROM rustlang/rust:nightly as builder

RUN apt-get update --quiet && \
    apt-get install --yes --quiet --no-install-recommends\
    apt-utils \
    software-properties-common \
    lsb-release &&\
    rm -rf /var/lib/apt/lists/* && \
    bash -c "$(wget --show-progress -O - https://apt.llvm.org/llvm.sh)"

WORKDIR /rs
RUN git clone https://github.com/mkaczanowski/pastebin.git . && \
    cargo build --release

FROM debian:buster-slim
RUN apt-get update --quiet && \
    apt-get install --quiet --yes --no-install-recommends \
    gosu && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir -p /rs/db
COPY --from=builder /rs/target/release/pastebin /rs/pastebin
COPY entrypoint.sh /rs/entrypoint.sh
WORKDIR /rs

CMD ["/bin/sh", "/rs/entrypoint.sh"]
