FROM ubuntu:20.04

MAINTAINER David Wang <yung-chun@wustl.edu>

#ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

# Install Ubuntu essential tools
RUN apt-get update --fix-missing && \
    apt-get install -y --no-install-recommends \
    wget \
    bzip2 \
    build-essential \
	locales \
    less \
    vim \
	## snakemake:
    ca-certificates \
    git \
    libglib2.0-0 \
    libxext6 \
    libsm6 \
    libxrender1 \
    mercurial \
    subversion \
    python3-dev && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

# Configure default locale
RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
	&& locale-gen en_US.utf8 \
	&& /usr/sbin/update-locale LANG=en_US.UTF-8

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

# Install mamba
RUN conda install -n base -c conda-forge mamba
RUN mamba install -n base -c conda-forge -c bioconda snakemake pysam 
RUN mamba install -n base -c anaconda pandas

# Install R and packages
RUN conda install -n base -c bioconda r-optparse r-pscbs
RUN conda install -n base -c conda-forge r-tidyverse r-bbmle r-recommended

# Install Triomix
RUN mkdir -p /tools/triomix
RUN git clone https://github.com/cjyoon/triomix.git /tools/triomix

# Define default command
WORKDIR /tools/triomix
CMD ["bash", "-c", "source /opt/conda/etc/profile.d/conda.sh && conda activate base && python triomix.py -h"]


