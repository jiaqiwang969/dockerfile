# DockerFile for Firedrake in complex mode with a full set of capabilities.

FROM firedrakeproject/firedrake-env:latest

# This DockerFile is looked after by
MAINTAINER David Ham <david.ham@imperial.ac.uk>

USER firedrake
WORKDIR /home/firedrake

# Now install Firedrake.
RUN curl -O https://raw.githubusercontent.com/firedrakeproject/firedrake/master/scripts/firedrake-install
RUN bash -c "python3 firedrake-install --complex --tinyasm --slepc --no-package-manager --disable-ssh --documentation-dependencies"
RUN pip3 install scipy

# 将Petsc文件的bin导入到petsc路径中
RUN cd /home
RUN git clone https://github.com/jiaqiwang969/petsc.git
#RUN cp -r petsc/bin firedrake/firedrake/src/petsc/
# 下载最新的nthRootsOfUnity
RUN git clone https://github.com/jiaqiwang969/nthRootsOfUnity.git

RUN echo "export PETSC_DIR=/home/firedrake/firedrake/src/petsc" >> ~/.bashrc
RUN echo "export PETSC_ARCH=default" >> ~/.bashrc
RUN echo "export PYTHONPATH=/home/nthRootsOfUnity/src:/home/petsc/bin" >> ~/.bashrc
#RUN source ~/.bashrc
RUN export PETSC_DIR=/home/firedrake/firedrake/src/petsc
RUN export PETSC_ARCH=default
RUN export export PYTHONPATH=/home/nthRootsOfUnity/src:/home/petsc/bin
RUN cd /home/nthRootsOfUnity/examples
RUN python3 0-ComputeBaseflow.py
RUN python3 1-ExtractMatrices.py
RUN bash 2a-ComputeSpectrum.sh
RUN python3 2b-ViewSpectrum.py
RUN bash 3a-ComputeFrequencyResponse.sh
RUN python3 3b-ViewFrequencyResponse.py

