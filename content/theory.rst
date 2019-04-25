.. _theory:

Background Theory
=================

This section aims to provide the user with a basic review of the physics, discretization, and optimization techniques used to solve the frequency domain quasi-static electromagnetics problem. It
is assumed that the user has some background in these areas. For further reading see :cite:`Nabighian1991`.

.. important::
    The theory provided on this page works for the following right-handed coordinate systems:
        - X = Easting, Y = Northing, Z = Up (standard Cartesian)
        - X = Northing, Y = Easting, Z = Down (standard magnetotelluric which this code uses!)

.. _theory_fundamentals:

Fundamental Physics
-------------------

Maxwell's equations provide the starting point from which an understanding of how electromagnetic
fields can be used to uncover the substructure of the Earth. In the frequency domain Maxwell's
equations are:

.. math::
    \begin{align}
        &\nabla \times \mathbf{E} - i\omega\mu \mathbf{H} = 0 \\
        &\nabla \times \mathbf{H} - \sigma \mathbf{E} = \mathbf{s} \\
        &\nabla \cdot  \mathbf{J} = 0 \\
        &\mathbf{J} = \sigma \mathbf{E}
    \end{align}
    :label: Maxwell_eq

where :math:`\mathbf{E}` is the electric field, :math:`\mathbf{H}` is the magnetic field, :math:`\mathbf{J}` is the current density, :math:`\mathbf{s}` is some external source and :math:`e^{-i\omega t}` is suppressed. Symbols :math:`\mu`, :math:`\sigma` and :math:`\omega` are the magnetic permeability, conductivity, and angular frequency, respectively. This formulation assumes a quasi-static mode so that the system can be viewed as a diffusion equation (Weaver, 1994; Ward and Hohmann, 1988 in :cite:`Nabighian1991`). By doing so, some difficulties arise when
solving the system;

    - the curl operator has a non-trivial null space making the resulting linear system highly ill-conditioned
    - the conductivity :math:`\sigma` varies over several orders of magnitude

.. _theory_nsem:

Natural Sources: MT and ZTEM
----------------------------

The sources in the magnetotelluric (MT) and Z-axis tipper electromagnetic (ZTEM) methods are modeled as plane waves originating
from natural phenomenon. These waves can be of very low frequency (< 1 Hz) and very high
energy, making it possible to image very deep targets. This also implies that the source term is
zero inside the domain of interest, and therefore the source term on the boundaries becomes very
important. For natural source electromagnetic (NSEM) problems, Maxwell's equations can be expressed as:

.. math::
    \begin{align}
        \nabla \times &\mathbf{E} - i\omega\mu \mathbf{H} = 0 \\
        \nabla \times &\mathbf{H} - \sigma \mathbf{E} = 0 \\
        &\mathbf{E} \big |_{\partial \Omega} = \mathbf{E_0}
    \end{align}
    :label: NSEM_system

where :math:`\mathbf{E_0}` is the electric field solution on the boundary :math:`\partial \Omega`.

Consider the case where the Earth is a uniform half-space with a plane surface. If the source field is assumed
to be homogeneous, infinite in dimension, and is located at infinity, then the plane waves impinging on the Earth's surface travel in the z-direction.

For plane waves polarized such that their electric fields lie along the x direction, the electric field is defined by the following Helmholtz equation:

.. math::
    \frac{\partial^2 E_x}{\partial z^2} = k^2 E_x \;\;\; \textrm{s.t.} \;\;\; k^2 = -i\mu\omega\sigma
    :label: Helmholtz_E

and the relationship between :math:`E_x` and :math:`H_y` is given by:

.. math::
    i \omega \mu H_y = \frac{\partial E_x}{\partial z}
    :label:

The solution to :eq:`Helmholtz_E` takes the form:

.. math::
    E_x = Q e^{-kz}
    :label:

where :math:`Q` is some constant. Taking the ratio of the electric and magnetic fields measured at the surface
gives:

.. math::
    Z_{xy} = \frac{E_x}{H_y} = \frac{-i\omega \mu}{k} = \sqrt{\dfrac{-i\omega\mu}{\sigma}}
    :label: impedance_hs


