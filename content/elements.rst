.. _elements:

Elements of the Program MTZTEM
==============================

This section provides a brief description of each program in the MTZTEM library. In addition, we describe the file formats for all input and supporting files used by the coding library.

Introduction
------------

The MTZTEM package makes use of the following executables:

        - **MT3Dfwd:** solves the magnetotelluric forward problem
        - **ZTEM_MT3Dinv:** solves the inverse problem for both magnetotelluric and/or Z-axis tipper data (or can solve the forward problem for multiple frequencies)
        - **blk3cell:** Creates models from a set of blocks on a tensor mesh


Main Input Files
----------------

Here, we describe the main input files for executables contained with the MTZTEM coding package.

.. toctree::
    :maxdepth: 2

    Create octree model <inputfiles/createModel>
    Forward modeling <inputfiles/forward>
    Inversion <inputfiles/inversion>

General Files for MTZTEM Executables
------------------------------------

Here, we describe the formats of supplementary files used to run MTZTEM executable files. The input files for each executable program are described in the :ref:`running the programs<running>` section.


.. toctree::
    :maxdepth: 1

    Survey/Locations File <files/locationsFile>
    Observations File <files/obsFile>
    Predicted Data (MT3Dfwd) <files/preFileFWD>
    Predicted Data (ZTEM_MT3Dinv) <files/preFileINV>
    Topography File <files/topoFile>
    Tensor Mesh File <files/tensor_mesh>
    Model File <files/model>
    Bounds File <files/bounds>
    Model and Face Weights Files <files/weights>












