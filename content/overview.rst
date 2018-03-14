.. _overview:

E3DMT package overview
======================

Description
-----------

This manual provides instruction and background for the e3dMT program library for the forward
modeling and inversion of magnetotelluric survey data. The e3dMT library is built in the same
manner as e3d, so much of the background presented in this manual is identical to the e3d manual (**link**).
In order to decrease computational time and increase accuracy by mesh refinement in areas of
interest, e3d models are discretized on an Octree mesh. 


.. figure:: images/OcTree.png
     :align: center
     :width: 700

     2D (QuadTree) mesh discretization about a ring (left). Cell refinement for OcTree mesh (right).


For ease of use the program library includes several utilities which generate a regular base mesh, enabling the user to construct initial
or simulation models on a regular mesh and then convert to an octree mesh. From the users point of
view the software operates in much the same way as previous GIF codes. This version is currently
run through the command line only.

The program library provides codes to do the following:

   1. Construct models on a rectangular mesh, where each cell is assigned a constant value of conductivity, and transfers the model to an octree mesh.

   2. Forward model electric and magnetic field anomaly responses to a 3D volume of contrasting conductivity, on and octree mesh.

   3. Convert from and octree mesh to a regular base mesh.

   4. Invert surface (MT), airborne (ZTEM) data to generate 3D conductivity models:
   
      - The inversion is solved as an optimization problem with the simultaneous goals of (i) minimizing an objective function dependent on the model and (ii) generating synthetic data that match observations to within a degree of misfit consistent with the statistics of those data.
      - To counteract the inherent lack of information, the formulation incorporates reference model and smoothing by regularization.
      - Capacity for the user to directionally weight smoothing and reference model influence as well as overall influence of regularization on objective function minimization. Explicit prior information may also take the form of upper and lower bounds on the conductivity contrast in any cell.
      - The regularization parameter (controlling relative importance of objective function and misfit terms).


The initial research underlying this program library was funded principally by the mineral industry consortium \Joint and Cooperative Inversion of Geophysical and Geological Data" (1991 -
1997) which was sponsored by NSERC and the following 11 companies: BHP Minerals, CRA Exploration, Cominco Exploration, Falconbridge, Hudson Bay Exploration and Development, INCO
Exploration & Technical Services, Kennecott Exploration Company, Newmont Gold Company,
Noranda Exploration, Placer Dome, and WMC.

Since then, improvements have been implemented as time and resources permit.

E3DMT Program Library Content
-----------------------------

The main executable programs within the E3DMT program library are:

    - **MTcreate_octree_mesh_e3d:** Creates the OcTree used in forward simulations and inversions from survey data.
    - **blk3cell:** Creates simple conductivity models on a core tensor mesh
    - **3DModel2Octree:** Converts 3D conductivity on core mesh to OcTree mesh
    - **e3dMTfwd:** Performs the forward simulation
    - **e3dMTinv:** Inverts observed data in order to recover a conductivity model

Also included are the following Octree utility programs:

      - create weight file
      - face weights
      - octree cell centre
      - octreeTo3D
      - refine octree
      - re-mesh octree model
      - surface electrodes

Licensing
---------

Licensing for commercial use is managed by distributors, not by the UBC-GIF research group.


Installing E3DMT
----------------

There is no automatic installer currently available for the e3dMT. Please follow the following steps in order to use the software:

   1. Extract all files provided from the given zip-based archive and place them all together in a new folder.
   2. Add this directory as new path to your environment variables.
   3. If you are running the software on a cluster of computers, please install the Message Pass Interface (MPI) on your computer and add it to your path in addition from
   4. Make sure to create a separate directory for each new inversion, where all the associated files will be stored. Do not store anything in the bin directory other than executable applications and Graphical User Interface applications (GUIs).






