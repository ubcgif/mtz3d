.. _elements:

Elements of the Program E3DMT
=============================

Introduction
------------

This section provides a description of each program in the E3DMT library.

	1. E3DMT Executable programs:
		- MTcreate_octree_mesh_e3d: creates an octree mesh around the receiver array
		- e3dMTfwd: Solves the forward problem. Computes the electric and magnetic response to a 3D conductivity model (fields, and impedance)
		- e3dMTinv: Solves the inverse problem using a direct solver approach. Recovers a conductivity model by inverting MT and/or ZTEM data.
		- e3dMTinv_iter: Solves the inverse problem using an iterative solver approach. Recovers a conductivity model by inverting MT and/or ZTEM data.

	2. Octree utilities:
		- blk3cell: Creates models from a set of blocks
		- 3DModel2Octree: Converts models from tensor to Octree meshes
		- interface_weights: Creates interface weights


General Files for E3DMT Executables
-----------------------------------

Here, we describe the formats of supplementary files used to run E3DMT executable files. The input files for each executable program are described in the :ref:`running the programs<running>` section.


.. toctree::
    :maxdepth: 1

    Survey/Locations File <files/surveyFile>
    Predicted Data File <files/preFile>
    Observations File <files/obsFile>
    Topography File <files/topoFile>
    Tensor Mesh File <files/tensor_mesh>
    Octree Mesh File <files/octree_mesh>
    Model File <files/model>
    Model and Face Weights Files <files/weights>












