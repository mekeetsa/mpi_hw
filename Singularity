# Header

Bootstrap: docker
From: ubuntu:latest

%labels
  MAINTAINER mekeetsa

# Sections

%environment
  SW_TMP=/.singularity.d/tmp-sw
  export SW_TMP

%help
  Latest Ubuntu with OpenMPI 4.0.2 and UCX 1.7.0

%runscript
  /.singularity.d/app/mpi_hello_world

%post
  echo $SW_TMP

  # Update Ubuntu

  apt-get update -y
  apt-get upgrade -y

  # Prepare the build

  mkdir /.singularity.d/tmp-sw/
  apt-get install -y build-essential wget

  # Build UCX

  cd /.singularity.d/tmp-sw/

  wget https://github.com/openucx/ucx/releases/download/v1.7.0/ucx-1.7.0.tar.gz
  tar xvfz ucx-1.7.0.tar.gz

  cd /.singularity.d/tmp-sw/ucx-1.7.0
  apt-get install -y libnuma-dev
  apt-get install -y libibverbs-dev librdmacm-dev

  ./configure --disable-logging --disable-debug --disable-assertions --disable-params-check --with-verbs --with-rc --with-ud --with-dc --enable-optimizations --enable-mt

  make
  make install
  ldconfig

  # Build OpenMPI

  cd /.singularity.d/tmp-sw/
  wget https://github.com/open-mpi/ompi/archive/v4.0.2.tar.gz
  tar xvfz v4.0.2.tar.gz

  cd /.singularity.d/tmp-sw/ompi-4.0.2/
  apt-get install -y autoconf libtool flex

  ./autogen.pl
  ./configure --with-ucx --enable-mca-no-build=btl-uct

  make
  make install
  ldconfig

  # Cleanup

  rm -rf /.singularity/tmp-sw

  # Build the MPI hello world app

  mkdir /.singularity.d/app
  cd /.singularity.d/app
  wget https://raw.githubusercontent.com/huyle333/mpi-hello-world/master/mpi_hello_world.c

  mpicc -o mpi_hello_world mpi_hello_world.c
  
