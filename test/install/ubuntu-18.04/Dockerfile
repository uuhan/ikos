FROM ubuntu:18.04
MAINTAINER Maxime Arthaud <maxime.arthaud@nasa.gov>
ARG njobs=2
ARG build_type=Release

# Installs the following versions (note that it might be out of date):
# cmake 3.10.2
# gmp 6.1.2
# boost 1.65.1
# python 2.7.15
# sqlite 3.22.0
# llvm 7.0.1
# clang 7.0.1
# gcc 7.3.0

# Upgrade
RUN apt-get update
RUN apt-get upgrade -y

# Add ppa for llvm 7.0
RUN echo "deb http://apt.llvm.org/bionic/ llvm-toolchain-bionic-7 main" >> /etc/apt/sources.list

# Add llvm repository key
RUN apt-get install -y wget gnupg
RUN wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | apt-key add -

# Refresh cache
RUN apt-get update

# Install all dependencies
RUN apt-get install -y gcc g++ cmake libgmp-dev libboost-dev \
        libboost-filesystem-dev libboost-test-dev python python-pygments \
        libsqlite3-dev libz-dev libedit-dev llvm-7 llvm-7-dev llvm-7-tools clang-7

# Add ikos source code
ADD . /root/ikos

# Build ikos
RUN rm -rf /root/ikos/build && mkdir /root/ikos/build
WORKDIR /root/ikos/build
ENV MAKEFLAGS "-j$njobs"
RUN cmake \
        -DCMAKE_INSTALL_PREFIX="/opt/ikos" \
        -DCMAKE_BUILD_TYPE="$build_type" \
        -DLLVM_CONFIG_EXECUTABLE="/usr/lib/llvm-7/bin/llvm-config" \
        ..
RUN make
RUN make install

# Run the tests
RUN make check

# Add ikos to the path
ENV PATH "/opt/ikos/bin:$PATH"

# Done
WORKDIR /
