.. _running:

Running the programs
====================

This section provides describes how to run all executables pertaining to the MTZ3D package.

.. note::

    All executable files, input files, output filenames and files specified within input files can be specified in the following manner:

    - as just the filename if contained within the current working directory (Example: *filename.txt*)
    - as a file path relative to the current working directory (Example: *sub_dir\\filename.txt*)
    - as the full path (Example: *C:\\Users\\Name\\Tests\\filename.txt*)

    Executable files should **not** be renamed. However, input file names can be specified by the user if desired.


Executables
-----------

.. important:: Although described here, this generation of the code may not be supported by GIFtools in the future.

The program library provides the following codes:

   - **mtz3d.exe:** A code for forward modeling or inverting natural source EM data

Utility codes relevant to this package include:

   - **blk3cell.exe:** A utility for generating block models on tensor meshes


Contents
--------

To learn the specifics of running each executable, see the following sections:

  .. toctree::
    :maxdepth: 1

    Create Model <programs/createModel>
    Forward Modeling <programs/forward>
    Inversion <programs/inversion>

