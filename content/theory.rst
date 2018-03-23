.. _theory:

Background Theory
=================


.. _theory_fundamentals:

Forward Problem
---------------

Maxwell's equations provide the starting point from which an understanding of how electromagnetic fields can be used to uncover the substructure of the Earth. The sources in the magnetotelluric (MT) and Z-axis tipper elecromagnetic (ZTEM) methods are modeled as plain waves originating from natural phenomenon. These waves can be of very low frequency (< 1 Hz) and very high energy, making it possible to image very deep targets. This also implies that the source term is
zero inside the domain of interest, and therefore the source term on the boundaries becomes very important. And in contrast to the controlled-source case, the forward-modelling problem for the NSEM case involves homogeneous equations with inhomogeneous boundary conditions.

For natural source electromagnetic (NSEM) problems, we solve the following system in the frequency domain:

.. math::
    \begin{align}
        \nabla \times \mathbf{E} + i\omega\mu \mathbf{H} &= 0 \\
        \nabla \times \mathbf{H} - \sigma \mathbf{E} &= 0 \\
        \nabla \cdot \mathbf{J} &= 0
    \end{align}
    :label: Maxwell_eq

such that

.. math::
    \nabla \times \mathbf{H} \, \Big |_{\partial \Omega}= \mathbf{H_0} \times \mathbf{\hat{n}}
    :label: boundary_cond


where :math:`\mathbf{E}`, :math:`\mathbf{H}` and :math:`\mathbf{J}` are the electric fields, magnetic fields and current density respectively. For this formulation, a time dependence of :math:`e^{-i\omega t}` is assumed. Variables :math:`\mu`, :math:`\sigma` and :math:`\omega` are the magnetic permeability, conductivity, and angular frequency respectively. This formulation assumes a quasi-static mode so that the system can be viewed as a diffusion equation (Weaver, 1994; Ward and Hohmann, 1988 in :cite:`Nabighian1991`). By doing so, some difficulties arise when solving the system;

    - the curl operator has a non-trivial null space making the resulting linear system highly ill-conditioned
    - the conductivity :math:`\sigma` varies over several orders of magnitude
    - the fields can vary significantly near the sources but smooth out at distance requiring high resolution near sources


The electric field is decomposed into the sum of a vector potential (:math:`\mathbf{A}`) and the gradient of a scalar potential (:math:`\phi`):

.. math::
    \mathbf{E} = \mathbf{A} + \nabla \phi
    :label: E_decomp

To obtain a unique solution, we implement the Coulomb gauge condition:

.. math::
    \nabla \cdot \mathbf{A} = 0
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

.. _theory_mt:

MT Problem
^^^^^^^^^^

Unlike for a controlled source, one more step is required in the forward-modelling procedure for the MT case. In practice, the source field for the MT case is never known and its effects are “cancelled” by considering ratios of the electric and magnetic fields. For a 3-dimensional Earth the magnetotelluric data are defined as the ratio of the electric and magnetic field components in both the x and y directions for 2 polarizations, also know
as the impedance tensor :math:`\mathbf{Z}`, where:

.. math::
    \mathbf{ZH} = \mathbf{E}
    :label:

such that:

.. math::
    \begin{bmatrix} Z_{xx} & Z_{xy} \\ Z_{yx} & Z_{yy} \end{bmatrix}
    \begin{bmatrix} H_x^1 & H_x^2 \\ H_y^1 & H_y^1 \end{bmatrix}=
    \begin{bmatrix} E_x^1 & E_x^2 \\ E_y^1 & E_y^1 \end{bmatrix}
    :label: impedance_tensor

The superscripts in the above equation indicate the E and H fields computed in the same
conductivity model for two different polarizations of the source field, and the subscripts denote
the components of the fields. Two invocations of the forward modelling algorithm are therefore
required, once using a primary field calculated for boundary conditions corresponding to an inducing
H-field polarized in the x-direction, and again using a primary field calculated with
boundary conditions for an inducing H-field polarized in the y-direction. The MT case therefore
requires the solution of two systems of equations:

.. math::
    \begin{align}
    A(m) u_s^{(1)} &= \hat{q}^{(1)} (m) \\
    A(m) u_s^{(2)} &= \hat{q}^{(2)} (m)
    \end{align}
    :label: mt_system

here the vector :math:`u_s^{(1)}` contains the values of the components of the secondary vector potential and
the values of the secondary scalar potential on the mesh for the first polarization, :math:`\hat{q}^{(1)}` represents
the discretization of the right-hand side of Eqs. :eq:`a_system` for the first polarization, and :math:`A(m)` represents
the discretization of the left-hand side of Eqs. :eq:`a_system`. The second equation in :eq:`mt_system` is the equivalent equation for
the second polarization. Each of these equations is analogous to the matrix equation that is
solved for the controlled-source case.

MT data can be represented by the real and imaginary components of the entries of the impedance tensor, or as the apparent resistivity and phase of each entry, i.e.:

.. math::
    \rho_{ij} = \frac{1}{\omega \mu} \big | Z_{ij} \big |^2
    :label:

and

.. math::
    \phi_{ij} = \textrm{tan} \Bigg [ \frac{\textrm{Im} [Z_{ij}]}{\textrm{Re} [Z_{ij}]} \Bigg ]
    :label:

.. _theory_ztem:

ZTEM Problem
^^^^^^^^^^^^

The Z-Axis Tipper Electromagnetic Technique (ZTEM) (Lo2008) records the vertical component of the magnetic field everywhere above the survey area while recording the horizontal fields at a ground base reference station. In the same manner as demonstrated for MT, transfer functions are computed which relate the vertical fields to the ground based horizontal fields. This relation is given by:

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

.. _theory_inv:

Inversion Problem
-----------------

Exactly as for the controlled-source case, the MT inverse problem is solved by finding the
conductivity model that minimizes the sum of a data misfit term and a measure of the amount of
structure in the model, where this model is determined using an iterative, Gauss-Newton
procedure. The only difference between the MT and controlled-source cases is the explicit
composition of the Jacobian matrix of sensitivities. As mentioned above, the data in the MT
inverse problem are impedances, or functions of the impedances, and can be represented by:

.. math::
    d_i = \mathbb{F}_i \big [ Q \big ( u_p^{(1)} + u_s^{(1)} \big ), \; Q \big ( u_p^{(2)} + u_s^{(2)} \big ) \big ]
    :label: datum

where :math:`d_i` is the i-th datum, :math:`Q` is the matrix that produces the components of the E and H fields
at the observation locations given the values of the vector and scalar potentials on the
mesh, and the function :math:`\mathbb{F}_i` represents the operation of calculating the i-th datum.
The sensitivity of the i-th datum with respect to the j-th model parameter is therefore

.. math::
    J_{ij} = \frac{\partial d_i}{\partial m_j} = 
    \frac{\partial \mathbb{F}_i}{\partial F_k^{(1)}} \frac{\partial F_k^{(1)}}{\partial m_j} + \frac{\partial \mathbb{F}_i}{\partial F_k^{(2)}} \frac{\partial F_k^{(2)}}{\partial m_j}
    S_{ij}^{(1)} \frac{\partial F_k^{(1)}}{\partial m_j} + S_{ij}^{(2)} \frac{\partial F_k^{(2)}}{\partial m_j}
    :label: sensitivity

where :math:`F_k^{(1)}` represents an E or H-field component (for the first polarization) at the observation
location and

.. math::
    \frac{\partial F_k^{(1)}}{\partial m_j} = \frac{\partial}{\partial m_j} \big [ Q_k \big ( u_p^{(1)} + u_s^{(1)} \big ) \big ] = Q_k \frac{\partial u_s^{(1)}}{\partial m_j}
    :label:

since the primary fields are not dependent on the model parameters in the inversion.

The expressions for the derivatives of the secondary vector and scalar potentials for both
polarizations with respect to the j-th model parameter are obtained by differentiation both
sides of Eqs. :eq:`mt_system`:

.. math::
    A(m) \frac{\partial u_s^{(1)}}{\partial m_j} + \frac{\partial}{\partial m_j} \bigg [ A(m) u_s^{(1)} \bigg ] = \frac{\partial \hat{q}^{(1)}}{\partial m_j}
    :label:

where

.. math::
    \frac{\partial u_s^{(1)}}{\partial m_j} = A^{-1}(m) \Bigg [ \frac{\partial \hat{q}^{(1)}}{\partial m_j} - \frac{\partial}{\partial m_j} \bigg [ A(m) u_s^{(1)} \bigg ] \Bigg ]
    :label:

The structure of the right-hand sides of :eq:`mt_system` is very similar to the model dependent parts of the left-hand sides of the previous equations. Hence,

.. math::
    \frac{\partial \hat{q}^{(1)}}{\partial m_j} = \frac{\partial}{\partial m_j} \bigg [ A(m) u_p^{(1)} \bigg ]
    :label:

and similarly for the second polarization. Introducing the same notation as for the controlledsource
case, the derivative of the discretization of the vector and scalar potentials can be
expressed as:

.. math::
    \frac{\partial u_s^{(1)}}{\partial m_j} = A^{-1}(m) \bigg [ G \big ( m ,u_p^{(1)} \big ) - G \big ( m ,u_s^{(1)} \big ) \bigg ]
    :label:


and the product of Jacobian matrix of sensitivities with a vector, as

.. math::
    Jv = \bigg [ J^{(1)} + J^{(2)} \bigg ] v = S^{(1)} Q A^{-1} \bigg [ \bigg ( G_p^{(1)} - G_s^{(1)} \bigg ) + S^{(2)} Q A^{-1} \bigg (  G_p^{(2)} - G_s^{(2)} \bigg ) \bigg ] v
    :label: Jv

where the superscripts and subscripts indicate to which polarization each term refers, and
whether it involves the primary or secondary potentials.

Equation :eq:`Jv` also indicates the sequence of operations that are required to compute the
product of the Jacobian matrix with a vector, which is one of the two computationally intensive
operations required by the iterative solution to the Gauss-Newton normal system
of equations. It can be seen that for the MT case, the solution of two prototypical forward modelling
problems, :math:`Ax=b` , are required for one product of the Jacobian matrix with a
vector. Likewise, the solution of two prototypical transpose systems, :math:`A^T v =w`, are required
to compute the product of the transpose of the Jacobian matrix with a vector, which is the
other computationally-intensive operation that is required.






