RAP3 - Anharmonic Force Constant calculation
=============================================

Files found in the ``AmorFo/anharmonic_linewidths
/`` folder in th AmorFo GitHub

How to use
-----------

1. Copy POSCAR structure file to the folder.

In ``Anharmonic.py``:

2. Change ``POSCAR_file`` variable to the name of your file.
3. Change the path to your potential, add a new one if you need.
4. Change potential to be used in ``potential`` variable.
5. Change algorithm parameters: ``ACE_r_cut``, `anharmonic_r_cut``, `displacement``.
6. Change ``no_process`` to the number of processors you want to use.

In ``submit``:

7. Change account
8. Change ``--ntasks`` to the number you use for ``no_process`` in step 6.
9. Change memory to your system default.
10. Load the necessary modules and source your python virtual environment if needed.

Output is a sparse matrix in ``fc3.dat``. To convert to dense hdf5:

11. Change POSCAR name in ``convert_dat_hdf5.py``.
12. Run Python script ``convert_dat_hdf5.py``.

Output is ``fc3.hdf5``.
