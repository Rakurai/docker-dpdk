FROM ubuntu:vivid

RUN apt-get update && apt-get install -y --no-install-recommends \
  gcc \
  build-essential \
  make \
  curl \
  && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV DPDK_VERSION=1.7.1 \
  RTE_SDK=/usr/src/dpdk

RUN curl -sSL http://dpdk.org/browse/dpdk/snapshot/dpdk-${DPDK_VERSION}.tar.gz | tar -xz; \
  mv dpdk-${DPDK_VERSION} ${RTE_SDK}

# don't build kernel modules
RUN sed -i s/CONFIG_RTE_EAL_IGB_UIO=y/CONFIG_RTE_EAL_IGB_UIO=n/ ${RTE_SDK}/config/common_linuxapp \
  && sed -i s/CONFIG_RTE_LIBRTE_KNI=y/CONFIG_RTE_LIBRTE_KNI=n/ ${RTE_SDK}/config/common_linuxapp

# don't build unnecessary stuff, can be reversed in dpdk_env.sh
RUN sed -i s/CONFIG_RTE_APP_TEST=y/CONFIG_RTE_APP_TEST=n/ ${RTE_SDK}/config/common_linuxapp \
  && sed -i s/CONFIG_RTE_TEST_PMD=y/CONFIG_RTE_TEST_PMD=n/ ${RTE_SDK}/config/common_linuxapp

# set RTE_TARGET by sourcing dpdk_env.sh, should be in the build dir of the child image
# must be sourced in beginning of any RUN instruction that needs it
ONBUILD COPY dpdk_env.sh ${RTE_SDK}/
ONBUILD COPY dpdk_config.sh ${RTE_SDK}/

ONBUILD RUN \
  . ${RTE_SDK}/dpdk_env.sh; \
  . ${RTE_SDK}/dpdk_config.sh; \
  cd ${RTE_SDK} \
  && make install T=${RTE_TARGET} -j20 \
#  && make clean
