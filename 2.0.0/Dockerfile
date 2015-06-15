# Use phusion/baseimage as base image. To make your builds reproducible, make
# sure you lock down to a specific version, not to `latest`!
# See https://github.com/phusion/baseimage-docker/blob/master/Changelog.md for
# a list of version numbers.
FROM phusion/baseimage:0.9.16

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

# ...put your own build instructions here...
RUN apt-get update && apt-get install -y \
  gcc \
  make

ENV DPDK_VERSION 2.0.0
ENV RTE_SDK /usr/src/dpdk
#ENV RTE_TARGET x86_64-native-linuxapp-gcc

RUN curl -sSL http://dpdk.org/browse/dpdk/snapshot/dpdk-${DPDK_VERSION}.tar.gz | tar -xz
RUN mv dpdk-${DPDK_VERSION} ${RTE_SDK}

# set RTE_TARGET by sourcing dpdk_env.sh, should be in the build dir of the child image
# must be sourced in beginning of any RUN instruction that needs it
ONBUILD COPY dpdk_env.sh ${RTE_SDK}/

ONBUILD RUN apt-get update && apt-get install -y \
  linux-headers-`uname -r`

ONBUILD RUN . ${RTE_SDK}/dpdk_env.sh; \
  cd ${RTE_SDK} && make install T=${RTE_TARGET}

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
