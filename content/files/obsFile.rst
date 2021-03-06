.. _obsFile:

Observations/Locations File
===========================

The locations file contains all necessary survey information for forward modeling and the observations file contains all necessary survey information for running the inversion. This information includes the data type, observation locations, frequencies, data and uncertainties. 

.. note::
    - Bolded entries are fixed flags recognized by the Fortran codes and blue hyperlinked entries are values/regular expressions specified by the user


MT or ZTEM data
---------------

If only MT, ZTEM or apparent resistivity and phase data are being inverted, the observation file is formatted as follows:

| **DATATYPE** :math:`\;` :ref:`A<e3dmt_obs_ln1>`
| **IGNORE** :math:`\;` :ref:`B<e3dmt_obs_ln2>`
|
| :ref:`C<e3dmt_obs_ln3>`
| :ref:`D<e3dmt_obs_ln4>`
| :ref:`Data Array<e3dmt_obs_ln5>`
|
| :ref:`C<e3dmt_obs_ln3>`
| :ref:`D<e3dmt_obs_ln4>`
| :ref:`Data Array<e3dmt_obs_ln5>`
|
| :math:`\;\;\;\;\;\;\;\; \vdots`
|
| :ref:`C<e3dmt_obs_ln3>`
| :ref:`D<e3dmt_obs_ln4>`
| :ref:`Data Array<e3dmt_obs_ln5>`
|
|


.. figure:: images/files_data.png
     :align: center
     :width: 700

     Example data file for MTZ data.


Joint MT and ZTEM data
----------------------

If joint MT and ZTEM data are being inverted, the observation file is formatted as follows:

| **DATATYPE** :math:`\;` :ref:`A<e3dmt_obs_ln1>`
| **IGNORE** :math:`\;` :ref:`B<e3dmt_obs_ln2>`
|
| :ref:`Cmt<e3dmt_obs_ln3>`
| :ref:`Dmt<e3dmt_obs_ln4>`
| :ref:`MT Data Array<e3dmt_obs_ln5>`
|
| :ref:`Cztem<e3dmt_obs_ln3>`
| :ref:`Dztem<e3dmt_obs_ln4>`
| :ref:`ZTEM Data Array<e3dmt_obs_ln5>`
|
| :ref:`Cmt<e3dmt_obs_ln3>`
| :ref:`Dmt<e3dmt_obs_ln4>`
| :ref:`MT Data Array<e3dmt_obs_ln5>`
|
| :ref:`Cztem<e3dmt_obs_ln3>`
| :ref:`Dztem<e3dmt_obs_ln4>`
| :ref:`ZTEM Data Array<e3dmt_obs_ln5>`
|
| :math:`\;\;\;\;\;\;\;\; \vdots`
|
| :ref:`Cmt<e3dmt_obs_ln3>`
| :ref:`Dmt<e3dmt_obs_ln4>`
| :ref:`MT Data Array<e3dmt_obs_ln5>`
|
| :ref:`Cztem<e3dmt_obs_ln3>`
| :ref:`Dztem<e3dmt_obs_ln4>`
| :ref:`ZTEM Data Array<e3dmt_obs_ln5>`
|
|

.. note:: Frequencies for MT and subsequent ZTEM data do not need to match. However, the number of MT data blocks and ZTEM data blocks must be equal.


Parameter Descriptions
----------------------


.. _e3dmt_obs_ln1:

    - **(A) Data type:**. The type of data being forward modeled is specified at the beginning of the file. Example: *DATATYPE MTZ*. There are 4 options for DATATYPE:

        - "MTZ" - MT data (both real and imaginary impedance tensor data)
        - "MTR" - MT data (expressed in terms of apparent resistivity and phase)
        - "MTT" - ZTEM data (both real and imaginary z-tipper data)
        - "MTB" - A joint dataset combining datatypes MTZ and MTT

.. _e3dmt_obs_ln2:

    - **(B) Flag to ignore data entries:** A regular expression is entered, signifying data in the data structure which is ignored during the inversion. Example: *!IGNORE -0*
        
.. _e3dmt_obs_ln3:

    - **(C) Frequency:** Frequency at which the corresponding set of field observations are made. Example: *1.0000E+002*.

.. _e3dmt_obs_ln4:

    - **(D) Number of receivers:** Number of receivers collecting data at the aforementioned frequency for the aforementioned data type. Example: *900*.

.. _e3dmt_obs_ln5:

    - **Data Array:** Contains the locations and field observations for the data specified by :ref:`data type<e3dmt_obs_ln3>`. The number of lines in this array is equal to the number of receivers. The number of columns depends on the type of data specified. The columns for defined for each array are show :ref:`below<obsFile_data>`.


.. _obsFile_data:

Data Arrays by Type
-------------------

Locations
^^^^^^^^^

If forward modeling is being performed, only the (x,y,z) locations are required in each data array, regardless of the data type (MTZ, MTR, MTT, MTB). The data array in this case is N X 3 and each row is formatted as follows:

.. math::
    | \; x \; | \; y \; | \; z \; |

where :math:`x` is Easting, :math:`y` is Northing and :math:`z` is elevation.


MT impedance data (DATATYPE = MTZ):
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Each row in the array contains the elements of the impedance tensor at a particular location separated into real and imaginary components, along with the corresponding uncertainties. The units for MT data are (V/A). The columns for this data format are as follows:

.. math::
    | \; x \; | \; y \; | \; z \; | \;\;\; Z_{xx} \; data \;\;\; | \;\;\; Z_{xy} \; data \;\;\; | \;\;\; Z_{yx} \; data \;\;\; | \;\;\; Z_{yy} \; data \;\;\; |

such that each :math:`Z_{ij} \; data` is comprised of 4 columns:

.. math::

    | \; Z^\prime_{ij} \; | \; U^\prime_{ij} \; | \; Z^{\prime \prime}_{ij} \; | \; U^{\prime \prime}_{ij} \; |

where

    - :math:`Z^\prime_{ij}` is the real component of entry i,j of the impedance tensor
    - :math:`Z^{\prime\prime}_{ij}` is the imaginary component of entry i,j of the impedance tensor
    - :math:`U^\prime_{ij}` is the uncertainty on :math:`Z^\prime_{ij}`
    - :math:`U^{\prime\prime}_{ij}` is the uncertainty on :math:`Z^{\prime\prime}_{ij}`

MT apparent resistivity and phase data (DATATYPE = MTR):
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Each row in the array contains the elements of the impedance tensor at a particular location separated into apparent resistivity and phase, along with the corresponding uncertainties. The units for apparent resistivity are Ohms and those for apparent resistivity are degrees. The columns for this data format are as follows:

.. math::
    | \; x \; | \; y \; | \; z \; | \;\;\; Z_{xx} \; data \;\;\; | \;\;\; Z_{xy} \; data \;\;\; | \;\;\; Z_{yx} \; data \;\;\; | \;\;\; Z_{yy} \; data \;\;\; |

such that each :math:`Z_{ij} \; data` is comprised of 4 columns:

.. math::

    | \; \rho_{ij} \; | \; \rho_{ij} \; Unc. \; | \; \phi_{ij} \; | \; \phi_{ij} \; Unc. \; |

where

    - :math:`\rho_{ij}` is the apparent resistivity of entry i,j of the impedance tensor
    - :math:`\rho_{ij} \; Unc.` is the uncertainty on :math:`\rho_{ij}`
    - :math:`\phi_{ij}` is phase for entry i,j of the impedance tensor
    - :math:`\phi_{ij} \; Unc.` is the uncertainty on :math:`\phi_{ij}`



ZTEM data (DATATYPE = MTT):
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Each row in the array contains the elements of the transfer function at a particular location separated into real and imaginary components, along with the corresponding uncertainties. Data values and uncertainties are unitless with no normalization factor. The columns for this data format are as follows:

.. math::
    | \; x \; | \; y \; | \; z \; | \;\;\; T_x \; data \;\;\; | \;\;\; T_y \; data \;\;\; |

such that each :math:`T_x \; data` is comprised of 4 columns:

.. math::

    | \; T^\prime_x \; | \; U^\prime_x \; | \; T^{\prime \prime}_x \; | \; U^{\prime \prime}_x \; |

where

    - :math:`T^\prime_x` is the real component of :math:`T_x`
    - :math:`T^{\prime\prime}_x` is the imaginary component of :math:`T_x`
    - :math:`U^\prime_x` is the uncertainty on :math:`T^\prime_x`
    - :math:`U^{\prime\prime}_x` is the uncertainty on :math:`T^{\prime\prime}_x`

and similarly for :math:`y`.


.. important::

	- For **MTT data (ZTEM)**, the first line in the array refers to the base/reference station location. Only the x,y and z locations are required. **However**, each remaining field must be given a flag value. *Example for first row:* :math:`350 \;\; 200 \;\; 0 \;\; i \;\; i \;\; i \;\; i \;\; i \;\; i \;\; i \;\; i`


Joint MT and ZTEM data (DATATYPE = MTB):
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In this case there are two data arrays, one for the MT data and one for the ZTEM data. 

**MT data:**

Each row in the array contains the elements of the impedance tensor at a particular location separated into real and imaginary components, along with the corresponding uncertainties. The units for MT data are (V/A). The columns for this data format are as follows:

.. math::
    | \; x \; | \; y \; | \; z \; | \;\;\; Z_{xx} \; data \;\;\; | \;\;\; Z_{xy} \; data \;\;\; | \;\;\; Z_{yx} \; data \;\;\; | \;\;\; Z_{yy} \; data \;\;\; | \; 8 \; flagged \; columns \; |

such that each :math:`Z_{ij} \; data` is comprised of 4 columns:

.. math::

    | \; Z^\prime_{ij} \; | \; U^\prime_{ij} \; | \; Z^{\prime \prime}_{ij} \; | \; U^{\prime \prime}_{ij} \; |

where

    - :math:`Z^\prime_{ij}` is the real component of entry i,j of the impedance tensor
    - :math:`Z^{\prime\prime}_{ij}` is the imaginary component of entry i,j of the impedance tensor
    - :math:`U^\prime_{ij}` is the uncertainty on :math:`Z^\prime_{ij}`
    - :math:`U^{\prime\prime}_{ij}` is the uncertainty on :math:`Z^{\prime\prime}_{ij}`


**ZTEM data:**

Each row in the array contains the elements of the transfer function at a particular location separated into real and imaginary components, along with the corresponding uncertainties. Data values and uncertainties are unitless with no normalization factor. The columns for this data format are as follows:

.. math::
    | \; x \; | \; y \; | \; z \; | \; 16 \; flagged \; columns \; | \;\;\; T_x \; data \;\;\; | \;\;\; T_y \; data \;\;\; |

such that each :math:`T_x \; data` is comprised of 4 columns:

.. math::

    | \; T^\prime_x \; | \; U^\prime_x \; | \; T^{\prime \prime}_x \; | \; U^{\prime \prime}_x \; |

where

    - :math:`T^\prime_x` is the real component of :math:`T_x`
    - :math:`T^{\prime\prime}_x` is the imaginary component of :math:`T_x`
    - :math:`U^\prime_x` is the uncertainty on :math:`T^\prime_x`
    - :math:`U^{\prime\prime}_x` is the uncertainty on :math:`T^{\prime\prime}_x`

and similarly for :math:`y`.











