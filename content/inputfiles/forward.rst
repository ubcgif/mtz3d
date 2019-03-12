.. _mtztem_input_fwd:

Forward Modeling Input File
===========================


The forward problem is solved using the executable program **MT3Dfwd.exe**. Parameters necessary for running the forward modeling code are set in the input file. The lines of input file are as follows:

.. tabularcolumns:: |L|C|C|

+--------+------------------------------------------------------+-----------------------------------------------+
| Line # | Parameter                                            | Description                                   |
+========+======================================================+===============================================+
|   1    |:ref:`Frequency<mtztem_input_fwd_ln1>`                | Frequency being modeled in Hz                 |
+--------+------------------------------------------------------+-----------------------------------------------+
|   2    |:ref:`Tensor mesh<mtztem_input_fwd_ln2>`              | path to the tensor mesh file                  |
+--------+------------------------------------------------------+-----------------------------------------------+
|   3    |:ref:`Conductivity model<mtztem_input_fwd_ln3>`       | path to conductivity model                    |
+--------+------------------------------------------------------+-----------------------------------------------+
|   4    |:ref:`Susceptibility model<mtztem_input_fwd_ln4>`     | path to susceptibility model (optional)       |
+--------+------------------------------------------------------+-----------------------------------------------+
|   5    |:ref:`Locations<mtztem_input_fwd_ln5>`                | path to locations file                        |
+--------+------------------------------------------------------+-----------------------------------------------+
|   6    |:ref:`droptol tol max_iter<mtztem_input_fwd_ln6>`     | set iterative solver tolerances               |
+--------+------------------------------------------------------+-----------------------------------------------+
|   7    |:ref:`Preconditionner type<mtztem_input_fwd_ln7>`     | set iterative solver type                     |
+--------+------------------------------------------------------+-----------------------------------------------+
|   8    |:ref:`Fields options 1<mtztem_input_fwd_ln8>`         | compute full forward modeling options         |
+--------+------------------------------------------------------+-----------------------------------------------+
|   9    |:ref:`Fields options 2<mtztem_input_fwd_ln9>`         | outputs fields on mesh                        |
+--------+------------------------------------------------------+-----------------------------------------------+
|   10   |:ref:`Data options<mtztem_input_fwd_ln10>`            | output options for fields and data            |
+--------+------------------------------------------------------+-----------------------------------------------+




.. figure:: images/create_fwd_input.png
     :align: center
     :width: 700

     Example input file for forward modeling program (`Download <https://github.com/ubcgif/mtztem/raw/master/assets/input_files1/mtztem_octree_fwd.inp>`__ ).


Line Descriptions
^^^^^^^^^^^^^^^^^

.. _mtztem_input_fwd_ln1:

    - **Frequency:** the frequency (in Hz) at which the fields and MT data are modeling

.. _mtztem_input_fwd_ln2:

    - **Tensor Mesh:** file path to the :ref:`tensor mesh file <tensorFile>`

.. _mtztem_input_fwd_ln3:

    - **Conductivity Model:** file path to the conductivity :ref:`model file <modelFile>1.

.. _mtztem_input_fwd_ln4:

    - **Background Susceptibility Model:** The user can choose from several options with regarding the use of a background susceptibility model

        - For no background susceptibility, the flag *null* is used.
        - If a constant background susceptibility is being used for cells below the topography, enter the value this line. 
        - The user may also provide the file path to a background susceptibility :ref:`model file <modelFile>`. 

.. _mtztem_input_fwd_ln5:

    - **Receiver Locations:** file path to the :ref:`locations file<surveyFile>`.

.. _mtztem_input_fwd_ln6:

    - **Solver parameters:**
        - **droptol:** sets the threshold for dropping small term in the ILU factorization
        - **tol:** sets tolerance for convergence
        - **max_iter:** sets maximum number of iterations to find convergence

.. _mtztem_input_fwd_ln7:

    - **Preconditioner Type:** This is specified using a flag value of *0* or *4*.

        - If *0* is entered, a `symmetric successive over-relaxation <https://en.wikipedia.org/wiki/Symmetric_successive_over-relaxation>`__ (SSOR) preconditioner is used.
        - If *4* is entered, a BLUGS preconditioner is used. In general the SSOR preconditioner uses less memory, but converges slower (is recommended for older computers and large problems). The Blugs provides faster convergence, but uses more memory.

.. _mtztem_input_fwd_ln8:

    - **Fields Options 1:** This line indicates whether a complete forward modeling is performed, or whether field values have been previously computed and only impedances and/or apparent resistivities and phases need to be computed.
        
        - If *1* is entered, the program computes the E and H fields everywhere.
        - The user enters *0* followed by a set of 4 EDI filenames separated by spaces if the fields have been previously computed; example "*1 e1.dat e2.dat h1.dat h2.dat*". The 1 and 2 indicate that fields were computed using different polarizations of the source field.

.. _mtztem_input_fwd_ln9:

    - **Fields Options 2:** This flag determines if the E and H fields computed on the mesh are output.

        - If *0* is entered, the program does not output files containing the fields.
        - If *1* is entered, 4 files are output (*e1.dat, e2.dat, h1.dat* and *h2.dat*).

.. _mtztem_input_fwd_ln10:

    - **Data Options:** This flag determines whether the program outputs the data at the observation locations.

        - If *0* is entered, the program does not output fields or data at the observation locations
        - If *1* is entered, the program outputs the E and H fields at the observation locations for both polarizations (*MT_fields.txt*), the real and imaginary components of the elements of the impedance tensor (*MT_impedance_ri.txt*) and the apparent resistivities and phases (*MT_impedance_rho_ph.txt*)




