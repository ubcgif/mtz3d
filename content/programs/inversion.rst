.. _mtztem_inv:

Inversion Program
=================

Both the forward and inverse problems are solved using the **mtz3d** executable program. In each case, format of the input file (denoted here as **mt3dinv.inp**) is the same. In the case of forward modeling however, some lines in the input file are omitted.

Running the Program
-------------------

A basic way of running inversion is done by opening a command line window and typing the path to the code **mtz3d.exe**, followed by a space, followed by the path to the input file named **mt3dinv.inp**.

.. figure:: images/mtztem_run_inv.png
    :align: center
    :width: 600


If MPI is installed, the *mpiexec* call can used to parallelize multiple processes (large-scale independent operations) within the code. To run the executable and utilize this functionality, open a command window and type the following:


.. figure:: images/mtztem_run_inv_mpi.png
    :align: center
    :width: 700


The call *mpiexec* is followed by the flag *-n*, then the number of processes (*"nFreq"* ) to be carried out simultaneously. This is followed by the paths to the executable and the corresponding input file, respectively. The number of simultaneous processes (*"nFreq"* ) **must** be equal or less than the number of frequencies. Ideally there is enough memory for *nFreq* to be equal to the number of frequencies.

.. important:: For forward modeling or inversion, the input file **must** be given the name **mt3dinv.inp**!!!

.. Setting Number of Threads with Open MPI
.. ---------------------------------------

.. Before running the executable, the number of threads used to carry out all simultaneous processes can be set with Open MPI. This is set in the command window **before** running the executable. To set the number of threads (*nThreads* ), use the following syntax:

..     - Windows computer: "set OMP_NUM_THREADS=nThreads"
..     - Linux (bash shell): "export OMP_NUM_THREADS=nThreads"
..     - Linux (csh shell): "setenv OMP_NUM_THREADS nThreads"

.. .. important:: The number of processes (*nFreq* ) times the number of threads (*nThreads* ) **cannot** exceed the total number of threads available from the computer.


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

The program **mtz3d.exe** creates the following output files:

    - **inv.con:** final conductivity model
    - **inv_xx.con:** recovered model at iteration *xx*
    - **dpredFWD.txt:**
    - **dpred_0.txt:** predicted data for each recovered conductivity model
    - **dpred_xx.txt:** predicted data for recovered model *xx*
    - **dpred.txt:** predicted data from final conductivity model
    - **mtz3d.log:** log file for the inversion
    - **mtz3d.out:** output file showing progress of the inversion






