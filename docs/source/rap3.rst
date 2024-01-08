RAP3 - Anharmonic Force Constant calculation
=============================================

Files found in the ``AmorFo/anharmonic_linewidths
/RAP3_MPI/`` folder in th AmorFo GitHub

How to use the code
--------------------

1. Copy the relaxed POSCAR to the folder.

To use the Regional Acceleration Protocol (RAP), in ``calc_NL_compList.py`` configure the variables identically to ``RAP3_MPI.py``. ``FC3_cutoff`` determines the radius within the forces are saved to the FC3 matrix. ``Force_cutoff`` determines the force range, so the atoms to be included in RAP are within radius of ``FC3_cutoff`` + ``Force_cutoff``. The script also calculates the pairs to compute the FC3 for.

2. Run the ``calc_NL_compList.py`` script.

In ``RAP3_MPI.py``\:
 
2. Change ``POSCAR_file`` variable to the name of the input POSCAR.
3. Change the path to potential you are using.
4. Set the type of potential under the ``potential`` variable.
5. Change the input parameter of the protocol under ``FC3_cutoff``, ``Force_cutoff`` and ``displacement`` variables.
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

12. Change POSCAR name if needed in ``RAP3_collect.py``, and change the sum rule if needed. Square sum minimisation of the translational sum rule is being implemented in the near future, a very simple rule is implemented instead at the moment.
13. Run the ``RAP3_collect.py`` script to collect the partial results to the ``fc3.hdf5`` file containing a COO sparse matrix under the "fc3" dataset, in a $$M\times 7$$ float64 matrix with the first 6 columns being the coordinates in $$i,\alpha,j,\beta,k,\gamma$$ order.


OLD VERSION
==========

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
