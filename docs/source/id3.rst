ID3 - Anharmonic linewidth calculation
=======================================

For this calculation you need the code from **the ``ID3-test`` branch.** 

The he master branch is sufficient, but improvements have been added to this branch. In the future improved code will be merged to master branch.

The first part of the code is ID2, as the connection between the ELPA code and the linewidth calculation is being developed.

Requirements
-------------

* MKL package.

How to use
-----------

1. Copy POSCAR structure file, ``fc2.hdf5`` and ``fc3.hdf5`` to the working directory.
2. Modify calculation parameters in ``consts.h``, e.g. temperature.

2. Modify ``parse2d.py`` to point to the correct POSCAR and ``fc2.hdf5``.
3. Modify ``parse3d_nonzero.py`` to point to the correct POSCAR and ``fc3.hdf5``. 
4. In ``mkl.f90`` change the ``natom`` variable and the smass variable for the number of atoms and the masses of the species. Implementation of more than 2 species is being developed.
5. In ``gamma_step2.f90`` change the atomic masses if needed. Improvements are being developed.

For different filename than ``POSCAR`` you need to change the filename in most of the files.

6. Compile the codes using the ``COMPILE_ALL.sh`` script. Modify the commands if needed.

If you are using a SLURM system, you can submit the jobs with ``SUBMIT1.ctrl``, ``SUBMIT2.ctrl``, ``SUBMIT3.ctrl``
The workflow countained in ``1SUBMIT.ctrl``:

7. Convert fc2 to text format with ``parse2d.py`` script. Ouput is ``mat2d.dat``.
8. Convert fc3 to sparse matrix in text format with ``parse3d_nonzero.py`` script. Ouput is ``mat3d_nonzero.dat``.
9. Diagonalise the dynamical matrix with ``eigen.ex``.
10. Perfom the first step of the linewidth calculation by executing ``gamma_step1.ex``.

Contained in ``2SUBMIT.ctrl``:

11. Perfom the second step of the linewidth calculation by executing ``gamma_step2.exe``

Contained in ``3SUBMIT.ctrl``:

13. Perform the third step of the linewidth calculation by executing ``gamma_step3.ex``.

Outputs are:

* ``mat2d.dat`` - fc2 in text format.
* ``mat3d_nonzero.dat`` - fc3 in sparse text format.
* ``frequencies.dat`` - Frequencies from diagonalisation
* ``eigenmodes.bin`` - Eigenmodes
* ``gamma_TK_new.dat`` - Linewidths (maybe a factor of 2 is needed)

Differences in ``master`` branch
---------------------------------------

Slight difference is that ``parse3d.py`` and ``d3.f90`` needs to be run to convert to dense matrix text file, then sparsify this. It takes ocnsiderable storage space and a bit of time, so these both are changed to ``parse3d_nonzero.py`` in the ``ID3-test`` branch. Removing older versions of ``mat2d`` and ``mat3d`` is necessary to have a correctly running code in this version. 


Known issues
-------------

A bug has been fixed on 28/11/2023. An extra do loop was removed.
