Relaxation
===========

How to use
------------

1. Copy your POSCAR structure to the directory of the code.

In ``1_relax_csd3.py``:

2. Change ``input_vasp_file`` variable to point to your structure file.
3. Change the processor number under ``n_proc``.
4. Change the path to the ACE potential files under ``path_ACE_potential``.
5. Change the path to LAMMPS under ``path_lammps``.
6. Set your thresholds and number of relaxations to be run under ``thresholds`` and ``number_relaxations``. Decreasing thresholds are necessary as we consecutively perform a relax and vc-relax stage to converge to the minimal energy structure.

In ``launch_1_relax_csd3.ctrl``:

7. Change account name.
8. Change ``ntasks`` to the number you gave in step 3.
9. Change Python module version or source your virtual environment.
10. Change the path to your phonopy installation.

In CSD3 teminal:

11. Submit the job with ``sbatch``.

After job returning without errors:

12. Check convergence in ``struct_n_relax_n.out`` files:
   * Pressure is less than 1.
   * Stopping criterion is due to reaching energy tolerance.
13. The relaxed structure will be in ``relaxed_structure_starting_point.POSCAR``.

