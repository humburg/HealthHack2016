FROM ubuntu:16.04
MAINTAINER Peter Humburg <p.humburg@garvan.org.au>

LABEL org.label-schema.name="HealthHack2016" \
      org.label-schema.description="Development environment for deep sequencing project at HealthHack2016" \
      org.label-schema.schema-version="1.0"

## Install dependencies through distribution packages
RUN apt-get update && apt-get install -y \
    less \
    libncurses5-dev \
    man \
    nano \
    openjdk-8-jdk \
    python3 \
    python3-nose \
    python3-pip \
    vim \
    wget \
    zlib1g-dev \
    zip \
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
    cp pear-0.9.10-bin-64/pear-0.9.10-bin-64 /usr/local/bin/pear && \
    cd / && rm -rf /tmp/download

## install fastQC
RUN mkdir -p /tmp/download && cd /tmp/download && \
    wget http://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.5.zip && \
    unzip fastqc_v0.11.5.zip && chmod +x FastQC/fastqc && \
    mv FastQC /usr/local/FastQC && \
    cp -s /usr/local/FastQC/fastqc /usr/local/bin/ && \
    cd / && rm -rf /tmp/download

## Install latest version of python packages
RUN pip3 install cython numpy && \
    pip3 install \
        argparse \
        biopython \
        coverage \
        pysam \
        python-Levenshtein && \
    rm -r /root/.cache
    
## Add utility scripts
COPY bin/*  /tmp/utils/
RUN chmod +x /tmp/utils/*  && mv /tmp/utils/* /usr/local/bin/ && \
    rm -r /tmp/utils

VOLUME ["/data/reference", "/data/gmap", "/data/fastq"]