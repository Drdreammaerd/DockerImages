FROM ubuntu:20.04

MAINTAINER David Wang <yung-chun@wustl.edu>

# Install ubuntu essential tools
RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
    less \
	wget \
	vim \
    zlib1g-dev \
	libbz2-dev \
	locales \ 
	ruby-full \
    build-essential \
	libdbi-perl \
	sudo \
	bzip2 \
	file \
	gdebi-core \
	&& apt-get clean -y \
	&& rm -rf /var/lib/apt/lists/*

RUN chmod -R +x usr/bin
ENV PATH /usr/bin:$PATH

# Configure default locale
RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
	&& locale-gen en_US.utf8 \
	&& /usr/sbin/update-locale LANG=en_US.UTF-8

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8

# Install MiniConda
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-py37_4.10.3-Linux-x86_64.sh -O ~/miniconda.sh && \
  /bin/bash ~/miniconda.sh -b -p /opt/conda && \
  rm ~/miniconda.sh && \
  /opt/conda/bin/conda clean -tipsy && \
  ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
  echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
  echo "conda activate base" >> ~/.bashrc && \
  find /opt/conda/ -follow -type f -name '*.a' -delete && \
  find /opt/conda/ -follow -type f -name '*.js.map' -delete && \
  /opt/conda/bin/conda clean -afy

ENV PATH /opt/conda/bin/:$PATH
RUN conda update conda

# Install mamba -> jupyter
RUN conda install -n base -c conda-forge mamba
RUN mamba install -n base -c conda-forge jupyterlab

# Install R and packages
RUN conda install -n base -c conda-forge r-devtools
RUN conda install -n base -c conda-forge r-recommended r-irkernel
RUN conda install -n base -c conda-forge -c r r-gatepoints r-optparse r-ggplot2

