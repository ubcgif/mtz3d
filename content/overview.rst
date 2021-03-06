.. _overview:

MTZ3D package overview
======================

Description
-----------

The MTZ3D package is primarily designed to recover conductivity models on 3D tensor meshes through the inversion of magnetotelluric (MT) and/or Z-axis tipper EM (ZTEM) data. If necessary, this package can also be used to forward model both MT and ZTEM data. The data to be inverted can be either the elements of the impedance tensor, the associated apparent resistivities and phases, or elements of magnetivariational transfer functions (Tippers). The inversion program can be run from the command line under LINUX, or MS-Windows or, in the MS Windows environment, using a graphical user interface (GUI). The UBC-GIF utility MeshTools3D is used to examine resulting 3D conductivity models.

As of 2010, the code has been updated to work with transfer function data, along with impedance MT data and furthermore, the code has been parallelized with open MP and MPI. MPI parallelization was used for variety of frequency data and will be discussed below in greater detail, while open MP parallelization is used for two orthogonal source polarization directions for each frequency. Hence, for the frequency-based parallelization, the number of processors defined under MPI settings can not exceed the number of frequencies described in the data file. Each of these frequency-oriented processes will be further split up in two source polarization threads using the open MP environment.

Experienced users of inversion understand that fine tuning the parameters concerned with computational accuracy can affect the efficiency of convergence. In principle, one wants to compute all quantities as accurately as possible and solve the matrix systems exactly. Unfortunately that can lead to prohibitively large computational costs and so strategies that reduce the computations, and yet do not compromise the final model, are sought. For this reason there are two levels in which MTZ3D can be run. The first uses all default parameters. In the second, the user can adjust tolerance, maximum number of iterations, etc. to gain computational efficiency. In order to adjust these parameters in a meaningful way, the user needs to understand the basic structure of the code and the parameters that control the calculations. Therefore it is important to read the overview of background theory and to use the manual that follows to understand exactly what each parameter does.

The initial research underlying this program library was funded principally by the “IMAGE” consortium, of which the following companies were participants: AGIP, Anglo American, Billiton, Cominco, Falconbridge, INCO, MIM, Muskox Minerals, Newmont, Placer Dome and Rio Tinto, and from the Natural Sciences and Engineering Research Council of Canada (NSERC).

Since then, improvements have been implemented as time and resources permit.


MTZ3D Program Library Content
-----------------------------

The program library provides the following codes:

   - **mtz3d.exe:** A code for forward modeling or inverting natural source EM data

Utility codes relevant to this package include:

   - **blk3cell.exe:** A utility for generating block models on tensor meshes


Licensing
---------

Licensing for commercial use is managed by distributors, not by the UBC-GIF research group.


Installing MTZ3D
-----------------

MTZ3D Executables
^^^^^^^^^^^^^^^^^

There is no automatic installer currently available for the MTZ3D. Please follow the following steps in order to use the software:

   1. Extract all files provided from the given zip-based archive and place them all together in a new folder.
   2. Add this directory as new path to your environment variables.
   3. If you are running the software on a cluster of computers, please install the Message Pass Interface (MPI) on your computer and add it to your path in addition from

MPI Executables
^^^^^^^^^^^^^^^

Message passaging interface (MPI) programming allows MTZ3D executables to utilize parallel computing. To set up MPI:

    1. Download and install:
    	
    	- `Microsoft MPI v10.0 <https://www.microsoft.com/en-us/download/details.aspx?id=57467>`__ : Required for window machines
    	- `MPICH <https://www.mpich.org/downloads/>`__ : Required for Linux machines
    	- `Open MPI v4 <https://www.open-mpi.org/software/ompi/v4.0/>`__ : Optional programming to set MPI threads

    2. Path the folders containing MPI executables to your environment variables.




