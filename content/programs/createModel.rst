.. _mtztem_model:

Create Model
============

To generate the tensor model on the tensor mesh, open a command window. In order, enter the path to **blk3cell.exe**, followed by the path to the tensor mesh file name (**tensor_mesh.txt**), followed by the path to the :ref:`input file <mtztem_blk3cell_input>` (denoted here as **blk3cell.inp**), followed by the desired name (or full path) for the output model file (denoted here as **3Dmodel.con**), all separated by spaces.

.. figure:: images/run_create_model.png
     :align: center
     :width: 700

**blk3cell.exe** outputs a :ref:`model<modelFile>` (**3Dmodel.con**) which contains a single value for each cell in the tensor mesh **3D_mesh.txt**.