This implies that conductivity :math:`\sigma` of the Earth can be determined by taking measurements of the
field components, and therefore the impedance constitutes the basic MT response function, or data.
A 1D layered Earth model can be used to compute the source wave components by iteratively propagating a plane wave from the surface to depth.

Magnetotelluric (MT) Data
^^^^^^^^^^^^^^^^^^^^^^^^^

For a 3-dimensional Earth, the magnetotelluric data are defined by the impedance tensor. The impedance tensor can be defined using the ratios of electric and magnetic field components in both the x and y directions for 2 orthogonal plane wave polarizations; one polarization with the electric field along the x axis and one polarization with the electric file along the y axis. Where the impedance tensor :math:`\mathbf{Z}` is a 2 by 2 matrix:

.. math::
    \mathbf{Z} = \mathbf{E H}^{-1}
    :label:

such that:

.. math::
    \begin{bmatrix} Z_{xx} & Z_{xy} \\ Z_{yx} & Z_{yy} \end{bmatrix} =
    \begin{bmatrix} E_{x}^{(1)} & E_{x}^{(2)} \\ E_{y}^{(1)} & E_{y}^{(2)} \end{bmatrix}
    \begin{bmatrix} H_{x}^{(1)} & H_{x}^{(2)} \\ H_{y}^{(1)} & H_{y}^{(2)} \end{bmatrix}^{-1}
    :label: impedance_tensor

where 1 and 2 refer to fields associated with plane waves polarized along two perpendicular directions.

.. important::
    For standard MT data, X = Northing, Y = Easting and Z = Down; which this code uses! Thus:
        - Superscript :math:`\! ^{(1)}` refers to fields resulting from a plane wave whose electric field is polarized along the Northing direction. And superscript :math:`\! ^{(2)}` refers to fields resulting from a plane wave whose electric field is polarized along the Easting direction.
        - :math:`Z_{xy}` is essentially the ratio of the electric field along the Northing and the magnetic field along the Easting.


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

where :math:`r` is the location of the vertical field and :math:`r_0` is the location of the ground base station. :math:`T_{zx}` and :math:`T_{zy}` are the vertical field transfer functions, from z to x and z to y respectively. For a 3-dimensional Earth, the transfer function can be defined using the magnetic field components for 2 orthogonal plane wave polarizations; one polarization with the electric field along the x axis and one polarization with the electric file along the y axis. In this case,

.. math::
    \begin{bmatrix} H_z^{(1)} \\ H_z^{(2)} \end{bmatrix} =
    \begin{bmatrix} H_x^{(1)} & H_y^{(1)} \\ H_x^{(2)} & H_y^{(2)} \end{bmatrix}
    \begin{bmatrix} T_{zx} \\ T_{zy} \end{bmatrix}
    :label: transfer_fcn

where 1 and 2 refer to fields associated with plane waves polarized along two perpendicular directions. Thus the transfer functions are given by:

.. math::
    \begin{bmatrix} T_{zx} \\ T_{zy} \end{bmatrix} = \big ( H_x^{(1)} H_y^{(2)} - H_x^{(2)} H_y^{(1)} \big )^{-1}
    \begin{bmatrix} - H_y^{(1)} H_z^{(2)} + H_y^{(2)} H_z^{(1)} \\ H_x^{(1)} H_z^{(2)} - H_x^{(2)} H_z^{(1)} \end{bmatrix}
    

.. important::
    For standard natural source data, X = Northing, Y = Easting and Z = Down; which this code uses! Thus:
        - Superscript :math:`\! ^{(1)}` refers to fields resulting from a plane wave whose electric field is polarized along the Northing direction. And superscript :math:`\! ^{(2)}` refers to fields resulting from a plane wave whose electric field is polarized along the Easting direction.
        - :math:`T_{zx}` is the transfer function related to an incident plane wave whose electric field is polarized along the Northing direction; which produces magnetic fields with components in the Easting direction.


Discretization of Operators
---------------------------

