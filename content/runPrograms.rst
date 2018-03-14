.. _running:

Running the programs
====================

The software package E3DMT contains the following Fortran executable codes:

- ``MTcreate_octree_mesh_e3d``: Creates OcTree mesh
- ``blk3cell``: Creates block model on tensor mesh
- ``3DModel2Octree``: Interpolates tensor model to Octree mesh
- ``e3dmtfwd``: Forward models synthetic MT and ZTEM data
- ``e3dmtinv``: Inverts MT and ZTEM data


.. note::

	All executable files, input files, output filenames and files specified within input files can be specified in the following manner:

	- as just the filename if contained within the current working directory (Example: *filename.txt*)
	- as a file path relative to the current working directory (Example: *sub_dir\\filename.txt*)
	- as the full path (Example: *C:\\Users\\Name\\Tests\\filename.txt*)

	Executable files should **not** be renamed. However, input file names can be specified by the user if desired.


To become proficient at using the E3DMT package, we suggest becoming familiar with programming in the following sections:

  .. toctree::
    :maxdepth: 1

    OcTree Mesh Generation <programs/createOcTree>
    Creating Octree Models <programs/createModel>
    Forward Modeling <programs/forward>
    Additional Cell and Face Weights <programs/weightsFiles>
    Inversion <programs/inversion>

