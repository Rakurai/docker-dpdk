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
  make \
  linux-headers-`uname -r`

ENV RTE_SDK=/usr/src/dpdk \
    RTE_TARGET=x86_64-native-linuxapp-gcc

RUN curl -sSL http://dpdk.org/browse/dpdk/snapshot/dpdk-2.0.0.tar.gz \
  | tar -C /usr/src -xz
RUN mv /usr/src/dpdk-2.0.0 $RTE_SDK

RUN cd $RTE_SDK && make install T=$RTE_TARGET

#ENTRYPOINT ["tools/setup.sh"]

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
