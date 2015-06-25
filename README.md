# Docker base image with DPDK
Resources to build a Docker image containing the Data Plane Development Kit (DPDK).  Used as a base image for DPDK-accelerated applications in Docker containers.

Child build requires two files:<br />
    dpdk_env.sh - minimally exports RTE_TARGET<br />
    dpdk_config.sh - can be empty, but preconfigures DPDK options for the child

An example dpdk_env.sh is:

\#!/bin/bash<br />
export RTE_TARGET=x86_64-native-linuxapp-gcc

RTE_SDK is defined as /usr/src/dpdk

See https://github.com/rakurai/docker-ovs-dpdk for an example child build.

Tutorial on using this container:
http://jason.digitalinertia.net/dockered-dpdk-packaging-open-vswitch/