The operators div, grad, and curl are discretized using a finite volume formulation. Although div and grad do not appear in :eq:`impedance_tensor` or :eq:`transfer_fcn`, they are required for the solution of the system. The divergence operator is discretized in the usual flux-balance approach, which by Gauss' theorem considers the current flux through each face of a cell. The nodal gradient (operates on a function with values on the nodes) is obtained by differencing adjacent nodes and dividing by edge length. The discretization of the curl operator is computed similarly to the divergence operator by utilizing Stokes theorem by summing the magnetic field components around the edge of each face. Please
see :cite:`Haber2012` for a detailed description of the discretization process.


.. _theory_fwd:

Forward Problem
---------------

Numerical Approach
^^^^^^^^^^^^^^^^^^

:eq:`Maxwell_eq` provides the starting point for solving the natural source electromagnetic (NSEM) problem. It is possible to discretize and solve this system in terms of the electric field. However a more stable approach is obtained by solving Maxwell's equation in terms of potentials. To do this, the electric field is decomposed into the sum of a vector potential (:math:`\mathbf{a}`) and the gradient of a scalar potential (:math:`\phi`):

.. math::
    \mathbf{E} = \mathbf{a} + \nabla \phi
    :label: E_decomp

To obtain a unique solution, we implement the Coulomb gauge condition:

.. math::
    \nabla \cdot \mathbf{a} = 0
    :label: coulomb_gauge

Replacing :math:`\mathbf{E}` with Eq. :eq:`E_decomp` in Maxwell's equations and using the Coulomb Gauge we obtain:

.. math::
    \nabla^2 \mathbf{A} - i \omega \mu \sigma ( \mathbf{A} + \nabla \phi ) = 0
    :label: helmholtz_a


The algorithm used for the MT forward modelling is based on the separation of the total electric and magnetic fields into primary parts that exist in a background conductivity model, and secondary parts that exist because of the difference between the actual conductivity model and the background model. That is,

.. math::
    \begin{align}
        \mathbf{E} &= \mathbf{E_p} + \mathbf{E_s} \\
        \mathbf{H} &= \mathbf{H_p} + \mathbf{H_s}
    \end{align}
    :label: primary_secondary_eh

where the primary fields :math:`\mathbf{E_p}` and :math:`\mathbf{H_p}` satisfy :eq:`Maxwell_eq` for the background conductivity :math:`\sigma_b` and the inhomogeneous boundary conditions. Equivalently, the vector and scalar potentials can be thought of as being divided into primary and secondary parts:

.. math::
    \begin{align}
        \mathbf{A} &= \mathbf{A_p} + \mathbf{A_s} \\
        \phi &= \phi_p + \phi_s
    \end{align}
    :label: primary_secondary_a

where :math:`\mathbf{A_p}` and :math:`\phi_p` satisfy :eq:`Maxwell_eq` for the background conductivity model and the inhomogeneous boundary conditions. Substituting Eqs. :eq:`primary_secondary_a` into Eq. :eq:`helmholtz_a` and the third expression in :eq:`Maxwell_eq` gives

.. math::
    \begin{align}
    \nabla^2  \mathbf{A_s} + i\omega \mu \sigma \big ( \mathbf{A_s} + \nabla \phi_s \big ) &= - i\omega \mu \Delta \sigma \mathbf{E_p} \\
    \nabla \cdot \sigma \mathbf{A_s} + \nabla \cdot \sigma \nabla \phi_s &= - \nabla \cdot \Delta \sigma \mathbf{E_p}
    \end{align}
    :label: a_system

where :math:`\Delta \sigma = \sigma - \sigma_b`. The preceding pair of simultaneous equations are the equations that are solved in the NSEM forward-modelling algorithm. The secondary potentials are assumed to vanish on the boundaries of the computational domain, that is, satisfy homogeneous boundary conditions. This pair of inhomogeneous equations, and the homogeneous boundary conditions, match the boundary value problem that is solved for the controlled-source case. The discretization and solution of Eqs. :eq:`a_system` is done in exactly the same way as for the controlled-source case; i.e. finite volume.

Source Term
^^^^^^^^^^^

For this approach, we solve a 1D wave equation of the following form:

.. math::
    \mathbf{\tilde{A} \tilde{u}_e} = \mathbf{\tilde{q}}
    :label: wave_eq_1d


where :math:`\mathbf{\tilde{u}_e}` is the electric field for the 1D solution polarized along the x or y directions. :math:`\mathbf{\tilde{A}}` is an operator of the form:

