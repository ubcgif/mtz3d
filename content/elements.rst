.. _elements:

Elements of the Program MTZ3D
=============================

This section provides a brief description of each program in the MTZ3D library. In addition, we describe the file formats for all input and supporting files used by the coding library.

Introduction
------------

The MTZ3D package makes use of the following executables:

        - **ZTEM_MT3Dinv:** A code for forward modeling or inverting natural source EM data
        - **blk3cell:** Creates models from a set of blocks on a tensor mesh


Main Input Files
----------------

Here, we describe the main input files for executables contained with the MTZ3D coding package.

.. toctree::
    :maxdepth: 2

    Create octree model <inputfiles/createModel>
    Forward modeling <inputfiles/forward>
    Inversion <inputfiles/inversion>

General Files for MTZ3D Executables
------------------------------------

Here, we describe the formats of supplementary files used to run MTZ3D executable files. The input files for each executable program are described in the :ref:`running the programs<running>` section.


.. toctree::
    :maxdepth: 1

    Observations/Locations File <files/obsFile>
    Predicted Data (ZTEM_MT3Dinv) <files/preFileINV>
    Topography File <files/topoFile>
    Tensor Mesh File <files/tensor_mesh>
    Model File <files/model>
    Bounds File <files/bounds>
    Model and Face Weights Files <files/weights>












