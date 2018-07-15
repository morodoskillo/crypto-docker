# Build the Pesetacoin daemon, cli, and GUI from the code at
# https://github.com/FundacionPesetacoin/PesetacoinCore

FROM ubuntu:12.04

# 1. We need to do this first in order to see any packages at all
RUN apt-get update

# 2. Install the basic foundation of build tools, as per the docs.
RUN apt-get install -y --no-install-recommends \
    automake \
    autotools-dev \ 
    bsdmainutils \
    build-essential \ 
    libevent-dev \
    libssl-dev \
    libtool \
    pkg-config  

# 3. As per the docs, we only need to selectively install a handful of the boost libraries
RUN apt-get install -y --no-install-recommends \
    libboost-all-dev

# This is a dependency of add-apt-repository.
RUN apt-get install -y --no-install-recommends software-properties-common

# And now we need to update this again
RUN apt-get update

# Now get v5.1 of the wallet db
RUN apt-get install -y --no-install-recommends libdb5.1-dev libdb5.1++-dev

# Now miniupnpc
RUN apt-get install -y --no-install-recommends libminiupnpc-dev

# 5. Let's try to build this with QT5

RUN apt-get install -y --no-install-recommends \
    libprotobuf-dev \
    libqt5core5 \
    libqt5dbus5 \
    libqt5gui5 \
    protobuf-compiler \
    qttools5-dev \
    qttools5-dev-tools

# 6. Install these packages in order to view the QT GUI using a VNC viewer.  Which you need to do if you want to run the QT GUI inside a docker container.
RUN apt-get install -y --no-install-recommends \
    x11vnc \
    xvfb

# 7. Clone the source code.

# You'll need this for git clone
RUN apt-get install -y --no-install-recommends \
    git

RUN  git clone https://github.com/FundacionPesetacoin/PesetacoinCore.git
                                                       
WORKDIR /PesetacoinCore

# 8. Now build it!

RUN ./autogen.sh 

# Recall that by default we use the v5.1 version of the db
RUN ./configure  

RUN make && \

strip src/pesetacoind && \
strip src/pesetacoin-cli && \
strip src/qt/pesetacoin-qt && \

make install && make clean 

LABEL author.name="Thomas Radloff"

COPY entrypoint.sh /
RUN chmod 755 /entrypoint.sh

