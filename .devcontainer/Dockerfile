FROM ghcr.io/microchip-ung/bsp-buildenv:1.21 AS linux-base
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Moscow
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

FROM linux-base AS container-base

# Toolchain
RUN mkdir /opt/resources
RUN cd /opt/resources
RUN wget http://mscc-ent-open-source.s3-eu-west-1.amazonaws.com/public_root/toolchain/mscc-toolchain-bin-2020.02.3-085.tar.gz
RUN mkdir -p /opt/mscc
RUN tar xf mscc-toolchain-bin-2020.02.3-085.tar.gz -C /opt/mscc
RUN /opt/mscc/mscc-toolchain-bin-2020.02.3-085/arm-cortex_a8-linux-gnueabihf/usr/bin/arm-cortex_a8-linux-gnueabihf-gcc --version

# BRSDK
RUN wget http://mscc-ent-open-source.s3-eu-west-1.amazonaws.com/public_root/bsp/mscc-brsdk-arm64-2020.02.3-312.tar.gz
RUN mkdir -p /opt/brsdk
RUN tar xf mscc-brsdk-arm64-2020.02.3-312.tar.gz -C /opt/brsdk

# User wrap
RUN adduser --disabled-password --gecos '' dev && \
    adduser dev sudo && \
    echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
USER dev

FROM container-base AS build-and-test


SHELL ["/bin/bash", "-o", "pipefail", "-c"]
ENTRYPOINT ["make", "-C", "/home/dev/sources"]
CMD ["test"]