.. math::
    \mathbf{\tilde{A}} = \mathbf{L} + i \omega \mu_0 \tilde{\sigma}


such that :math:`\mathbf{L}` is the Laplacian operator, :math:`\mu_0` is the permeability of free-space and :math:`\tilde{\sigma}` is a 1D conductivity model. The right-hand side :math:`\mathbf{\tilde{q}}` is a vector of zeros except for :math:`\tilde{q}_1`. A Dirichlet condition is imposed by setting :math:`A_{11} = 1` and :math:`\tilde{q}_1 = i\omega \mu_0 h^{-1}`; where :math:`h` is the layer thickness. Once Eq. :eq:`wave_eq_1d` is solved for a particular frequency, the solution is transferred to the edges of an tensor mesh. If the electric field is polarized along the x direction, there are no electric fields along y or z; similarly for a solution polarized along the y direction. 

Let :math:`\mathbf{u_s}` and :math:`\sigma_s` be the electric fields and 1D conductivity model transferred to the edges of the tensor mesh, respectively. Then the source term in Eq. *discrete e sys* is computed for a given frequency and polarization using:

.. math::
    \frac{1}{i\omega} \mathbf{A u_s} = \mathbf{s}


where :math:`\mathbf{A}` is similar to expression *A operator*, except the mass matrix :math:`\mathbf{M_\sigma}` is formed using the transferred conductivity :math:`\sigma_s`.



.. .. _theory_mt:

.. MT Problem
.. ^^^^^^^^^^

.. Unlike for a controlled source, one more step is required in the forward-modelling procedure for the MT case. In practice, the source field for the MT case is never known and its effects are “cancelled” by considering ratios of the electric and magnetic fields. For a 3-dimensional Earth the magnetotelluric data are defined as the ratio of the electric and magnetic field components in both the x and y directions for 2 orthogonal polarizations, also know
.. as the impedance tensor :math:`\mathbf{Z}`, where:

.. .. math::
..     \mathbf{ZH} = \mathbf{E}
..     :label:

.. such that:

.. .. math::
..     \begin{bmatrix} Z_{xx} & Z_{xy} \\ Z_{yx} & Z_{yy} \end{bmatrix}
..     \begin{bmatrix} H_x^1 & H_x^2 \\ H_y^1 & H_y^1 \end{bmatrix}=
..     \begin{bmatrix} E_x^1 & E_x^2 \\ E_y^1 & E_y^1 \end{bmatrix}
..     :label: impedance_tensor

.. The superscripts in the above equation indicate the E and H fields computed in the same
.. conductivity model for two different polarizations of the source field, and the subscripts denote
.. the components of the fields. Two invocations of the forward modelling algorithm are therefore
.. required, once using a primary field calculated for boundary conditions corresponding to an inducing
.. H-field polarized in the x-direction, and again using a primary field calculated with
.. boundary conditions for an inducing H-field polarized in the y-direction. The MT case therefore
.. requires the solution of two systems of equations:

.. .. math::
..     \begin{align}
..     A(m) u_s^{(1)} &= \hat{q}^{(1)} (m) \\
..     A(m) u_s^{(2)} &= \hat{q}^{(2)} (m)
..     \end{align}
..     :label: mt_system

.. here the vector :math:`u_s^{(1)}` contains the values of the components of the secondary vector potential and
.. the values of the secondary scalar potential on the mesh for the first polarization, :math:`\hat{q}^{(1)}` represents
.. the discretization of the right-hand side of Eqs. :eq:`a_system` for the first polarization, and :math:`A(m)` represents
.. the discretization of the left-hand side of Eqs. :eq:`a_system`. The second equation in :eq:`mt_system` is the equivalent equation for
.. the second polarization. Each of these equations is analogous to the matrix equation that is
.. solved for the controlled-source case.

.. MT data can be represented by the real and imaginary components of the entries of the impedance tensor, or as the apparent resistivity and phase of each entry, i.e.:

.. .. math::
..     \rho_{ij} = \frac{1}{\omega \mu} \big | Z_{ij} \big |^2
..     :label:

.. and

