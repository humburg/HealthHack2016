FROM ubuntu:16.04
MAINTAINER Peter Humburg <p.humburg@garvan.org.au>

ARG VCS_REF
ARG VCS_URL
ARG BUILD_DATE
ARG VERSION
LABEL org.label-schema.build-date=$BUILD_DATE \
      org.label-schema.name="HealthHack2016" \
      org.label-schema.description="Development environment for deep sequencing project at HealthHack2016" \
      org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.vcs-url=$VCS_URL \
      org.label-schema.version=$VERSION \
      org.label-schema.schema-version="1.0"

## Install dependencies through distribution packages
RUN apt-get update && apt-get install -y \
    python3 \
    python3-nose \
    python3-pip \
    zlib1g-dev \
    zip \
    libncurses5-dev \
    wget \
    openjdk-8-jdk \
    && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

## Install samtools et al. from source
RUN mkdir -p /tmp/download && cd /tmp/download && \
    wget https://github.com/samtools/samtools/releases/download/1.3.1/samtools-1.3.1.tar.bz2 && \
    tar -xf samtools-1.3.1.tar.bz2 && \
    cd samtools-1.3.1 && \
    make && make install && \
    cd .. && wget https://github.com/samtools/bcftools/releases/download/1.3.1/bcftools-1.3.1.tar.bz2 && \
    tar -xf bcftools-1.3.1.tar.bz2 && \
    cd bcftools-1.3.1 && make && make install && \
    cd .. && wget https://github.com/samtools/htslib/releases/download/1.3.1/htslib-1.3.1.tar.bz2 && \
    tar -xf htslib-1.3.1.tar.bz2 && \
    cd htslib-1.3.1 && make && make install && \
    cd / && rm -rf /tmp/download

## Install Picard
RUN mkdir -p /tmp/download && cd /tmp/download && \
    wget https://github.com/broadinstitute/picard/releases/download/2.2.4/picard-tools-2.2.4.zip && \
    unzip picard-tools-2.2.4.zip && \
    mkdir -p /tools/picard && \
    cp picard-tools-2.2.4/* /tools/picard && \
    cd / && rm -rf /tmp/download
ENV PICARD /tools/picard/picard.jar

## Install GMAP
RUN mkdir -p /tmp/download && cd /tmp/download && \
    wget http://research-pub.gene.com/gmap/src/gmap-gsnap-2016-09-23.tar.gz && \
    tar -xf gmap-gsnap-2016-09-23.tar.gz && cd gmap-2016-09-23 && \
    ./configure --with-gmapdb=/data/gmap MAX_READLENGTH=500 && \
    make && make install && \
    cd / && rm -rf /tmp/download

## install PEAR
RUN mkdir -p /tmp/download && cd /tmp/download && \
    wget http://sco.h-its.org/exelixis/web/software/pear/files/pear-0.9.10-bin-64.tar.gz && \
    tar -xf pear-0.9.10-bin-64.tar.gz && \
    cp pear-0.9.10-bin-64/pear-0.9.10-bin-64 /usr/local/bin/ && \
    cd / && rm -rf /tmp/download
    
## Install latest version of python packages
RUN pip3 install cython
RUN pip3 install \
    argparse \
    pysam \
    coverage

VOLUME ["/data/reference", "/data/gmap", "/data/fastq"]