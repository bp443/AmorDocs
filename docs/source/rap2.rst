RAP2 - Interatomic Force Constant calculation
=============================================

Latest code available on the `AMorFo project GitHub page <https://github.com/WignerTransport/AmorFo/tree/master>_`, under ``dynmat_lammps/dynmat_ACE_region``.
The main code is contained in ``ASE_lammps_multi.py``.

Description of the code
-----------------------

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

On the terminal:

10. Submit your job using the ``sbatch`` command. The part is under review for use on CSD3.

After finishing without errors:

11. Run the ``npz_hdf5.py`` script on the login node or on your computer to convert the sparse to the dense hdf5 formats.
12. Output is ``fc2.hdf5``.

Known issues
-------------

On CSD3, the ``multiprocessing`` and the ``lammps`` Python packages do not work together. In the future serial LAMMPS is probably going to be in use.