.. .. math::
..     \phi_{ij} = \textrm{tan} \Bigg [ \frac{\textrm{Im} [Z_{ij}]}{\textrm{Re} [Z_{ij}]} \Bigg ]
..     :label:

.. .. _theory_ztem:

.. ZTEM Problem
.. ^^^^^^^^^^^^

.. The Z-Axis Tipper Electromagnetic Technique (ZTEM) (Lo2008) records the vertical component of the magnetic field everywhere above the survey area while recording the horizontal fields at a ground base reference station. In the same manner as demonstrated for MT, transfer functions are computed which relate the vertical fields to the ground based horizontal fields. This relation is given by:

.. .. math::
..     H_z(r) = T_{zx}(r,r_0)H_x(r_0) + T_{zy}(r,r_0)H_y(r_0)
..     :label:

.. where :math:`r` is the location of the vertical field and :math:`r_0` is the location of the ground base station. :math:`T_{zx}` and :math:`T_{zy}` are the vertical field transfer functions, from z to x and z to y respectively. The transfer
.. functions are given by:

.. .. math::
..     \begin{bmatrix} T_x \\ T_y \end{bmatrix} =
..     \Big ( H_x^{(r)}H_y^{(r_0)} - H_x^{(r_0)}H_y^{(r)} \Big )^{-1}
..     \begin{bmatrix} - H_y^{(r)}H_z^{(r_0)} + H_y^{(r_0)}H_z^{(r)} \\ H_x^{(r)}H_z^{(r_0)} - H_x^{(r_0)}H_z^{(r)} \end{bmatrix}
..     :label: transfer_fcn


.. _theory_sensitivity:

Sensitivity
-----------




.. _theory_inv:

Inversion Problem
-----------------

To solve the inverse problem, we minimize the following global objective function:


.. math::
    \phi = \phi_d + \beta \phi_m
    :label: global_objective


where :math:`\phi_d` is the data misfit and :math:`\phi_m` is the model objective function. The data misfit ensures the recovered model adequately explains the set of field observations. The model objective function adds geological constraints to the recovered model.

.. _theory_inv_misfit:

Data Misfit
^^^^^^^^^^^

Here, the data misfit is represented as the L2-norm of a weighted residual between the observed data (:math:`d_{obs}`) and the predicted data for a given conductivity model :math:`\boldsymbol{\sigma}`, i.e.:

.. math::
    \phi_d = \frac{1}{2} \big \| \mathbf{W_d} \big ( \mathbf{d_{obs}} - \mathbb{F}[\boldsymbol{\sigma}] \big ) \big \|^2
    :label: data_misfit_2


where :math:`W_d` is a diagonal matrix containing the reciprocals of the uncertainties :math:`\boldsymbol{\varepsilon}` for each measured data point, i.e.:

.. math::
    \mathbf{W_d} = \textrm{diag} \big [ \boldsymbol{\varepsilon}^{-1} \big ] 


.. important:: For a better understanding of the data misfit, see the `GIFtools cookbook <http://giftoolscookbook.readthedocs.io/en/latest/content/fundamentals/Uncertainties.html>`__ .


Model Objective Function
^^^^^^^^^^^^^^^^^^^^^^^^

Due to the ill-posedness of the problem, there are no stable solutions obtained by freely minimizing the data misfit, and thus regularization is needed. The regularization uses penalties for both smoothness, and likeness to a reference model :math:`m_{ref}` supplied by the user. The model objective function is given by:

.. math::
    \begin{align}
    \phi_m = \frac{\alpha_s}{2} \!\int_\Omega w_s | m - & m_{ref} |^2 dV
    + \frac{\alpha_x}{2} \!\int_\Omega w_x \Bigg | \frac{\partial}{\partial x} \big (m - m_{ref} \big ) \Bigg |^2 dV \\
    &+ \frac{\alpha_y}{2} \!\int_\Omega w_y \Bigg | \frac{\partial}{\partial y} \big (m - m_{ref} \big ) \Bigg |^2 dV
    + \frac{\alpha_z}{2} \!\int_\Omega w_z \Bigg | \frac{\partial}{\partial z} \big (m - m_{ref} \big ) \Bigg |^2 dV
    \end{align}
    :label:

