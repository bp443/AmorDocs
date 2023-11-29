ID2 - Diagonalisation of dynamical matrix
==========================================

Multiple implementations are available. Implementations with ScaLAPACK and ELPA are using dense matrix diagonalisation, while future code will allow diagonalisation of sparse formats.

The :ref:`elpa` is available in the ``harmonic_properties_ELPA/dynmat_diagonalisation/diagonalisation_gamma`` folder, while the ScaLAPACK code is currently part of the PER3 code in the ``3_step_linewidth`` folder.

.. _elpa:

ELPA script
-----------------

Code is in folder ``AmorFo/harmonic_properties_ELPA/dynmat_diagonalisation
/diagonalisation_gamma/``.

Requirements
^^^^^^^^^^^^^^

* ELPA to be installed.

How to use
^^^^^^^^^^^

1. Copy ``fc2.hdf5`` and your POSCAR structure file to the folder.

2. Change the input filenames in ``1_fc2_to_true_dmat.py``.
3. Run ``1_fc2_to_true_dmat.py`` script ot generate text file from hdf5. To launch on cluster, use the ``launch_1.ctrl`` file to submit after appropriate modifications.

In ``test_real2.f90`` file:

2. Change na&nev to the number of atoms of current model
3. Change nblk: a number needed for diagonalisation. no_atoms must be divisible by (nblk)^2. nblk=1 is needed for small models.

In ``eigen.pbs``:

4. ``--ntasks-per-node`` change to nblk^2
5. Change necessary modules, account name

In ``compile.sh``:

6. Change the necessary modules, and path for elpa to work. ScaLAPACK might not exist in the ELPA library, so path needed to be added separately in this case.

In ``gather_new.f90``:

7. Change ``blocks`` to the value of ``nblk``
8. Change ``bsize`` to no_atoms*3/blocks

In ``convert_freqdat.f90``:

9. Change natom at line 4.

In the terminal:

9. ``module purge``
10. ``./compile.sh``
11. ``sbatch eigen.pbs``

Wait till the job finishes.

12. ``module load intel``
13. ``ifort -O2 gather_new.f90 -o gather.exe``
14. ``./gather.exe``

Convert frequencies to :math:`cm^{-1}`.

15. ``ifort -O2 convert_freqdat.f90 -o convert.exe``
16. ``./convert.exe``

Output is in ``frequencies_elpa_new.dat``.

DoS calculation
------------------

Use the Python script ``calc_DOS.py`` in the ``AmorFo/harmonic_properties_ELPA/dynmat_diagonalisation/DOS_calculation`` folder. Change axis names as needed.

Known issues
-------------

A bug has been fixed on 28/11/2023.




