FROM centos:7
MAINTAINER Maxime Arthaud <maxime.arthaud@nasa.gov>
ARG njobs=2
ARG build_type=Release

# Installs the following versions (note that it might be out of date):
# cmake 3.13.1
# gmp 6.0.0
# boost 1.68.0
# python 2.7.5
# sqlite 3.7.17
# llvm 7.0.1
# clang 7.0.1
# gcc 7.3.1

# Upgrade
RUN yum -y update

# Install dependencies using CentOS Sofware Collections Repository
RUN yum -y install which patch bzip2 xz zlib-devel libedit-devel make gmp-devel python python-pygments sqlite-devel
RUN yum -y install centos-release-scl
RUN yum -y install devtoolset-7-gcc devtoolset-7-gcc-c++

# Update env to use devtoolset-7
ENV PATH "/opt/rh/devtoolset-7/root/usr/bin:$PATH"
ENV LD_LIBRARY_PATH "/opt/rh/devtoolset-7/root/usr/lib64:/opt/rh/devtoolset-7/root/usr/lib"

# Add ikos source code
ADD . /root/ikos

# Use the bootstrap script to install ikos and run the tests
WORKDIR /root/ikos/script
RUN ./bootstrap -vvf \
        --prefix="/opt/ikos" \
        --builddir="/root/ikos-build" \
        --build-type="$build_type" \
        --jobs="$njobs"

# Done
WORKDIR /