where :math:`\alpha_s, \alpha_x, \alpha_y` and :math:`\alpha_z` weight the relative emphasis on minimizing differences from the reference model and the smoothness along each gradient direction. And :math:`w_s, w_x, w_y` and :math:`w_z` are additional user defined weighting functions.

An important consideration comes when discretizing the regularization onto the mesh. The gradient operates on
cell centered variables in this instance. Applying a short distance approximation is second order
accurate on a domain with uniform cells, but only :math:`\mathcal{O}(1)` on areas where cells are non-uniform. To
rectify this a higher order approximation is used (:cite:`Haber2012`). The second order approximation of the model
objective function can be expressed as:

.. math::
    \phi_m (\mathbf{m}) = \mathbf{\big (m-m_{ref} \big )^T W^T W \big (m-m_{ref} \big )}

where the regularizer is given by:

.. math::
    \begin{align}
    \mathbf{W^T W} =& \;\;\;\;\alpha_s \textrm{diag} (\mathbf{w_s \odot v}) \\
    & + \alpha_x \mathbf{G_x^T} \textrm{diag} (\mathbf{w_x \odot v_x}) \mathbf{G_x} \\
    & + \alpha_y \mathbf{G_y^T} \textrm{diag} (\mathbf{w_y \odot v_y}) \mathbf{G_y} \\
    & + \alpha_z \mathbf{G_z^T} \textrm{diag} (\mathbf{w_z \odot v_z}) \mathbf{G_z}
    \end{align}
    :label: MOF

The Hadamard product is given by :math:`\odot`, :math:`\mathbf{v_x}` is the volume of each cell averaged to x-faces, :math:`\mathbf{w_x}` is the weighting function :math:`w_x` evaluated on x-faces and :math:`\mathbf{G_x}` computes the x-component of the gradient from cell centers to cell faces. Similarly for y and z.

If we require that the recovered model values lie between :math:`\mathbf{m_L  \preceq m \preceq m_H}` , the resulting bounded optimization problem we must solve is:

.. math::
    \begin{align}
    &\min_m \;\; \phi_d (\mathbf{m}) + \beta \phi_m(\mathbf{m}) \\
    &\; \textrm{s.t.} \;\; \mathbf{m_L \preceq m \preceq m_H}
    \end{align}
    :label: inverse_problem

A simple Gauss-Newton optimization method is used where the system of equations is solved using ipcg (incomplete preconditioned conjugate gradients) to solve for each G-N step. For more
information refer again to :cite:`Haber2012` and references therein.


Inversion Parameters and Tolerances
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _theory_cooling:

Cooling Schedule
~~~~~~~~~~~~~~~~

Our goal is to solve Eq. :eq:`inverse_problem`, i.e.:

.. math::
    \begin{align}
    &\min_m \;\; \phi_d (\mathbf{m}) + \beta \phi_m(\mathbf{m - m_{ref}}) \\
    &\; \textrm{s.t.} \;\; \mathbf{m_L \leq m \leq m_H}
    \end{align}

but how do we choose an acceptable trade-off parameter :math:`\beta`? For this, we use a cooling schedule. This is described in the `GIFtools cookbook <http://giftoolscookbook.readthedocs.io/en/latest/content/fundamentals/Beta.html>`__ . The cooling schedule is defined by the following parameters:

    - **beta_start:** The initial value for :math:`\beta`
    - **beta_end:** The minimum :math:`\beta` for which Eq. :eq:`inverse_problem` is solved before the inversion will quit
    - **beta_factor:** The factor at which :math:`\beta` is decrease to a subsequent solution of Eq. :eq:`inverse_problem`
    - **Chi Factor:** The inversion program stops when the data misfit is less than the target data misfit :math:`\phi_d^* = N \times Chi \; Factor`, where :math:`N` is the number of data observations

If the user chooses the *DEFAULT* options, then we let:

.. math::
    \begin{align}
        \textrm{beta_factor} &= 0.16681 \\
        \textrm{beta_start} &= 1000 \times \dfrac{\| \mathbf{Jr} \|^2}{\| \mathbf{Wr} \|^2} \\
        \textrm{beta_end} &= 10^{-7} \times \textrm{beta_start} 
    \end{align}

.. _theory_GN:

