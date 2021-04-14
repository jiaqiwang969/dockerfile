FROM ubuntu:bionic

RUN cd /home

RUN apt update
RUN apt-get -qq update
RUN apt-get install -y g++ gcc gfortran ghostscript m4 make patch pkg-config wget python unzip libopenblas-dev liblapack-dev libhdf5-dev libscotch-dev libfftw3-dev libarpack2-dev libsuitesparse-dev libmumps-seq-dev libnlopt-dev coinor-libipopt-dev libgmm++-dev libtet1.5-dev gnuplot-qt autoconf automake autotools-dev bison flex gdb valgrind git cmake mpich
RUN git clone https://github.com/FreeFem/FreeFem-sources
RUN git clone https://gitlab.com/petsc/petsc
RUN export FF_DIR=${PWD}/FreeFem-sources
RUN export PETSC_DIR=${PWD}/petsc
RUN export PETSC_ARCH=arch-FreeFem
RUN export PETSC_VAR=${PETSC_DIR}/${PETSC_ARCH}
RUN cd ${PETSC_DIR} && ./configure --download-mumps --download-parmetis --download-metis --download-hypre --download-superlu --download-slepc --download-hpddm --download-ptscotch --download-suitesparse --download-scalapack --download-tetgen --with-fortran-bindings=no --with-scalar-type=real --with-debugging=no
RUN make 
RUN export PETSC_ARCH=arch-FreeFem-complex
RUN ./configure --with-mumps-dir=arch-FreeFem --with-parmetis-dir=arch-FreeFem --with-metis-dir=arch-FreeFem --with-superlu-dir=arch-FreeFem --download-slepc --download-hpddm --with-ptscotch-dir=arch-FreeFem --with-suitesparse-dir=arch-FreeFem --with-scalapack-dir=arch-FreeFem --with-tetgen-dir=arch-FreeFem --with-fortran-bindings=no --with-scalar-type=complex --with-debugging=no
RUN make
RUN cd ${FF_DIR}
RUN git checkout develop
RUN autoreconf -i
RUN ./configure --without-hdf5 --disable-iohdf5 --with-petsc=${PETSC_VAR}/lib --with-petsc_complex=${PETSC_VAR}-complex/lib
RUN make -j4
RUN export PATH=${PATH}:${FF_DIR}/src/mpi
RUN export PATH=${PATH}:${FF_DIR}/src/nw

