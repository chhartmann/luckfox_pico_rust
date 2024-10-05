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
    libssl-dev pkg-config qemu-user file cpio rsync wget \
    git ssh make gcc gcc-multilib g++-multilib module-assistant expect g++ gawk texinfo bison flex fakeroot cmake gperf autoconf \
    device-tree-compiler libncurses5-dev pkg-config bc python-is-python3 passwd openssl openssh-server openssh-client vim \
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

# Set default toolchain
RUN rustup default stable
RUN rustup component add clippy
RUN rustup component add rustfmt

#RUN rustup toolchain install stable --component rust-src

# Setup buildroot incl. toolchain with new uclibc
RUN git clone https://github.com/LuckfoxTECH/luckfox-pico.git /home/luckfox/luckfox-pico
WORKDIR /home/luckfox/luckfox-pico
COPY --chown=luckfox FilesForDocker/buildroot_defconfig config/
COPY --chown=luckfox FilesForDocker/kernel_defconfig config/
COPY --chown=luckfox FilesForDocker/dts_config config/
RUN ln -s project/cfg/BoardConfig_IPC/BoardConfig-SPI_NAND-Buildroot-RV1106_Luckfox_Pico_Pro_Max-IPC.mk .BoardConfig.mk
RUN touch /home/${CONTAINER_USER}/.bash_profile
SHELL ["/bin/bash", "-c"]
WORKDIR ./tools/linux/toolchain/arm-rockchip830-linux-uclibcgnueabihf
RUN source env_install_toolchain.sh && cd /home/luckfox/luckfox-pico && ./build.sh

CMD [ "/bin/bash" ]