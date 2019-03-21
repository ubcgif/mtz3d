.. _running:

Running the programs
====================

This section provides describes how to run all executables pertaining to the MTZTEM package.

.. note::

    All executable files, input files, output filenames and files specified within input files can be specified in the following manner:

    - as just the filename if contained within the current working directory (Example: *filename.txt*)
    - as a file path relative to the current working directory (Example: *sub_dir\\filename.txt*)
    - as the full path (Example: *C:\\Users\\Name\\Tests\\filename.txt*)

    Executable files should **not** be renamed. However, input file names can be specified by the user if desired.


Executables
-----------

.. important:: Although described here, this generation of the code may not be supported by GIFtools in the future.

The MTZTEM coding package make use of the following Fortran executables:

    - **ZTEM_MT3Dinv.exe:** A parallelizable inversion code for inverting MT and/or ZTEM data at a multitude of frequencies
    - **MT3Dfwd.exe:** A forward modeling code for predicting MT data (no ZTEM) at a single frequency
    - **blk3cell.exe:** A utility for generating block models on tensor meshes


Contents
--------

To learn the specifics of running each executable, see the following sections:

  .. toctree::
    :maxdepth: 1

    Create Model <programs/createModel>
    Forward Modeling <programs/forward>
    Inversion <programs/inversion>

