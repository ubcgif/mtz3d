.. _theory:

Background Theory
=================

This section aims to provide the user with a basic review of the physics, discretization, and optimization techniques used to solve the frequency domain quasi-static electromagnetics problem. It
is assumed that the user has some background in these areas. For further reading see :cite:`Nabighian1991`.

.. _theory_fundamentals:

Fundamental Physics
-------------------

Maxwell's equations provide the starting point from which an understanding of how electromagnetic
fields can be used to uncover the substructure of the Earth. In the frequency domain Maxwell's
equations are:

.. math::
	\begin{align}
		&\nabla \times \mathbf{E} + i\omega\mu \mathbf{H} = 0 \\
		&\nabla \times \mathbf{H} - \sigma \mathbf{E} = \mathbf{S}
	\end{align}
	:label:

where :math:`\mathbf{E}` and :math:`\mathbf{H}` are the electric and magnetic fields, s is some external source, and :math:`\mu`, :math:`\sigma` and :math:`\omega` are the magnetic permeability, conductivity, and angular frequency respectively. This formulation assumes a quasi-static mode so that the system can be viewed as a diffusion equation (Weaver, 1994; Ward and Hohmann, 1988 in :cite:`Nabighian1991`). By doing so, some difficulties arise when
solving the system;

	- the curl operator has a non-trivial null space making the resulting linear system highly ill-conditioned
	- the conductivity :math:`\sigma` varies over several orders of magnitude
	- the fields can vary significantly near the sources but smooth out at distance requiring high resolution near sources

.. _theory_nsem:

Natural Sources: MT and ZTEM
----------------------------

The sources in the magnetotelluric (MT) and Z-axis tipper elecromagnetic (ZTEM) methods are modeled as plain waves originating
from natural phenomenon. These waves can be of very low frequency (< 1 Hz) and very high
energy, making it possible to image very deep targets. This also implies that the source term is
zero inside the domain of interest, and therefore the source term on the boundaries becomes very
important. For natural source electromagnetic (NSEM) problems, we solve the following system:

.. math::
	\begin{align}
		&\nabla \times \mathbf{E} + i\omega\mu \mathbf{H} = 0 \\
		&\nabla \times \mathbf{H} - \sigma \mathbf{E} = 0 \\
		&\mathbf{H} \times \mathbf{\hat n} \big |_{\partial \Omega} = \mathbf{H_0} \times \mathbf{\hat n}
	\end{align}
	:label: NSEM_system

where :math:`\partial \Omega` denotes the boundary, :math:`\mathbf{\hat n}` is the unit vector perpendicular to the boundary and :math:`\mathbf{H_0}` is the magnetic field solution in free-space.
The source is still the main difficulty in this method as it is unknown. To deal with this, consider the case where the Earth is a uniform half-space with a plane surface. If the source field is assumed
to be homogeneous, infinite in dimension, and is located at infinity, then the plane waves impinging on the Earth's surface travel in the z-direction and have components :math:`E_x` and :math:`H_y` only. In this case, the electric field is defined by the following Helmholtz equation:

.. math::
	\frac{\partial^2 E_x}{\partial z^2} = k^2 E_x \;\;\; \textrm{s.t.} \;\;\; k^2 = i\mu\omega\sigma
	:label: Helmholtz_E

and the relationship between :math:`E_x` and :math:`H_y` is given by:

.. math::
	H_y = -\frac{i}{\omega\mu} \frac{\partial E_x}{\partial z}
	:label:

The solution to :eq:`Helmholtz_E` takes the form:

.. math::
	E_x = Q e^{-kz}
	:label:

where :math:`Q` is some constant. Taking the ratio of the electric and magnetic fields measured at the surface
gives:

.. math::
	Z = \frac{E_x}{H_y} = \frac{i\omega \mu}{k} = \sqrt{\dfrac{i\omega\mu}{\sigma}}
	:label: impedance_hs


This implies that conductivity :math:`\sigma` of the Earth can be determined by taking measurements of the
field components, and therefore the impedance constitutes the basic MT response function, or data.
A 1D layered Earth model can be used to compute the source wave components by iteratively propagating a plane wave from the surface to depth.


MT Data
^^^^^^^

For a 3-dimensional Earth the magnetotelluric data are defined as the ratio of the electric and magnetic field components in both the x and y directions for 2 polarizations, also know
as the impedance tensor :math:`\mathbf{Z}`, where:

.. math::
	\mathbf{ZH} = \mathbf{E}
	:label:

such that:

.. math::
	\begin{bmatrix} Z_{11} & Z_{12} \\ Z_{21} & Z_{22} \end{bmatrix}
	\begin{bmatrix} H_{x1} & H_{x2} \\ H_{y1} & H_{y2} \end{bmatrix}=
	\begin{bmatrix} E_{x1} & E_{x2} \\ E_{y1} & E_{y2} \end{bmatrix}
	:label: impedance_tensor





