.. _preFileINV:

Predicted Data
==============

The predicted data files are output from **ZTEM_MT3Dinv.exe** and contain the locations and predicted field data. The order of the data points are the same as the :ref:`observations file <obsFile>`. Each block, separated by a blank line, are the data for a particular transmitter/frequency. Thus predicted data files take the format (unless joint MT and ZTEM):

|
| **Data Array 1**
|
| **Data Array 2**
|
| :math:`\;\;\;\;\;\;\;\; \vdots`
|
| **Data Array N**
|
|


.. _preFileINV_MTZ:

Impedance Tensor Data (DATATYPE = MTZ)
--------------------------------------

Each row in the array contains the elements of the impedance tensor at a particular location separated into real and imaginary components. The units for predicted MT data are (V/A). The columns for this data format are as follows:

.. math::
    | \; x \; | \; y \; | \; z \; | \; Z^\prime_{xx} \; | \; Z^{\prime \prime}_{xx} \; | \; Z^\prime_{xy} \; | \; Z^{\prime \prime}_{xy} \; | \; Z^\prime_{yx} \; | \; Z^{\prime \prime}_{yx} \; | \; Z^\prime_{yy} \; | \; Z^{\prime \prime}_{yy} \; |

where

    - :math:`Z^\prime_{ij}` is the real component of entry i,j of the impedance tensor
    - :math:`Z^{\prime\prime}_{ij}` is the imaginary component of entry i,j of the impedance tensor

.. _preFileINV_MTR:

Apparent Resistivity and Phase Data (DATATYPE = MTR)
----------------------------------------------------

Each row in the array contains the apparent resistivity and phase corresponding to impedance tensor entries at a particular location. The units for apparent resistivity data are :math:`\Omega m` and phase are :math:`\phi \in [-180^o,180^o]`. The columns for this data format are as follows:

.. math::
    | \; x \; | \; y \; | \; z \; | \; \rho_{xx} \; | \; \phi_{xx} \; | \; \rho_{xy} \; | \; \phi_{xy} \; | \; \rho_{yx} \; | \; \phi_{yx} \; | \; \rho_{yy} \; | \; \phi_{yy} \; |

where

    - :math:`\rho_{ij}` is the apparent resistivity corresponding to entry i,j of the impedance tensor
    - :math:`\phi_{ij}` is the phase corresponding to entry i,j of the impedance tensor

.. _preFileINV_MTT:

ZTEM Data (DATATYPE = MTT)
--------------------------

Each row in the array contains the elements of the transfer function at a particular location separated into real and imaginary components. Predicted ZTEM data are unitless with no normalization factor. The columns for this data format are as follows:

.. math::
    | \; x \; | \; y \; | \; z \; | \; T^\prime_x \; | \; T^{\prime \prime}_x \; | \; T^\prime_y \; | \; T^{\prime \prime}_y \; |

where

    - :math:`T^\prime_x` is the real component of :math:`T_x`
    - :math:`T^{\prime\prime}_x` is the imaginary component of :math:`T_x`

and similarly for :math:`y`.


.. important::

	- For **MTT data**, the first line of each data array within the :ref:`observations file<obsFile>` contains the base/reference station location with the remaining columns flagged. As a result, if a data array contains :math:`N` rows within the observations file, the associated array within the predicted data file will have :math:`N-1` rows.


Joint MT and ZTEM Data (DATATYPE = MTB)
---------------------------------------

In the case that joint MT and ZTEM data are being inverted, the predicted data takes the following format:


|
| **MT Data Array 1**
|
| **ZTEM Data Array 1**
|
| **MT Data Array 2**
|
| **ZTEM Data Array 2**
|
| :math:`\;\;\;\;\;\;\;\; \vdots`
|
| **MT Data Array N**
|
| **ZTEM Data Array N**
|
|


where the :ref:`format of MT data arrays<preFileINV_MTZ>` and the :ref:`format of ZTEM data arrays<preFileINV_MTT>` are the same as above.









