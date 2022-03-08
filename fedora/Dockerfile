FROM fedora:latest

RUN dnf install -y https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
RUN dnf update -y
RUN dnf install -y git make cmake openmpi openmpi-devel hdf5 hdf5-devel python3-pip m4 \
                   hdf5-openmpi-devel python3 g++ ninja-build environment-modules python3-devel

ENV OMPI_ALLOW_RUN_AS_ROOT=1
ENV OMPI_ALLOW_RUN_AS_ROOT_CONFIRM=1
ENV TMPDIR=/tmp
ENV MODULEPATH=/etc/scl/modulefiles:/etc/scl/modulefiles:/usr/share/Modules/modulefiles:/etc/modulefiles:/usr/share/modulefiles

# https://github.com/open-mpi/ompi/issues/4948#issuecomment-395468231
ENV OMPI_MCA_btl_vader_single_copy_mechanism=none

RUN eval `modulecmd bash load mpi/openmpi-x86_64`;  \
    git clone https://github.com/PHAREHUB/PHARE -b master --depth 1 --recursive --shallow-submodules phare; \
      cd phare; python3 -m pip install -r requirements.txt; \
      mkdir build; cd build; cmake .. -G Ninja; ninja; cd ..; \
      PYTHONPATH=$PWD:$PWD/build:$PWD/pyphare mpirun -n 2 ./build/src/phare/phare-exe src/phare/phare_init_small.py
