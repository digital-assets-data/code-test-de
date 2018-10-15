FROM ubuntu:14.04 as parity
WORKDIR /build

# install tools and dependencies
RUN apt-get update && \
        apt-get install -y \
        g++ \
        build-essential \
        curl \
        git \
        file \
        binutils \
        libssl-dev \
        pkg-config \
        libudev-dev \
        software-properties-common

# install cmake 3.2
RUN add-apt-repository ppa:george-edison55/cmake-3.x
RUN apt-get update
RUN apt-get install cmake

# install rustup
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y

# rustup directory
ENV PATH /root/.cargo/bin:$PATH

# show backtraces
ENV RUST_BACKTRACE 1

# show tools
RUN rustc -vV && \
cargo -V && \
gcc -v && \
g++ -v

# build parity
RUN git clone https://github.com/paritytech/parity.git
RUN cd parity && \
        git checkout stable && \
        cargo build --release --verbose && \
        ls /build/parity/target/release/parity && \
        strip /build/parity/target/release/parity

RUN file /build/parity/target/release/parity

RUN rm -rf /var/lib/apt/lists/*

FROM parity

ARG CHAIN=expanse
# need to define this as an env var to parse it in the CMD
ENV CHAIN=$CHAIN
# TODO use this
ARG DATAPATH=/root/parity/data
# RPC port, P2P port
EXPOSE 8545 30303

HEALTHCHECK --interval=120s --timeout=120s --retries=5 CMD curl --data '{"method":"net_listening","params":[],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST localhost:8545

VOLUME /root/parity/data

CMD ["/bin/bash", "-c", "parity/target/release/parity --cache-size=2000 --auto-update=critical --chain=$CHAIN --port=30303 --jsonrpc-port=8545 --jsonrpc-interface=0.0.0.0 --jsonrpc-cors=all -d /root/parity/data"]
