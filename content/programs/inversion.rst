.. _mtztem_inv:

Inversion Program
=================

Both the forward and inverse problems are solved using the **ZTEM_MT3Dinv** executable program. In each case, format of the input file (denoted here as **mt3dinv.inp**) is the same. In the case of forward modeling however, some lines in the input file are omitted.

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

.. important:: For forward modeling or inversion, the input file **must** be given the name **mt3dinv.inp**!!!


Units:
------

**Input and outputs:**

    - **Impedance data:** Real and imaginary components of impedance tensor entries (V/A)
    - **Apparent resistivity data:** Apparent resistivity and phase corresponding to impedance tensor entries. Units :math:`\Omega m` and :math:`\phi \in [-180^o, 180^o]` respectively
    - **ZTEM data:** Real and imaginary components of transfer function entries (unitless)
    - **conductivity models:** S/m 
    - **Background susceptibility model:** SI
    - **Sensitivity weights:** unitless

Output Files
------------

The program **ZTEM_MT3Dinv.exe** creates the following output files:

    - **inv.con:** final conductivity model
    - **inv_xx.con:** recovered model at iteration *xx*
    - **dpredFWD.txt:**
    - **dpred_0.txt:** predicted data for each recovered conductivity model
    - **dpred_xx.txt:** predicted data for recovered model *xx*
    - **dpred.txt:** predicted data from final conductivity model
    - **mt3dinv.log:** log file for the inversion
    - **mt3dinv.out:** output file showing progress of the inversion