ZTEM Data
^^^^^^^^^

The Z-Axis Tipper Electromagnetic Technique (ZTEM) (Lo2008) records
the vertical component of the magnetic field everywhere above the survey area while recording
the horizontal fields at a ground base reference station. In the same manner as demonstrated for
MT, transfer functions are computed which relate the vertical fields to the ground based horizontal
fields. This relation is given by:

.. math::
	H_z(r) = T_{zx}(r,r_0)H_x(r_0) + T_{zy}(r,r_0)H_y(r_0)
	:label:

where :math:`r` is the location of the vertical field and :math:`r_0` is the location of the ground base station. :math:`T_{zx}` and :math:`T_{zy}` are the vertical field transfer functions, from z to x and z to y respectively. The transfer
functions are given by:

.. math::
	\begin{bmatrix} T_x \\ T_y \end{bmatrix} =
	\Big ( H_x^{(r)}H_y^{(r_0)} - H_x^{(r_0)}H_y^{(r)} \Big )^{-1}
	\begin{bmatrix} - H_y^{(r)}H_z^{(r_0)} + H_y^{(r_0)}H_z^{(r)} \\ H_x^{(r)}H_z^{(r_0)} - H_x^{(r_0)}H_z^{(r)} \end{bmatrix}
	:label: transfer_fcn

Maxwell's equations and the source fields then discretized on an Octree mesh in order to solve the forward and inverse problems.



Octree Mesh
-----------

By using an Octree discretization of the earth domain, the areas near sources and likely model
location can be give a higher resolution while cells grow large at distance. In this manner, the
necessary refinement can be obtained without added computational expense. Figure(2) shows an
example of an Octree mesh, with nine cells, eight of which are the base mesh minimum size.


.. figure:: images/OcTree.png
     :align: center
     :width: 700


When working with Octree meshes, the underlying mesh is defined as a regular 3D orthogonal grid where
the number of cells in each dimension are :math:`2^{m_1} \times 2^{m_2} \times 2^{m_3}`, with grid size :math:`h`. This underlying mesh
is the finest possible, so that larger cells have lengths which increase by powers of 2 multiplied by
:math:`h`. The idea is that if the recovered model properties change slowly over a certain volume, the cells
bounded by this volume can be merged into one without losing the accuracy in modeling, and are
only refined when the model begins to change rapidly.



Discretization of Operators
---------------------------

The operators div, grad, and curl are discretized using a finite volume formulation. Although div and grad do not appear in :eq:`impedance_tensor`, they are required for the solution of the system. The divergence
operator is discretized in the usual flux-balance approach, which by Gauss' theorem considers the current flux through each face of a cell. The nodal gradient (operates on a function with values on the nodes) is obtained by differencing adjacent nodes and dividing by edge length. The discretization of the curl operator is computed similarly to the divergence operator by utilizing Stokes theorem by summing the magnetic field components around the edge of each face. Please
see :cite:`Haber2012` for a detailed description of the discretization process.



Forward Problem
---------------

The solutions for the :math:`\mathbf{H}` and :math:`\mathbf{E}` fields are computed iteratively using the stabilized conjugate gradient method (BiCGstab). Because of the null space of the curl operator a discrete Helmholtz decomposition is used to write the electric field as

.. math::
	\mathbf{E} = \mathbf{A} + \nabla \phi
	:label:

where :math:`\mathbf{A}` is a vector potential and :math:`\phi` is a scalar potential. For MT or ZTEM data, :eq:`NSEM_system` is solved by eliminating the curl operator and solving for :math:`\mathbf{A}` and :math:`\phi`.

The forward problem of simulating data can now be written in the following form. Let :math:`\mathbf{D(m)}` be the discrete linear system obtained by the discretization of Maxwell's equations, where :math:`\mathbf{m} = log(:mathbf{\sigma})`.
The electric fields :math:`U` on the edges everywhere in the mesh are then:

.. math::
	U(\sigma) = \mathbf{D(m)^{-1} S}
	:label:

where :math:`\mathbf{S} = (s_1,s_2)` is the source for 2 polarizations and is approximated from a 1D MT solution and
interpolated to the entire mesh. The fields at the receivers locations are then

.. math::
	\begin{align}
	\mathbf{H} = Q_h u \\
	\mathbf{E} = Q_e u
	\end{align}

where

.. math::
	Q_h = \dfrac{1}{i\omega\mu_0} Q_c A_{f2c} CURL
	:label:

and

.. math::
	Q_e = Q_c A_{e2c}
	:label:

