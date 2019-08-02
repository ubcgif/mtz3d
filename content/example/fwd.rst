.. _example_fwd:

Forward Modeling
================

Here, the code **mtz3d.exe** and the input file **mt3dinv.inp** (:ref:`see format<mtztem_input_inv>`) are used to forward model MT data for a synthetic model. Files relevant to this part of the example are in the sub-folder *fwd*. For this example, we use the model that was created in the example :ref:`create model<example_model>`. Before running this example, you may want to do the following:

	- `Download and open the zip folder containing the entire MTZ3D example <https://github.com/ubcgif/mtztem/raw/master/assets/MTZ3D_example.zip>`__ (if not done already)
	- Learn how to :ref:`run code from command line<mtztem_inv>`
	- Learn the format of the input file :ref:`in the case of forward modeling<mtztem_input_inv>`

To forward model the data, the following input file was used:


.. figure:: ../inputfiles/images/create_fwd_input.png
     :align: center
     :width: 700

Predicted data are shown below.

.. figure:: images/fwd.png
     :align: center
     :width: 700

