.. _modelFile:

Model File
==========

For a given model, the model file has the following format:


|
| :math:`m_{1,1,1}`
| :math:`m_{1,1,2}`
| :math:`\;\vdots`
| :math:`m_{1,1,nz}`
| :math:`m_{1,2,1}`
| :math:`\;\vdots`
| :math:`m_{1,nx,nz}`
| :math:`m_{2,1,1}`
| :math:`\;\vdots`
| :math:`m_{i,j,k}`
| :math:`\;\vdots`
| :math:`m_{ny,nx,nz}`
|
|

where :math:`N=nx \times ny \times nz` is the total number of cells in the corresponding tensor mesh. Cells in the x and y directions are organized E to W and S to N, respectively, while cells are organized top to bottom in the vertical. Thus :math:`m_{1,1,1}` is the top southwesternmost model value and :math:`m_{ny,nx,nz}` is the bottom northeastermost model value.

Conductivity Models
^^^^^^^^^^^^^^^^^^^

For conductivity models, conductivity values are entered in units S/m.

Susceptibility Models
^^^^^^^^^^^^^^^^^^^^^

For background susceptibility models, values are entered in SI units.

.. _modelActiveFile:

Active Cells Model
^^^^^^^^^^^^^^^^^^

In this case, cells are assigned a value of *1* if they are active during the inversion and *0* if they are not (i.e. air cells). Cells designated with a *0* in the active cells model are given a value of :math:`\sigma = 10^{-8}` S/m and :math:`\chi=0` SI in the inversion. 







