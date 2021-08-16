FROM ubuntu:20.04

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    less wget vim git automake \
    libpcre3 libpcre3-dev \
    zlib1g-dev libbz2-dev \
    build-essential && \
    apt-get clean -y

COPY Triodenovo.0.04.tar.gz /opt/Triodenovo.0.04.tar.gz
RUN cd /opt && tar zxvf Triodenovo.0.04.tar.gz && rm Triodenovo.0.04.tar.gz

COPY Argument.h /opt/triodenovo.0.04/base/Argument.h
COPY tabix-0.2.4.tar.bz2 /opt/triodenovo.0.04/third/tabix-0.2.4.tar.bz2
COPY Makefile /opt/triodenovo.0.04/third/Makefile

RUN cd /opt/triodenovo.0.04 && make
RUN rm /opt/triodenovo.0.04/third/*.tar.*
RUN chmod -R +x /opt/triodenovo.0.04

ENV PATH /opt/triodenovo.0.04/src:$PATH
