FROM mcr.microsoft.com/vscode/devcontainers/base:debian-10

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# This Dockerfile's base image has a non-root user with sudo access. Use the "remoteUser"
# property in devcontainer.json to use it. On Linux, the container user's GID/UIDs
# will be updated to match your local UID/GID (when using the dockerFile property).
# See https://aka.ms/vscode-remote/containers/non-root-user for details.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=985

# Configure apt and install packages
RUN apt-get update \
    #
    # Install C++ tools
    && apt-get -y install build-essential cmake cppcheck valgrind \
    #
    # [Optional] Update UID/GID if needed
    && if [ "$USER_GID" != "1000" ] || [ "$USER_UID" != "1000" ]; then \
        groupmod --gid $USER_GID $USERNAME \
        && usermod --uid $USER_UID --gid $USER_GID $USERNAME \
        && chown -R $USER_UID:$USER_GID /home/$USERNAME; \
    fi \
    #
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=dialog

RUN cp /etc/skel/.bashrc /root && cp /etc/skel/.profile /root

RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
	bash-completion \
	bc \
	clang-7 \
	ca-certificates \
	curl \
	dkms \
	gnupg2 \
	gcc \
	jq \
	libc6-dev \
	libelf-dev \
	libmpx2 \
	llvm-7 \
	netcat \
	xz-utils \
	rpm2cpio \
	cpio \
	flex \
	bison \
	openssl \
	pkg-config \
	libssl-dev \
	libncurses-dev \
	libudev-dev \
	libpci-dev \
	libiberty-dev \
	libyaml-dev \
	libc-ares-dev \ 
	libprotobuf-dev \ 
	protobuf-compiler \
	libjq-dev \
	libyaml-cpp-dev \
	libgrpc++-dev \
	protobuf-compiler-grpc \
	libcurl4-openssl-dev \
	&& rm -rf /var/lib/apt/lists/*

# gcc 6 is no longer included in debian stable, but we need it to
# build kernel modules on the default debian-based ami used by
# kops. So grab copies we've saved from debian snapshots with the
# prefix https://snapshot.debian.org/archive/debian/20170517T033514Z
# or so.

RUN curl -L -o cpp-6_6.3.0-18_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/cpp-6_6.3.0-18_amd64.deb \
	&& curl -L -o gcc-6-base_6.3.0-18_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/gcc-6-base_6.3.0-18_amd64.deb \
	&& curl -L -o gcc-6_6.3.0-18_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/gcc-6_6.3.0-18_amd64.deb \
	&& curl -L -o libasan3_6.3.0-18_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libasan3_6.3.0-18_amd64.deb \
	&& curl -L -o libcilkrts5_6.3.0-18_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libcilkrts5_6.3.0-18_amd64.deb \
	&& curl -L -o libgcc-6-dev_6.3.0-18_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libgcc-6-dev_6.3.0-18_amd64.deb \
	&& curl -L -o libubsan0_6.3.0-18_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libubsan0_6.3.0-18_amd64.deb \
	&& curl -L -o libmpfr4_3.1.3-2_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libmpfr4_3.1.3-2_amd64.deb \
	&& curl -L -o libisl15_0.18-1_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libisl15_0.18-1_amd64.deb \
	&& dpkg -i cpp-6_6.3.0-18_amd64.deb gcc-6-base_6.3.0-18_amd64.deb gcc-6_6.3.0-18_amd64.deb libasan3_6.3.0-18_amd64.deb libcilkrts5_6.3.0-18_amd64.deb libgcc-6-dev_6.3.0-18_amd64.deb libubsan0_6.3.0-18_amd64.deb libmpfr4_3.1.3-2_amd64.deb libisl15_0.18-1_amd64.deb \
	&& rm -f cpp-6_6.3.0-18_amd64.deb gcc-6-base_6.3.0-18_amd64.deb gcc-6_6.3.0-18_amd64.deb libasan3_6.3.0-18_amd64.deb libcilkrts5_6.3.0-18_amd64.deb libgcc-6-dev_6.3.0-18_amd64.deb libubsan0_6.3.0-18_amd64.deb libmpfr4_3.1.3-2_amd64.deb libisl15_0.18-1_amd64.deb

# gcc 5 is no longer included in debian stable, but we need it to
# build centos kernels, which are 3.x based and explicitly want a gcc
# version 3, 4, or 5 compiler. So grab copies we've saved from debian
# snapshots with the prefix https://snapshot.debian.org/archive/debian/20190122T000000Z.

RUN curl -L -o cpp-5_5.5.0-12_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/cpp-5_5.5.0-12_amd64.deb \
	&& curl -L -o gcc-5-base_5.5.0-12_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/gcc-5-base_5.5.0-12_amd64.deb \
	&& curl -L -o gcc-5_5.5.0-12_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/gcc-5_5.5.0-12_amd64.deb \
	&& curl -L -o libasan2_5.5.0-12_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libasan2_5.5.0-12_amd64.deb \
	&& curl -L -o libgcc-5-dev_5.5.0-12_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libgcc-5-dev_5.5.0-12_amd64.deb \
	&& curl -L -o libisl15_0.18-4_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libisl15_0.18-4_amd64.deb \
	&& curl -L -o libmpx0_5.5.0-12_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libmpx0_5.5.0-12_amd64.deb \
	&& dpkg -i cpp-5_5.5.0-12_amd64.deb gcc-5-base_5.5.0-12_amd64.deb gcc-5_5.5.0-12_amd64.deb libasan2_5.5.0-12_amd64.deb libgcc-5-dev_5.5.0-12_amd64.deb libisl15_0.18-4_amd64.deb libmpx0_5.5.0-12_amd64.deb \
	&& rm -f cpp-5_5.5.0-12_amd64.deb gcc-5-base_5.5.0-12_amd64.deb gcc-5_5.5.0-12_amd64.deb libasan2_5.5.0-12_amd64.deb libgcc-5-dev_5.5.0-12_amd64.deb libisl15_0.18-4_amd64.deb libmpx0_5.5.0-12_amd64.deb

# gcc 4 is no longer included in debian stable, but we need it to
# build centos kernels, which are 2.x based and explicitly want a gcc
# version 4 compiler. So grab copies we've saved from debian
# snapshots with the prefix http://ftp.debian.org/debian/pool/main/g/gcc-4.8/.

RUN curl -L -o cpp-4.8_4.8.4-1_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/cpp-4.8_4.8.4-1_amd64.deb \
	&& curl -L -o gcc-4.8-base_4.8.4-1_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/gcc-4.8-base_4.8.4-1_amd64.deb \
	&& curl -L -o gcc-4.8_4.8.4-1_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/gcc-4.8_4.8.4-1_amd64.deb \
	&& curl -L -o libasan0_4.8.4-1_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libasan0_4.8.4-1_amd64.deb \
	&& curl -L -o libgcc-4.8-dev_4.8.4-1_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libgcc-4.8-dev_4.8.4-1_amd64.deb \
	&& curl -L -o libisl10_0.12.2-2_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libisl10_0.12.2-2_amd64.deb \
	&& curl -L -o multiarch-support_2.19-18+deb8u10_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/multiarch-support_2.19-18+deb8u10_amd64.deb \
	&& curl -L -o libcloog-isl4_0.18.4-1+b1_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libcloog-isl4_0.18.4-1+b1_amd64.deb \
	&& dpkg -i multiarch-support_2.19-18+deb8u10_amd64.deb \
	&& dpkg -i libisl10_0.12.2-2_amd64.deb gcc-4.8-base_4.8.4-1_amd64.deb libasan0_4.8.4-1_amd64.deb libgcc-4.8-dev_4.8.4-1_amd64.deb libcloog-isl4_0.18.4-1+b1_amd64.deb cpp-4.8_4.8.4-1_amd64.deb gcc-4.8_4.8.4-1_amd64.deb \
	&& rm -f multiarch-support_2.19-18+deb8u10_amd64.deb libisl10_0.12.2-2_amd64.deb gcc-4.8-base_4.8.4-1_amd64.deb libasan0_4.8.4-1_amd64.deb libgcc-4.8-dev_4.8.4-1_amd64.deb libcloog-isl4_0.18.4-1+b1_amd64.deb cpp-4.8_4.8.4-1_amd64.deb gcc-4.8_4.8.4-1_amd64.deb

# debian:stable head contains binutils 2.31, which generates
# binaries that are incompatible with kernels < 4.16. So manually
# forcibly install binutils 2.30-22 instead.
RUN curl -L -o binutils_2.30-22_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/binutils_2.30-22_amd64.deb \
	&& curl -L -o libbinutils_2.30-22_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/libbinutils_2.30-22_amd64.deb \
	&& curl -L -o binutils-x86-64-linux-gnu_2.30-22_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/binutils-x86-64-linux-gnu_2.30-22_amd64.deb \
	&& curl -L -o binutils-common_2.30-22_amd64.deb https://dl.bintray.com/falcosecurity/dependencies/binutils-common_2.30-22_amd64.deb \
	&& dpkg -i *binutils*.deb \
	&& rm -f *binutils*.deb

RUN ln -s /usr/bin/clang-7 /usr/bin/clang \
	&& ln -s /usr/bin/llc-7 /usr/bin/llc