Gauss-Newton Update
~~~~~~~~~~~~~~~~~~~

For a given trade-off parameter (:math:`\beta`), the model :math:`\mathbf{m}` is updated using the Gauss-Newton approach. Because the problem is non-linear, several model updates may need to be completed for each :math:`\beta`. Where :math:`k` denotes the Gauss-Newton iteration, we solve:

.. math::
    \mathbf{H}_k \, \mathbf{\delta m}_k = - \nabla \phi_k
    :label: GN_gen


using the current model :math:`\mathbf{m}_k` and update the model according to:

.. math::
    \mathbf{m}_{k+1} = \mathbf{m}_{k} + \alpha \mathbf{\delta m}_k
    :label: GN_update


where :math:`\mathbf{\delta m}_k` is the step direction, :math:`\nabla \phi_k` is the gradient of the global objective function, :math:`\mathbf{H}_k` is an approximation of the Hessian and :math:`\alpha` is a scaling constant. This process is repeated until any of the following occurs:

1. The gradient is sufficiently small, i.e.:

.. math::
    \| \nabla \phi_k \|^2 < tol \_ nl

2. The smallest component of the model perturbation its small in absolute value, i.e.:

.. math::
    \textrm{max} ( |\mathbf{\delta m}_k | ) < mindm

3. A max number of GN iterations have been performed, i.e.

.. math::
    k = nit


.. _theory_IPCG:

Gauss-Newton Solve
~~~~~~~~~~~~~~~~~~

Here we discuss the details of solving Eq. :eq:`GN_gen` for a particular Gauss-Newton iteration :math:`k`. Using the data misfit from Eq. :eq:`data_misfit_2` and the model objective function from Eq. :eq:`MOF`, we must solve:

.. math::
    \Big [ \mathbf{J^T W_d^T W_d J + \beta \mathbf{W^T W}} \Big ] \mathbf{\delta m}_k =
    - \Big [ \mathbf{J^T W_d^T W_d } \big ( \mathbf{d_{obs}} - \mathbb{F}[\mathbf{m}_k] \big ) + \beta \mathbf{W^T W m}_k \Big ]
    :label: GN_expanded


where :math:`\mathbf{J}` is the sensitivity of the data (:math:`\mathbf{Z}` or :math:`\mathbf{T}`) to the current model :math:`\mathbf{m}_k`; see :ref:`sensitivity section <theory_sensitivity>` to learn how sensitivities are computed. The system is solved for :math:`\mathbf{\delta m}_k` using the incomplete-preconditioned-conjugate gradient (IPCG) method. This method is iterative and exits with an approximation for :math:`\mathbf{\delta m}_k`. Let :math:`i` denote an IPCG iteration and let :math:`\mathbf{\delta m}_k^{(i)}` be the solution to :eq:`GN_expanded` at the :math:`i^{th}` IPCG iteration, then the algorithm quits when:

1. the system is solved to within some tolerance and additional iterations do not result in significant increases in solution accuracy, i.e.:

.. math::
    \| \mathbf{\delta m}_k^{(i-1)} - \mathbf{\delta m}_k^{(i)} \|^2 / \| \mathbf{\delta m}_k^{(i-1)} \|^2 < tol \_ ipcg


2. a maximum allowable number of IPCG iterations has been completed, i.e.:

.. math::
    i = max \_ iter \_ ipcg



















.. Exactly as for the controlled-source case, the MT inverse problem is solved by finding the
.. conductivity model that minimizes the sum of a data misfit term and a measure of the amount of
.. structure in the model, where this model is determined using an iterative, Gauss-Newton
.. procedure. The only difference between the MT and controlled-source cases is the explicit
.. composition of the Jacobian matrix of sensitivities. As mentioned above, the data in the MT
.. inverse problem are impedances, or functions of the impedances, and can be represented by:

.. .. math::
..     d_i = \mathbb{F}_i \big [ Q \big ( u_p^{(1)} + u_s^{(1)} \big ), \; Q \big ( u_p^{(2)} + u_s^{(2)} \big ) \big ]
..     :label: datum

