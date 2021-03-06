# Copyright (c) 2018, 2018 IBM Corp. and others
#
# This program and the accompanying materials are made available under
# the terms of the Eclipse Public License 2.0 which accompanies this
# distribution and is available at https://www.eclipse.org/legal/epl-2.0/
# or the Apache License, Version 2.0 which accompanies this distribution and
# is available at https://www.apache.org/licenses/LICENSE-2.0.
#
# This Source Code may also be made available under the following
# Secondary Licenses when the conditions for such availability set
# forth in the Eclipse Public License, v. 2.0 are satisfied: GNU
# General Public License, version 2 with the GNU Classpath
# Exception [1] and GNU General Public License, version 2 with the
# OpenJDK Assembly Exception [2].
#
# [1] https://www.gnu.org/software/classpath/license.html
# [2] http://openjdk.java.net/legal/assembly-exception.html
#
# SPDX-License-Identifier: EPL-2.0 OR Apache-2.0 OR GPL-2.0 WITH Classpath-exception-2.0 OR LicenseRef-GPL-2.0 WITH Assembly-exception

# To use this docker file:
#   docker build -t=openj9-jdk10 .
#   docker run --name=jdk10 -it openj9-jdk10

FROM ubuntu:18.04

# Install the required tools.
RUN apt-get update \
 && apt-get install -qq -y --no-install-recommends \
	  autoconf \
	  ca-certificates \
	  ccache \
	  cmake \
	  cpio \
	  file \
	  g++-4.8 \
	  gcc-4.8 \
	  git \
	  libasound2-dev \
	  libcups2-dev \
	  libdwarf-dev \
	  libelf-dev \
	  libfontconfig1-dev \
	  libfreetype6-dev \
	  libx11-dev \
	  libxext-dev \
	  libxrender-dev \
	  libxt-dev \
	  libxtst-dev \
	  make \
	  pkg-config \
      software-properties-common \
	  ssh \
	  unzip \
	  wget \
	  zip \
 && rm -rf /var/lib/apt/lists/*

# Install optional tools.
RUN apt-get update \
 && apt-get install -qq -y --no-install-recommends \
	  less \
	  vim \
 && rm -rf /var/lib/apt/lists/*

# Create links to the required compiler version.
RUN ln -s gcc-4.8 /usr/bin/cc \
 && ln -s gcc-4.8 /usr/bin/gcc \
 && ln -s g++-4.8 /usr/bin/c++ \
 && ln -s g++-4.8 /usr/bin/g++

# Download and unpack freemarker.
RUN cd /root \
 && wget https://sourceforge.net/projects/freemarker/files/freemarker/2.3.8/freemarker-2.3.8.tar.gz/download -O freemarker.tar.gz \
 && tar -xzf freemarker.tar.gz freemarker-2.3.8/lib/freemarker.jar --strip=2 \
 && rm -f freemarker.tar.gz

# Download the boot JDK from AdoptOpenJDK.
RUN cd /root \
 && wget https://api.adoptopenjdk.net/openjdk9-openj9/releases/s390x_linux/latest/binary -O bootjdk9.tar.gz \
 && tar -xzf bootjdk9.tar.gz \
 && rm -f bootjdk9.tar.gz \
 && mv $(ls | grep -i jdk) bootjdk9

# Set JAVA_HOME, and prepend $JAVA_HOME/bin to PATH.
ENV JAVA_HOME=/root/bootjdk9
ENV PATH="$JAVA_HOME/bin:$PATH"

WORKDIR /root