The matrix :math:`Q_c` is an interpolation matrix from cell centers to receiver locations, :math:`A_{f2c}` averages from faces to cell centers, and :math:`A_{e2c}` averages from edges to cell centers.

.. _theory_inv:

Inverse Problem
---------------

Solving the non-linear EM inverse problem for electric conductivity is akin to minimizing the
following objective function:


.. math::
	\Phi_{mis} (\mathbf{m}) = \frac{1}{2} \bigg \| \Sigma \odot \big ( \mathbf{D}(\sigma) - \mathbf{d^{obs}} \big ) \big \|^2_2
	:label:


where :math:`\Sigma` is a matrix of the inverse standard deviation for each measured data point :math:`\mathbf{d^{obs}}`. Due to the ill-posedness of the problem, there are no stable solutions and a regularization is needed. The regularization used penalizes for both smoothness, and likeness to a reference model :math:`\mathbf{m_{ref}}` supplied by the user.

.. math::
	\Phi_{reg} (\mathbf{m-m_{ref}}) = \frac{1}{2} \big \| \nabla (\mathbf{m - m_{ref}}) \big \|^2_2
	:label:

An important consideration comes when discretizing the regularization. The gradient operates on
cell centered variables in this instance. Applying a short distance approximation is second order
accurate on a domain with uniform cells, but only :math:`\mathcal{O}(1)` on areas where cells are non-uniform. To
rectify this a higher order approximation is used (:cite:`Haber2012`). The discrete regularization
operator can then be expressed as

.. math::
	\begin{align}
	\Phi_{reg}(\mathbf{m}) &= \frac{1}{2} \int_\Omega \big | \nabla m \big |^2 dV \\
	& \approx \frac{1}{2}  \beta \mathbf{ m^T G_c^T} \textrm{diag} (\mathbf{A_f^T v}) \mathbf{G_c m}
	\end{align}
	:label:

where :math:`\mathbf{A_f}` is an averaging matrix from faces to cell centres, :math:`\mathbf{G}` is the cell centre to cell face gradient operator, and v is the cell volume For the benefit of the user, let :math:`\mathbf{WTW}` be the weighting matrix given by

.. math::
	\mathbf{WTW} = \beta \mathbf{ G_c^T} \textrm{diag}(\mathbf{A_f^T v}) \mathbf{G_c m} =
	\begin{bmatrix} \mathbf{\alpha_x} & & \\ & \mathbf{\alpha_y} & \\ & & \mathbf{\alpha_z} \end{bmatrix} \big ( \mathbf{G_x^T \; G_y^T \; G_z^T} \big ) \textrm{diag} (\mathbf{v_f}) \begin{bmatrix} \mathbf{G_x} \\ \mathbf{G_y} \\ \mathbf{G_z} \end{bmatrix}
	:label:

where :math:`\alpha_i` for :math:`i=x,y,z` are diagonal matricies. In the code the WTW matrix is stored as a separate matrix so that individual model norm components can be calculated. Now, if a cell weighting is used it is applied to the entire norm, that is, there is a w for each cell.

.. math::
	\mathbf{WTW} = \textrm{diag} (w) \mathbf{WTW} \textrm{diag} (w)
	:label:

There is also the option of choosing a cell interface weighting. This allows for a weight on each cell FACE. The user must supply the weights (:math:`w_x, w_y, w_z` ) for each weighted cell. When the interface
weighting option is chosen and the value is less than 1, a sharp discontinuity will be created. When
the value is greater than 1, there will be a smooth transition. To prevent the inversion from putting
"junk" on the surface, the top X and Y face weights should have a large value.

.. math::
	\mathbf{WTW} = \mathbf{\alpha_x G_x^T} \textrm{diag} (w_x v_f) \mathbf{G_x} + \mathbf{\alpha_y G_y^T} \textrm{diag} (w_y v_f) \mathbf{G_y} + \mathbf{\alpha_z G_z^T} \textrm{diag} (w_z v_f) \mathbf{G_z}
	:label:

The resulting optimization problem is therefore:

.. math::
	\begin{align}
	&\min_m \;\; \Phi_{mis} (\mathbf{m}) + \beta \Phi_{reg}(\mathbf{m - m_{ref}}) \\
	&\; \textrm{s.t.} \;\; \mathbf{m_L \leq m \leq m_H}
	\end{align}
	:label:

where :math:`\beta` is a regularization parameter, and :math:`\mathbf{m_L}` and :math:`\mathbf{m_H}` are upper and lower bounds provided by some a prior geological information.
A simple Gauss-Newton optimization method is used where the system of equations is solved using ipcg (incomplete preconditioned conjugate gradients) to solve for each G-N step. For more
information refer again to :cite:`Haber2012` and references therein.






