FROM ubuntu:20.04

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update --fix-missing && \
    apt-get install -y \
    wget bzip2 build-essential \
    less vim \
    gcc lbzip2 \
    zlib1g-dev \
    libbz2-dev \
    liblzma-dev \
    libcurl4-gnutls-dev \
    libssl-dev \
    libncurses5-dev \
    python2.7-dev \
    perl \
    g++

# Install R:
# https://linuxize.com/post/how-to-install-r-on-ubuntu-20-04/
RUN apt-get install -y dirmngr gnupg apt-transport-https ca-certificates software-properties-common
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
# RUN add-apt-repository -r ppa:marutter/rrutter4.0
RUN add-apt-repository universe
# RUN add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/'
RUN apt-get install -y r-base && apt-get clean -y

# Copy the pre-downloaded Tar file:
COPY LASER-2.04.tar.gz /opt
COPY HGDP-938-632958.tar.gz /opt

RUN cd /tmp \
    && wget https://github.com/samtools/samtools/releases/download/1.11/samtools-1.11.tar.bz2 \
    && tar -xf samtools-1.11.tar.bz2 \
    && cd /tmp/samtools-1.11 && ./configure --prefix=/opt && make all all-htslib && make install install-htslib \
    && ln -s /opt/bin/samtools /usr/bin/samtools \
    && ln -s /opt/bin/tabix /usr/bin/tabix \
    && ln -s /opt/bin/bgzip /usr/bin/bgzip \
    && rm /tmp/samtools-1.11.tar.bz2 \
    && rm -rf /tmp/samtools-1.11
    
RUN cd /opt/ \
    && tar zxf /opt/LASER-2.04.tar.gz \
    && rm /opt/LASER-2.04.tar.gz \
    && tar zxf /opt/HGDP-938-632958.tar.gz \
    && rm /opt/HGDP-938-632958.tar.gz \
    && ln -s /opt/LASER-2.04/trace /usr/bin/trace \
    && ln -s /opt/LASER-2.04/laser /usr/bin/laser \
    && ln -s /opt/LASER-2.04/vcf2geno/vcf2geno /usr/bin/vcf2geno \
    && ln -s /usr/bin/python2.7 /usr/bin/python
