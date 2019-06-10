.. _example_inv:

Inversion
=========

Here, the code **ZTEM_MT3Dinv.exe** and the input file **mt3dinv.inp** (:ref:`see format<mtztem_input_inv>`) are used to invert MT data. MT data were created in the example :ref:`forward modeling<example_fwd>` and simple floor uncertainties of 0.005 V/A were added to all impedance tensor elements. In practice, data are noisy and choosing appropriate uncertainties is very important for successful inversion. Files relevant to this part of the example are in the sub-folder *inv*. Before running this example, you may want to do the following:

	- `Download and open the zip folder containing the entire MTZ3D example <https://github.com/ubcgif/mtztem/raw/master/assets/MTZ3D_example.zip>`__ (if not done already)
	- Learn how to :ref:`run code from command line<mtztem_inv>`
	- Learn the :ref:`format of the input file<mtztem_input_inv>`

To invert the synthetic data, the following input file was used:


.. figure:: ../inputfiles/images/create_inv_input.png
     :align: center
     :width: 700

The true model (left) and recovered model (right) at iteration 4 are shown below. A cutoff of 0.05 S/m has been used for both models and the recovered model is transected at z = -1200 m.

.. figure:: images/inv.png
     :align: center
     :width: 700






