.. _boundsFile:

Model File
==========

For a given mesh, the bounds file has the following 2 column format:

|
| :math:`LB_{1,1,1} \;\;\;\;\;\; UB_{1,1,1}`
| :math:`LB_{1,1,2} \;\;\;\;\;\; UB_{1,1,2}`
| :math:`\;\;\;\;\;\vdots\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\vdots`
| :math:`LB_{1,1,nz} \;\;\;\;\; UB_{1,1,nz}`
| :math:`LB_{1,2,1} \;\;\;\;\;\; UB_{1,2,1}`
| :math:`\;\;\;\;\;\vdots\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\vdots`
| :math:`LB_{1,nx,nz} \;\;\;\, UB_{1,nx,nz}`
| :math:`LB_{2,1,1} \;\;\;\;\;\;\, UB_{2,1,1}`
| :math:`\;\;\;\;\;\vdots\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\vdots`
| :math:`LB_{i,j,k} \;\;\;\;\;\;\; UB_{i,j,k}`
| :math:`\;\;\;\;\;\vdots\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\vdots`
| :math:`LB_{ny,nx,nz} \;\; UB_{ny,nx,nz}`
|
|

where :math:`N=nx \times ny \times nz` is the total number of cells in the corresponding tensor mesh, :math:`LB` refers to a lower bound value in S/m and :math:`UB` refers to an upper bound value in S/m. Cells in the x and y directions are organized E to W and S to N, respectively, while cells are organized top to bottom in the vertical. Thus :math:`LB_{1,1,1}` is the lower bound for the top southwesternmost cell and :math:`UB_{ny,nx,nz}` is the upper bound for the bottom northeastermost cell.

.. note:: Bounds provided for inactive cells, including the air, do not play a role in the solution and so they can be assigned any number.








