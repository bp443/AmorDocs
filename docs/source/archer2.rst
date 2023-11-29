Installation instructions for Archer2
========================================

Python venvs
------------

::

   python -m venv --system-site-packages venv_name

QUIP
----

::

   cd /work/e89/e89/bp443

::

   module restore
   module load cpe
   module load PrgEnv-gnu
   module load cray-fftw
   module load cmake
   module swap gcc/11.2.0 gcc/10.3.0
   module load cray-python
   export LD_LIBRARY_PATH=$CRAY_LD_LIBRARY_PATH:$LD_LIBRARY_PATH#

::

   git clone --recursive https://github.com/libAtoms/QUIP.git

::

   export QUIP_ARCH=archer2
   export QUIP_ROOT="/work/e89/e89/bp443/programs/QUIP"

::

   cd QUIP
   make config

In the config, accept all the default options with the following two
exceptions:

Would you like to compile with GAP support ? [y] Would you like to
compile with many-body dispersion support ? [y]

::

   make

::


   mkdir quip_installation_directory
   export QUIP_INSTALLDIR=/work/e89/e89/bp443/programs/QUIP/quip_installation_directory

::

   make install
   make libquip

LAMMPS
--------

::

   module restore
   module load cpe
   module load PrgEnv-gnu
   module load cray-fftw
   module load cmake
   module swap gcc/11.2.0 gcc/10.3.0
   module load cray-python
   export LD_LIBRARY_PATH=$CRAY_LD_LIBRARY_PATH:$LD_LIBRARY_PATH

::

   wget https://download.lammps.org/tars/lammps-stable.tar.gz
   tar -xzf lammps-stable.tar.gz

::

   mv lammps-2Aug2023 lammps
   cd lammps
   mkdir build
   cd build
   wget -O libpace.tar.gz https://github.com/wcwitt/lammps-user-pace/archive/main.tar.gz

::

   export QUIP_ARCH=archer2
   export QUIP_ROOT="/work/e89/e89/bp443/programs/QUIP"

This part seems to be only working with user python. no venv:

::

   source /work/e89/e89/bp443/programs/python-venv/bin/activate
   export VENV_LIB_PATH=$VIRTUAL_ENV 
   deactivate

::

   cmake ../cmake/ -DCMAKE_CXX_COMPILER=CC -DBUILD_MPI=on -D BUILD_OMP=yes -D FFT=FFTW3         \
   -D FFTW3_INCLUDE_DIR=${FFTW_INC}                                      \
   -D FFTW3_LIBRARY=${FFTW_DIR}/libfftw3_mpi.so                          \
   -D PKG_ML-PACE=yes -D PKG_MANYBODY=yes -D PKG_PYTHON=yes             \
   -D PACELIB_MD5=$(md5sum libpace.tar.gz | awk '{print $1}') \
   -D BUILD_SHARED_LIBS=yes \
   -D LAMMPS_EXCEPTIONS=yes \
   -D PKG_ML-QUIP=on \
   -D DOWNLOAD_QUIP=no -D QUIP_LIBRARY=/work/e89/e89/bp443/programs/QUIP/build/archer2/libquip.a \
   -D BUILD_LIB=on -D BUILD_SHARED_LIBS=on \
   -D Python_EXECUTABLE=$(which python) \
   #-D CMAKE_INSTALL_PREFIX=$VENV_LIB_PATH

::

   make -j 8
   make install

::

   source /work/e89/e89/bp443/programs/python-venv/bin/activate

::

   make install-python

ELPA
-----

.. code:: bash

   salloc --nodes=1 --ntasks-per-node=8 --cpus-per-task=1 \
                   --time=00:20:00 --partition=standard --qos=short \
                   --account=e89-camp

.. code:: bash

   module restore
   module load PrgEnv-gnu
   module load mkl

::

   wget https://elpa.mpcdf.mpg.de/software/tarball-archive/Releases/2023.11.001.rc1/elpa-2023.11.001.rc1.tar.gz

::

   tar -xzf elpa-2023.11.001.rc1.tar.gz
   mv elpa-2023.11.001.rc1 elpa
   cd elpa
   mkdir install_dir
   mkdir build
   cd build/

.. code:: bash

   export LIBS="-L${MKLROOT}/lib/intel64 -lmkl_scalapack_lp64 -Wl,--no-as-needed -lmkl_gf_lp64 -lmkl_gnu_thread -lmkl_core -lmkl_blacs_intelmpi_lp64 -lgomp -lpthread -lm -ldl"
   ../configure    CC=cc CXX=CC FC=ftn LDFLAGS=-dynamic    \
     --enable-openmp=yes --enable-shared=no  --with-mpi=yes \
     --disable-avx512    --enable-allow-thread-limiting --without-threading-support-check-during-build     \
     --prefix=/work/e89/e89/bp443/programs/elpa/install_dir/
   make -j8
   make install

Compilation
~~~~~~~~~~~

.. code:: bash

   module restore
   module load PrgEnv-gnu
   module load mkl

   EPATH=/work/e89/e89/bp443/programs/elpa/install_dir

   ftn  -O3 test_real2.F90 -lmpich  -I${EPATH}/include/elpa_openmp-2023.11.001.rc1/modules \
     -L${EPATH}/lib -lelpa_openmp  \
     -L${MKLROOT}/lib/intel64 -lmkl_scalapack_lp64 -Wl,--no-as-needed -lmkl_gf_lp64 -lmkl_gnu_thread -lmkl_core -lmkl_blacs_intelmpi_lp64 -lgomp -lpthread -lm -ldl \
       -o elpa_eigen.exe

I am not sure about this, it seems to be working so I dont wanna modify
it.

Run
~~~

.. code:: bash

   #!/bin/bash
   #SBATCH --time=0:20:00                           # Maximum time requested
   #SBATCH --nodes=1
   #SBATCH --ntasks-per-node=1
   #SBATCH --cpus-per-task=2
   #SBATCH --job-name  elpa
   #SBATCH --output    %j.out            # File to which STDOUT will be written
   #SBATCH --error     %j.err            # File to which STDERR will be written

   #SBATCH -A e89-camp
   #SBATCH --partition=standard
   #SBATCH --qos=short

   export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
    
   echo "##########################################################"
   echo "Number of CPU cores:" $SLURM_NPROCS
   echo "Number of OMP_NUM_THREADS:" $SLURM_CPUS_PER_TASK
   echo "List of machines:" $SLURM_NODELIST
   echo "Current folder:" `pwd`
   echo "##########################################################"
   echo "Job started: " `date`

   module restore
   module load PrgEnv-gnu
   module load mkl

   export MKL_ENABLE_INSTRUCTIONS=AVX2
   export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

   srun -c $SLURM_CPUS_PER_TASK ./elpa_eigen.exe
   echo "Job finished: " `date`
