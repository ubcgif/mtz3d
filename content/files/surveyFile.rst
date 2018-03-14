.. _surveyFile:

Survey/Locations File
=====================

The survey/locations file is used to predict synthetic field data (forward modeling). This file contains all necessary survey information including: the number of transmitters (groups of natural source data), data types, measurement frequencies and receiver locations. 

.. note::
    - Bolded entries are fixed flags recognized by the Fortran codes and blue hyperlinked entries are values/regular expressions specified by the user



MT and ZTEM data
----------------

If only MT, ZTEM or apparent resistivity and phase data are being predicted, the lines of a locations file with one or more transmitters is formatted as follows:

| **DATATYPE** :math:`\;` :ref:`A<e3dmt_survey_ln1>`
| **!IGNORE** :math:`\;` :ref:`B<e3dmt_survey_ln2>`
|
| :ref:`C<e3dmt_survey_ln3>`
| :ref:`D<e3dmt_survey_ln4>`
| :ref:`Data Array<e3dmt_survey_ln5>`
|
| :ref:`C<e3dmt_survey_ln3>`
| :ref:`D<e3dmt_survey_ln4>`
| :ref:`Data Array<e3dmt_survey_ln5>`
|
| :math:`\;\;\;\;\;\;\;\; \vdots`
|
| :ref:`C<e3dmt_survey_ln3>`
| :ref:`D<e3dmt_survey_ln4>`
| :ref:`Data Array<e3dmt_survey_ln5>`
|
|


.. figure:: images/files_data.png
     :align: center
     :width: 700

     Example data file for MTZ data.


Joint MT and ZTEM
-----------------

If joint MT and ZTEM data are being predicted, the lines of a locations file with one or more transmitters is formatted as follows:

| **DATATYPE** :math:`\;` :ref:`A<e3dmt_survey_ln1>`
| **!IGNORE** :math:`\;` :ref:`B<e3dmt_survey_ln2>`
|
| :ref:`C_MT<e3dmt_survey_ln3>`
| :ref:`D_MT<e3dmt_survey_ln4>`
| :ref:`MT Data Array<e3dmt_survey_ln5>`
| :ref:`C_ZTEM<e3dmt_survey_ln3>`
| :ref:`D_ZTEM<e3dmt_survey_ln4>`
| :ref:`ZTEM Data Array<e3dmt_survey_ln5>`
|
| :ref:`C_MT<e3dmt_survey_ln3>`
| :ref:`D_MT<e3dmt_survey_ln4>`
| :ref:`MT Data Array<e3dmt_survey_ln5>`
| :ref:`C_ZTEM<e3dmt_survey_ln3>`
| :ref:`D_ZTEM<e3dmt_survey_ln4>`
| :ref:`ZTEM Data Array<e3dmt_survey_ln5>`
|
| :math:`\;\;\;\;\;\;\;\; \vdots`
|
| :ref:`C_MT<e3dmt_survey_ln3>`
| :ref:`D_MT<e3dmt_survey_ln4>`
| :ref:`MT Data Array<e3dmt_survey_ln5>`
| :ref:`C_ZTEM<e3dmt_survey_ln3>`
| :ref:`D_ZTEM<e3dmt_survey_ln4>`
| :ref:`ZTEM Data Array<e3dmt_survey_ln5>`
|
|



Parameter Descriptions
----------------------

.. _e3dmt_survey_ln1:

    - **(A) Data type:**. The type of data being forward modeled is specified at the beginning of the file. Example: *DATATYPE MTZ*. There are 4 options for DATATYPE:

        - "MTZ" - MT data (both real and imaginary impedance tensor data)
        - "MTR" - MT data (expressed in terms of apparent resistivity and phase)
        - "MTT" - ZTEM data (both real and imaginary z-tipper data)
        - "MTB" - A joint dataset combining datatypes MTZ and MTT


.. _e3dmt_survey_ln2:

    - **(B) Flag to ignore data entries:** A regular expression is entered, signifying data in the data structure which is ignored during the inversion. Example: *IGNORE -0*
        
.. _e3dmt_survey_ln3:

    - **(C) Frequency:** Frequency at which the corresponding set of field observations are made. Example: *FREQUENCY 1.0000E+002*.

.. _e3dmt_survey_ln4:

    - **(D) Number of receivers:** Number of receivers collecting data at the aforementioned frequency for the aforementioned data type. Example: *N_RECV 900*.

.. _e3dmt_survey_ln5:

    - **Data Array:** Contains the locations for the data specified by :ref:`data type<e3dmt_survey_ln1>`. The number of lines in this array is equal to the number of receivers.



.. _surveyFile_data:

Data Array
----------

**MT data (DATATYPE = MTZ or MTE) or ZTEM data:**

No matter what data type is being used (DATATYPE = MTZ, MTR, MTT or MTB), each row of the data array contains the x, y and z positions for readings at a particular location, i.e.:

.. math::
    | \; x \; | \; y \; | \; z \; |


.. important::

	- For **MTT data (ZTEM)**, the first line in the array refers to the base/reference station location. Thus if there are :math:`N` receiver locations specified for a given array with data type "MTT", the forward model will output :math:`N-1` predicted data.
    - For **MTB data (joint MT and ZTEM)**, the frequency and number of observation locations can differ.



















