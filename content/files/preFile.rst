.. _preFile:

Predicted Data File
===================

The predicted data file is output from **e3dmtfwd.exe** and contains the locations and predicted field data. The order of the data point is in the same order as the :ref:`survey/locations files <surveyFile>`. Each block, separated by a blank line, are the data for a particular transmitter and frequency. Thus predicted data files take the format:

|
| **Data Array 1**
|
| **Data Array 1**
|
| :math:`\;\;\;\;\;\;\;\; \vdots`
|
| **Data Array N**
|
|



MT data (DATATYPE = MTZ or MTE)
-------------------------------

Each row in the array contains the elements of the impedance tensor at a particular location separated into real and imaginary components. The units for predicted MT data are (V/A). The columns for this data format are as follows:

.. math::
    | \; x \; | \; y \; | \; z \; | \; Z^\prime_{11} \; | \; Z^{\prime \prime}_{11} \; | \; Z^\prime_{12} \; | \; Z^{\prime \prime}_{12} \; | \; Z^\prime_{21} \; | \; Z^{\prime \prime}_{21} \; | \; Z^\prime_{22} \; | \; Z^{\prime \prime}_{22} \; |

where

    - :math:`Z^\prime_{ij}` is the real component of entry i,j of the impedance tensor
    - :math:`Z^{\prime\prime}_{ij}` is the imaginary component of entry i,j of the impedance tensor


ZTEM data (DATATYPE = MTT or MTH)
---------------------------------

Each row in the array contains the elements of the transfer function at a particular location separated into real and imaginary components. Predicted ZTEM data are unitless with no normalization factor. The columns for this data format are as follows:

.. math::
    | \; x \; | \; y \; | \; z \; | \; T^\prime_x \; | \; T^{\prime \prime}_x \; | \; T^\prime_y \; | \; T^{\prime \prime}_y \; |

where

    - :math:`T^\prime_x` is the real component of :math:`T_x`
    - :math:`T^{\prime\prime}_x` is the imaginary component of :math:`T_x`

and similarly for :math:`y`.



















