FROM ubuntu:vivid

RUN apt-get update && apt-get install -y --no-install-recommends \
  gcc \
  build-essential \
  make \
  curl \
  && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV DPDK_VERSION=2.0.0 \
  RTE_SDK=/usr/src/dpdk

RUN curl -sSL http://dpdk.org/browse/dpdk/snapshot/dpdk-${DPDK_VERSION}.tar.gz | tar -xz; \
  mv dpdk-${DPDK_VERSION} ${RTE_SDK}

# set RTE_TARGET by sourcing dpdk_env.sh, should be in the build dir of the child image
# must be sourced in beginning of any RUN instruction that needs it
ONBUILD COPY dpdk_env.sh ${RTE_SDK}/

ONBUILD RUN apt-get update && apt-get install -y --no-install-recommends \
  linux-headers-`uname -r` \
  && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ONBUILD RUN . ${RTE_SDK}/dpdk_env.sh; \
  cd ${RTE_SDK} \
  && make install T=${RTE_TARGET} \
  && make clean
