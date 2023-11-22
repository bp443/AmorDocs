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

1. Copy ``fc2.hdf5`` to the folder.

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

12. ``module load intel``
13. ``ifort -O2 gather_new.f90 -o gather.exe``
14. ``./gather.exe``

15. ``ifort -O2 convert_freqdat.f90 -o convert.exe``
16. ``./convert.exe``

DoS calculation
------------------

Use the Python script ``calc_DOS.py`` in the ``AmorFo/harmonic_properties_ELPA/dynmat_diagonalisation/DOS_calculation`` folder. Change axis names as needed.






