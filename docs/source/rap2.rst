RAP2 - Interatomic Force Constant calculation
=============================================

Latest code available on the `AMorFo project GitHub page <https://github.com/WignerTransport/AmorFo/tree/master>_`, under ``dynmat_lammps/dynmat_ACE_region``.
The main code is contained in ``ASE_lammps_multi.py``.

Description
------------

The ``RAP2_MPI_bcast.py`` Python code provided reads a relaxed POSCAR file as input. Using the ``ase.calculators.LAMMPSlib`` object, which imports the ``lammps`` package. It computes the 2nd order force constant matrix of the given structure. The code utilises multiple CPUs using the ``mpi4py``package. The output is placed in the Results folder from each separate process, which can then be collected to a hdf5 file in dense matrix format, applying the acoustic sum rule for translation with minimising the square sum of the correction matrix.

Requirements
--------------

* Lammps to be installed, along with the ``lammps`` Python package.
* The `PACE <https://docs.lammps.org/Packages_details.html#pkg-ml-pace>_` package for LAMMPS installed for the ACE potential. Alternatively QUIP installed for GAP potential.
* ``ase`` Python pakcage to be installed in Python.
* ``mpi4py``, ``h5py`` Python packages.

To use the code
----------------------

1. Copy relaxed POSCAR into folder.

To use the Regiona Acceleration Protocol (RAP), in ``calc_NL.py`` configure the variables identically to ``RAP2_MPI_bcast.py``. ``FC2_cutoff`` determines the radius within the forces are saved to the FC2 matrix. ``Force_cutoff`` determines the force range, so the atoms to be included in RAP are within radius of ``FC2_cutoff``+``Force_cutoff``.

In ``RAP2_MPI_bcast.py``\:
 
2. Change ``POSCAR_file`` variable to the name of the input POSCAR.
3. Change the path to potential you are using.
4. Set the type of potential under the ``potential`` variable.
5. Change the input parameter of the protocol under ``FC2_cutoff``, ``Force_cutoff`` and ``displacement`` variables.
6. Set ``WRITE_OUT_TYPE``, currently only file-per-process is supported, collective file write is under developement.
7. If required set checkpointing: how many times the code should save partial results. Restart is not implemented yet.
8. Change output folder if required.

In the submit file of your cluster:

8. Change the account name
9. Change the number of tasks to the number you set in step 6.
10. (Optional) Change modules to be loaded if necessary. Source your Python virtual environment where the ``lammps`` library is installed if it is not installed to the user.

In the terminal:

11. Submit your job using the ``sbatch`` command. The part is under review for use on CSD3.

After finishing without errors, collect the output files:

12. Change POSCAR name if needed in ``RAP2_collect.py``, and change the sum rule if needed. Currently, square sum minimisation of the translational sum rule is implemented.
13. Run the ``RAP2_collect.py`` script to collect the partial results to the ``fc2.hdf5`` file containing a dense matrix under the "fc2" dataset.

OLD VERSION DOCUMENTATION
==========================

Description of the ``multiprocessing`` code
------------------------------------------

The ``ASE_lammps_multi.py`` Python code provided reads a relaxed POSCAR file as input. Using the ``ase.calculators.LAMMPSlib`` object, which imports the ``lammps`` package. It computes the 2nd order force constant matrix of the given structure. The code utilises multiple CPUs using the ``multiprocessing`` Python package. The output ``fc2.npz`` file containing a sparse matrix then converted to the general hdf5 extension in dense matrix format with the ``npz_hdf5.py`` script, which also checks the sum rule.

The GitHub folder contains submit files for the sulis and CSD3 clusters

Requirements
--------------

* Lammps to be installed, along with the ``lammps`` Python package.
* The `PACE <https://docs.lammps.org/Packages_details.html#pkg-ml-pace>_` package for LAMMPS installed for the ACE potential. Alternatively QUIP installed for GAP potential.
* ``ase`` Python pakcage to be installed in Python.

To use the code
----------------------

1. Copy relaxed POSCAR into folder.

In ``ASE_lammps_multi.py``\:
 
2. Change ``POSCAR_file`` variable to the name of the input POSCAR.
3. Change the path to potential you are using.
4. Set the type of potential under the ``potential`` variable.
5. Change the input parameter of the protocol under ``ASE_r_cut`` and ``displacement`` variables.
6. Change the number of tasks you want to use in the ``no_process`` variable.

In the submit file of your cluster:

7. Change the account name
8. Change the number of tasks to the number you set in step 6.
9. (Optional) Change modules to be loaded if necessary. Source your Python virtual environment where the ``lammps`` library is installed if it is not installed to the user.

In the terminal:

10. Submit your job using the ``sbatch`` command. The part is under review for use on CSD3.

After finishing without errors, convert sparse output to dense hdf5:

10. Change POSCAR name if needed in ``npz_hdf5.py``.
11. Run the ``npz_hdf5.py`` script on the login node or on your computer to convert the sparse to the dense hdf5 formats.

Output is ``fc2.hdf5``.

Known issues
-------------

On CSD3, the ``multiprocessing`` and the ``lammps`` Python packages do not work together. In the future serial LAMMPS is probably going to be in use.


