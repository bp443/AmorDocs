Velocity Operator Calculation
=======================================

To calculate the velocity operator, you can use the in-house code which is based on BLAS. On AMD processors (Archer2, Sulis, Kelvin2) use `OpenBLAS` (or `Cray-LibSci` for Archer2), and on Intel CPUs, use MKL.

Requirements
-------------

Working BLAS library (e.g. OpenBLAS, MKL, Cray-LibSci)

How to use
-----------


1. Copy fc2,hdf5, dmat.dat (rename it dmat_true.dat) , POSCAR_relaxed and rename it POSCAR, frequencies.dat and rename it frequencies_true.dat, eigenmodes.bin and rename it eigen_true.bin to working directory

2. （if you used code from Balazs’ code to get frequencies and eigens, this step is not necessary, but if you used elpa which manually added modes to make things divisible by 64 please message me again) Sbatch lauch_2 to get true frequencies(frequencies.dat) and eigenmodes(eigen_all.bin)


3. Sbatch launch_3 to get spacings between atoms (output svecs)

4. To get the velocity operators
sbatch bash.sh
(Output v_op)

5. Sbatch launch_0a to transform velocity operators to a form useful for conductivity calculations (output split_vel_op)


Please contact the developer for more information.
