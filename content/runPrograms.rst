.. _running:

Running the programs
====================

The software package MTZTEM contains the following Fortran executable codes:

- ``MT3Dfwd``: Forward models synthetic MT data at a single frequency
- ``ZTEM_MT3Dinv``: Inverts MT and/or ZTEM data


.. note::

	All executable files, input files, output filenames and files specified within input files can be specified in the following manner:

	- as just the filename if contained within the current working directory (Example: *filename.txt*)
	- as a file path relative to the current working directory (Example: *sub_dir\\filename.txt*)
	- as the full path (Example: *C:\\Users\\Name\\Tests\\filename.txt*)

	Executable files should **not** be renamed. However, input file names can be specified by the user if desired.


To become proficient at using the MTZTEM package, we suggest becoming familiar with programming in the following sections:

  .. toctree::
    :maxdepth: 1

    Forward Modeling <programs/forward>
    Inversion <programs/inversion>