.. where :math:`d_i` is the i-th datum, :math:`Q` is the matrix that produces the components of the E and H fields
.. at the observation locations given the values of the vector and scalar potentials on the
.. mesh, and the function :math:`\mathbb{F}_i` represents the operation of calculating the i-th datum.
.. The sensitivity of the i-th datum with respect to the j-th model parameter is therefore

.. .. math::
..     J_{ij} = \frac{\partial d_i}{\partial m_j} = 
..     \frac{\partial \mathbb{F}_i}{\partial F_k^{(1)}} \frac{\partial F_k^{(1)}}{\partial m_j} + \frac{\partial \mathbb{F}_i}{\partial F_k^{(2)}} \frac{\partial F_k^{(2)}}{\partial m_j}
..     S_{ij}^{(1)} \frac{\partial F_k^{(1)}}{\partial m_j} + S_{ij}^{(2)} \frac{\partial F_k^{(2)}}{\partial m_j}
..     :label: sensitivity

.. where :math:`F_k^{(1)}` represents an E or H-field component (for the first polarization) at the observation
.. location and

.. .. math::
..     \frac{\partial F_k^{(1)}}{\partial m_j} = \frac{\partial}{\partial m_j} \big [ Q_k \big ( u_p^{(1)} + u_s^{(1)} \big ) \big ] = Q_k \frac{\partial u_s^{(1)}}{\partial m_j}
..     :label:

.. since the primary fields are not dependent on the model parameters in the inversion.

.. The expressions for the derivatives of the secondary vector and scalar potentials for both
.. polarizations with respect to the j-th model parameter are obtained by differentiation both
.. sides of Eqs. :eq:`mt_system`:

.. .. math::
..     A(m) \frac{\partial u_s^{(1)}}{\partial m_j} + \frac{\partial}{\partial m_j} \bigg [ A(m) u_s^{(1)} \bigg ] = \frac{\partial \hat{q}^{(1)}}{\partial m_j}
..     :label:

.. where

.. .. math::
..     \frac{\partial u_s^{(1)}}{\partial m_j} = A^{-1}(m) \Bigg [ \frac{\partial \hat{q}^{(1)}}{\partial m_j} - \frac{\partial}{\partial m_j} \bigg [ A(m) u_s^{(1)} \bigg ] \Bigg ]
..     :label:

.. The structure of the right-hand sides of :eq:`mt_system` is very similar to the model dependent parts of the left-hand sides of the previous equations. Hence,

.. .. math::
..     \frac{\partial \hat{q}^{(1)}}{\partial m_j} = \frac{\partial}{\partial m_j} \bigg [ A(m) u_p^{(1)} \bigg ]
..     :label:

.. and similarly for the second polarization. Introducing the same notation as for the controlledsource
.. case, the derivative of the discretization of the vector and scalar potentials can be
.. expressed as:

.. .. math::
..     \frac{\partial u_s^{(1)}}{\partial m_j} = A^{-1}(m) \bigg [ G \big ( m ,u_p^{(1)} \big ) - G \big ( m ,u_s^{(1)} \big ) \bigg ]
..     :label:


.. and the product of Jacobian matrix of sensitivities with a vector, as

.. .. math::
..     Jv = \bigg [ J^{(1)} + J^{(2)} \bigg ] v = S^{(1)} Q A^{-1} \bigg [ \bigg ( G_p^{(1)} - G_s^{(1)} \bigg ) + S^{(2)} Q A^{-1} \bigg (  G_p^{(2)} - G_s^{(2)} \bigg ) \bigg ] v
..     :label: Jv

.. where the superscripts and subscripts indicate to which polarization each term refers, and
.. whether it involves the primary or secondary potentials.

.. Equation :eq:`Jv` also indicates the sequence of operations that are required to compute the
.. product of the Jacobian matrix with a vector, which is one of the two computationally intensive
.. operations required by the iterative solution to the Gauss-Newton normal system
.. of equations. It can be seen that for the MT case, the solution of two prototypical forward modelling
.. problems, :math:`Ax=b` , are required for one product of the Jacobian matrix with a
.. vector. Likewise, the solution of two prototypical transpose systems, :math:`A^T v =w`, are required
.. to compute the product of the transpose of the Jacobian matrix with a vector, which is the
.. other computationally-intensive operation that is required.






