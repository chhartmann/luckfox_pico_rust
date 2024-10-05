# Base image
ARG VARIANT=bookworm-slim
FROM debian:${VARIANT}
ENV DEBIAN_FRONTEND=noninteractive
ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8

# Arguments
ARG CONTAINER_USER=luckfox
ARG CONTAINER_GROUP=luckfox
ARG GITHUB_TOKEN

# Install dependencies
RUN apt-get update \
    && apt-get install -y git curl llvm-dev libclang-dev clang unzip gdb-multiarch \
    libssl-dev pkg-config \
    && apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/library-scripts

# Set users
RUN adduser --disabled-password --gecos "" ${CONTAINER_USER}
USER ${CONTAINER_USER}
WORKDIR /home/${CONTAINER_USER}

# Install rustup
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- \
    --default-toolchain none -y --profile minimal

# Update envs
ENV PATH=${PATH}:/home/${CONTAINER_USER}/.cargo/bin

#RUN rustup toolchain install stable --component rust-src


# Set default toolchain
RUN rustup default stable
RUN rustup component add clippy
RUN rustup component add rustfmt

# Activate ESP environment
#RUN echo "source /home/${CONTAINER_USER}/export-esp.sh" >> ~/.bashrc

CMD [ "/bin/bash" ]