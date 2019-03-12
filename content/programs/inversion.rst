.. _mtztem_inv:

Inversion Program
=================

The inversion of MT and/or ZTEM data at multiple frequencies is done using the program **ZTEM_MT3Dinv.exe**. Parameters for the inversion are set in the file **mt3dinv.inp**.

Running the Program
-------------------

A basic way of running inversion is done by opening a command line window and typing the path to the code **ZTEM_MT3Dinv.exe**, followed by a space, followed by the path to the input file named **mt3dinv.inp**.

.. figure:: images/mtztem_run_inv.png
     :align: center
     :width: 600

For multiple frequencies, the *mpiexec* call can be used for parallelization. In this case, we type in order: *mpiexec*, the flag *-n*, the number of frequencies (*"nFreq"*), the path to the inversion executable and the corresponding input file.


.. figure:: images/mtztem_run_inv_mpi.png
     :align: center
     :width: 700


Units:
------

**Input:**

    - **MT data:** Real and imaginary components of impedance tensor entries (V/A)
    - **MT data:** Apparent resistivity and phase corresponding to impedance tensor entries. Units :math:`\Omega m` and :math:`\phi \in [-180^o, 180^o]` respectively
    - **ZTEM data:** Real and imaginary components of transfer function entries (unitless)
    - **Reference/starting conductivity model:** S/m 
    - **Background susceptibility model:** SI
    - **Sensitivity weights:** unitless


.. important:: The current version of the code requires both components for all entries within the impedance tensor. For example, the user cannot invert only the off-diagonal impedance tensor data. Instead the user must supply large uncertainties for the diagonal data.

**Output:**

    - **Conductivity model:** S/m

Output Files
------------

The program **mtzteminv.exe** creates the following output files:

    - **inv.con:** recovered conductivity models

    - **dpred.txt** predicted data for each recovered conductivity model

    - **mtztem_octree_inv.log:** log file for the inversion

    - **mtztem_octree_inv.out:**






