Bootstrap: docker
From: ubuntu:latest

%runscript

    # echo "Arguments received: $*"
    export FASTICLIP_PATH=/FAST-iCLIP
    export CONDA=/conda
    export PATH=$CONDA/bin:$PATH
    
    exec fasticlip "$@"
  
%test

    echo "backend: Agg" > $HOME/.config/matplotlib/matplotlibrc          
    export FASTICLIP_PATH=/FAST-iCLIP
    export CONDA=/conda
    export PATH=$CONDA/bin:$PATH
    
    cd /FAST-iCLIP
    fasticlip -i rawdata/A_Human_Test_R1.fastq rawdata/A_Human_Test_R2.fastq --GRCh38 -s docs/GRCh38/GRCh38_STAR/ -n A_Human -o results
    
%environment
    CONDA=/conda
    PATH=$CONDA/bin:$PATH
    FASTICLIP_PATH=/FAST-iCLIP
    
%post
    export CONDA=/conda
    export PATH=$CONDA/bin:$PATH
    export FASTICLIP_PATH=/FAST-iCLIP
    
    alias ll="ls -l"
    
    apt-get update \
      && apt-get -y install \
            build-essential \
            bzip2 \
            git \
            sudo \
            wget \
            zip \
            nano

    # conda/pip available stuff:
    cd ~ \
      && wget https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh \
      && bash Miniconda2-latest-Linux-x86_64.sh -b -p $CONDA \
      && conda update -y -p $CONDA conda \
      && conda config --add channels conda-forge \
      && conda config --add channels defaults \
      && conda config --add channels r \
      && conda config --add channels bioconda \
      && conda install -y \
                    matplotlib=1.5.3 \
                    pandas=0.18.1 \
                    bedtools=2.25.0 \
                    pybedtools \
                    fastx_toolkit=0.0.14
    
    # Less automatic stuff:

     cd / \
      && mkdir software \
      && cd software \
      && wget https://github.com/alexdobin/STAR/archive/STAR_2.4.2a.tar.gz \
      && tar xfz STAR_2.4.2a.tar.gz \
      && cp STAR-STAR_2.4.2a/bin/Linux_x86_64_static/STAR $CONDA/bin \
      && wget -O bowtie2-2.1.0-linux-x86_64.zip \
          https://sourceforge.net/projects/bowtie-bio/files/bowtie2/2.1.0/bowtie2-2.1.0-linux-x86_64.zip/download \
      && unzip bowtie2-2.1.0-linux-x86_64.zip \
      && cp bowtie2-2.1.0/bowtie2* bowtie2-2.1.0/scripts/* $CONDA/bin \
      && wget http://hannonlab.cshl.edu/fastx_toolkit/fastx_toolkit_0.0.13_binaries_Linux_2.6_amd64.tar.bz2 \
      && tar xvjf fastx_toolkit_0.0.13_binaries_Linux_2.6_amd64.tar.bz2 \
      && cp bin/fast* $CONDA/bin \
      && wget http://www.biolab.si/iCLIPro/dist/iCLIPro-0.1.1.tar.gz \
      && tar xfz iCLIPro-0.1.1.tar.gz \
      && cd iCLIPro-0.1.1/ \
      && python setup.py install \
      && cd ../ \
      && rm -f *.tar.gz *.zip 
      
    cd ~ \
      && git clone https://github.com/ChangLab/FAST-iCLIP.git $FASTICLIP_PATH \
      && cd $FASTICLIP_PATH \
      && ./configure \
      && python setup.py install
    
    # build matplotlib font cache so we don't have that warning later            
    echo "backend: Agg" > $HOME/.config/matplotlib/matplotlibrc          
