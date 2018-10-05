.. _title_Flocculation_Model:

*****************************************
Flocculation Model
*****************************************

Particle aggregation is the fundamental mechanism that facilitates ultra low energy and low cost removal of particles and pathogens from water. Aggregation requires successful collisions. Success is defined by particles  attaching when they collide.

Model assumptions
=================

Key understanding: coagulant nanoparticles are sticky
-----------------------------------------------------

Prior to the AguaClara flocculation model it was widely assumed that attachment was made possible by reducing the net surface charge of the particles. The AguaClara flocculation model is based on the understanding that coagulant nanoparticles are sticky and are much larger than the length scale of the repulsive forces due to surface charges. Thus surface charge is largely irrelevant and this explains why particle aggregation begins even with very low dosages of coagulant.

Key understanding: Particles follow the fluid
----------------------------------------------

The collisions are caused by particles having relative motion due to fluid deformation. Particle trajectories can be different from the fluid trajectory if the density of the fluid and the particle are significantly different and if the viscous effects are small compared with inertial effects (the Stokes number). The motion of primary particles and small flocs in surface water treatment have low Stokes numbers and follow the fluid trajectory.

Key understanding: Long range transport is the slow part of the collision process
------------------------------------------------------------------------------------

We need to calculate the rate of primary particle collisions. In turbulent flow flocculators the fluid deformation is caused by turbulent eddies that lose their energy to viscosity. The relative motion of particles would appear somewhat random as the small eddies have ever changing orientation and intensity. The result is that primary particles take a long meandering path before they finally approach each other and connect in a final collision. The path of relative motion prior to the collision can be thought of as having two distinct components.

 - The first component is long range transport when the particles are far apart with a separation distance that is proportional to the average distance between particles.
 - The second component is the short range transport at length scales less than the average particle separation distance to the final collision

The AguaClara flocculation model assumes a relatively high velocity and long distance random walk clearing a volume of fluid equal to the volume occupied by a single particle. This is followed by a slow, short, straight walk toward a collision. The insight that the long range transport is the rate limiting step will be used to estimate the time required for particle collisions.

Key understanding: Primary particles can't attach to large flocs during Flocculation
------------------------------------------------------------------------------------

In our early modeling work we assumed that collisions between primary particles and large flocs were favorable. This assumption led to the prediction that the highest quality water should be obtained when the raw water has the highest turbidity. That prediction is inconsistent with observations and led to the insight that during flocculation, primary particles are only able to collide successfully with other primary particles (or potentially with other very small flocs).

The only transport mechanism that could cause a clay particle to collide with a large floc is the fluid deformation caused by the linear velocity gradient. In our flocculators that linear velocity gradient is caused by turbulent eddies at much larger scales of the flow. We hypothesize that primary particles can not attach to large flocs because primary particles can not collide with large flocs! To understand why this collision is impossible, we need a simple insight.

The insight is that the large flocs drag fluid around as they rotates (due to the linear velocity gradient). The viscous layer around the large flocs creates a flow field in which there is no location far from the flocs that will eventually approach the surface of the flocs or even approach within the clay particle radius. If this is correct, then clay particles never collide with large flocs in a linear velocity gradient flow field.

.. todo:: Find evidence that proves or disproves the hypothesis that no collisions occur between dissimilar sized particles in a linear velocity gradient.

Key understanding: Relative velocities between particles are dominated by viscous shear
---------------------------------------------------------------------------------------

Relative velocities between particles are dominated by viscous shear because the separation distances are smaller than the inner viscous length scale. The average particle separation distance is given by

.. math::
   :label: eq_spacing_of_number_concentration

   \bar \Lambda  = \frac{1}{n_P^{\frac{1}{3}}} = {\rlap{\kern.08em--}V_{\rm{Surround}}}^\frac{1}{3}

| Where:
| :math:`\bar \Lambda` is the average separation distance
| :math:`n_P` is the number of particles per volume of suspension
| :math:`{\rlap{\kern.08em--}V_{\rm{Surround}}}` is the suspension volume occupied by one particle

The number concentration of particles is given by

.. math::
   :label: eq_number_concentration_of_diameter

   n_P = \frac{C_P}{\rlap{--} V_P \rho_P} = \frac{6}{\pi \bar{d_P}^3} \frac{C_P}{\rho_P}

| Where:
| :math:`C_P` is the particle concentration
| :math:`\rlap{--} V_P` is the volume of a single particle
| :math:`\rho_P` is the particle density
| :math:`\bar{d_P}` is the average particle diameter

Equations :eq:`eq_spacing_of_number_concentration` and :eq:`eq_number_concentration_of_diameter` can be combined to obtain the relationship between separation distance and particle diameter.

.. math::
   :label: eq_spacing_of_diameter

    \bar \Lambda  = \frac{1}{n_P^{\frac{1}{3}}} =  \bar{d_P} \left(\frac{\pi}{6}\frac{\rho_P}{C_P}\right)^{\frac{1}{3}}


.. _figure_Particle_separation:

.. figure:: Images/Particle_separation.png
   :width: 200px
   :align: center
   :alt: Particle separation

   The average particle separation distance is defined as the distance between centers of cubes that each contain the volume of the suspension occupied by a single particle.

Particle separation distance matters because it determines which transport mechanisms are at play when two particles approach for a collision. The particle separation distance is a function of the particle concentration. Surface water treatment plants commonly treat water with turbidity between 1 and 1000 NTU. We will first find the number of clay particles per liter in typical raw water suspensions.

.. code:: python

    from aide_design.play import*
    from aguaclara_research.play import*
    import aguaclara_research.floc_model as fm
    C_Clay = np.arange(1,1000,1)*u.NTU
    n_Clay = fm.num_clay(C_Clay,fm.Clay)
    fig, ax = plt.subplots()
    ax.loglog(C_Clay.to(u.NTU),n_Clay.to(1/u.L))
    ax.set(xlabel='Clay concentration ($NTU$)', ylabel='Number of clay per liter')
    fig.savefig('Flocculation/Images/NClay_vs_CClay')
    plt.show()


.. _figure_NClay_vs_CClay:

.. figure:: Images/NClay_vs_CClay.png
   :width: 400px
   :align: center
   :alt: NClay vs CClay

   Diagram of number of clay particles per liter as a function of the clay concentration. Note that even 1 NTU water has millions of primary particles per liter.

The next step is to calculate the separation distance between the clay particles over this range of clay concentrations using equation :eq:`eq_spacing_of_diameter`.

.. code:: python

    from aide_design.play import*
    from aguaclara_research.play import*
    import aguaclara_research.floc_model as fm
    lamda_Clay = fm.sep_dist_clay(C_Clay,fm.Clay)
    fig, ax = plt.subplots()
    ax.semilogx(C_Clay.to(u.NTU),lamda_Clay.to(u.mm))
    ax.set(xlabel='Clay concentration ($NTU$)', ylabel=r'Clay separation distance ($mm$)')
    fig.savefig('Flocculation/Images/LambdaClay_vs_CClay')
    plt.show()


.. _figure_LambdaClay_vs_CClay:

.. figure:: Images/LambdaClay_vs_CClay.png
   :width: 400px
   :align: center
   :alt: LambdaClay vs CClay

   The clay separation distance varies with the cube root of the concentration and thus varies over a relatively narrow range (0.07 mm to 0.7 mm) while the turbidity varies from 1 to 1000 NTU.

Given this range of particle separation distances the next question is whether transport of these particles relative to each other is driven by inertial or viscous dominated processes. Turbulent eddies devolve into smaller and smaller eddies until viscosity finally kills them. Viscosity damps out the effects of inertia at the inner viscous length scale.  Higher intensity turbulence can generate more energetic small eddies and can resist the effects of viscosity longer. Thus the inner viscous length scale decreases as the turbulent energy dissipation rate increases.

The Camp-Stein velocity gradient used for flocculators varies from about 20 to 300 Hz. We will convert the Camp-Stein velocity gradient to an energy dissipation rate using

.. math::

   G_{CS} = \sqrt{\frac{\bar \varepsilon}{\nu}}

Solving for the average energy dissipation rate, :math:`\bar \varepsilon`, we obtain

.. math::

   \bar \varepsilon = \nu G_{CS}^2

We will use the inner viscous length scale, equation :eq:`eq_inner_viscous_length` to determine whether viscous or inertial transport dominates particle collisions in surface water treatment given the range of particle separation distances (see :numref:`figure_LambdaClay_vs_CClay`).

.. code:: python

    from aide_design.play import*
    from aguaclara_research.play import*
    import aguaclara_research.floc_model as fm
    Temperature = 20 * u.degC
    G=np.arange(1,1000,1)*u.Hz
    EDR = G**2 * pc.viscosity_kinematic(Temperature)
    Inner_viscous = fm.lambda_vel(EDR, Temperature)
    fig, ax = plt.subplots()
    ax.semilogx(G.to(u.Hz),Inner_viscous.to(u.mm))
    ax.set(xlabel='Velocity gradient (Hz)', ylabel='Inner viscous length scale (mm)')
    ax.text(10, 30, 'Eddies cause mixing', fontsize=12,rotation=-30)
    ax.text(3, 14, 'Viscous shear', fontsize=12,rotation=-30)
    fig.savefig('Flocculation/Images/innerviscous_vs_G')
    plt.show()


.. _figure_innerviscous_vs_G:

.. figure:: Images/innerviscous_vs_G.png
   :width: 400px
   :align: center
   :alt: inner viscous vs G

   The inner viscous length scale is approximately 3 to 10 mm for velocity gradients that are typically used in flocculators. Clay separation distances are smaller than the inner viscous length scale and thus viscous shear dominates particle collisions in flocculation.

By comparing :numref:`figure_LambdaClay_vs_CClay` and :numref:`figure_innerviscous_vs_G` it is apparent that the particle separation distances commonly found in surface water treatment plants are much smaller than the inner viscous length scale for all practical flocculation velocity gradients. Thus viscosity will dominate the flocculation process. This key insight reveals why turbulent flow flocculators have been designed using the dimensionless grouping :math:`G \theta` which is fundamentally :math:`\sqrt\frac{\epsilon}{\nu} \theta`. Given that flocculation is viscous dominated implies that the flocculation process will slow down as the temperature increases and the viscosity increases.

Collision time estimate
-----------------------

Now that we know that the collisions are controlled by viscosity we can begin formulating a model that describes the long distance random walk. The long range transport is assumed to be the rate limiting step. We model a system of two particles where one particle is held fixed and we observe the second particle's random motion. It may be helpful to visualize this by playing the video inside your mind in reverse starting from the moment of the collision. That way you know which two particles to follow! The random walk is illustrated in the video in :numref:`figure_Random_walk`.

.. _figure_Random_walk:

.. figure:: Images/Random_walk.png
   :target: https://youtu.be/I9sEOJ-kB3A
   :width: 400px
   :align: center
   :alt: Random walk toward a collision

   The red volume represents the potential end zone of the random walk that will slide into a collision with a short straight slow walk. The wandering particle sweeps through a volume of water equal to the volume occupied by a single particle.


.. _figure_Final_approach:

.. figure:: Images/Final_approach.png
   :target: https://youtu.be/BtG-IxCGAUk
   :width: 400px
   :align: center
   :alt: Final approach to a collision

   The final approach is the slow, straight path to the collision.

The volume cleared by the wandering particle is proportional to the area defined by a circle with diameter = sum of the particle diameters. This is because the wandering particle with strike the stationary particle if the wandering particle's center is anywhere within a diameter of the center of the stationary particle.

.. math:: {\rlap{\kern.08em--}V_{\rm{Cleared}}} \propto \pi \bar d_P^2

The volume cleared is proportional to time

.. math:: {\rlap{\kern.08em--}V_{\rm{Cleared}}} \propto t

The volume cleared is proportional to the relative velocity between the two particles.

.. math:: {\rlap{\kern.08em--}V_{\rm{Cleared}}} \propto \bar v_r

We use dimensional analysis to get a relative velocity for the long range transport controlled by shear. The relative velocity between the two particles that will eventually collide is assumed to be proportional to the average distance between the two particles.

The assumption that the relative velocity scales with the average distance between clay particles leads to the following steps. The first step is just a proposed functional relationship. We could also have jumped to the assumption that the relative velocity is a function of the length scale and the velocity gradient.

.. math:: \bar v_r = f \left( \bar \varepsilon ,\nu ,\bar \Lambda \right)

In a uniform shear environment the velocity gradient is linear. Thus the relative velocity must be proportional to the length scale.

.. math:: \bar v_r = \bar \Lambda f \left( \bar \varepsilon ,\nu \right)

The only way to for :math:`\bar \varepsilon` and :math:`\nu` to produce dimensions of time is to combine to create :math:`1/\bar G_{CS}`.

.. math:: \bar v_r \approx \bar \Lambda \bar G_{CS}

The volume cleared, :math:`{\rlap{\kern.08em--}V_{\rm{Cleared}}}` must equal the volume occupied by one particle, :math:`{\rlap{\kern.08em--}V_{\rm{Surround}}}` for a collision to occur. Combining the three equations for :math:`{\rlap{\kern.08em--}V_{\rm{Cleared}}}` and the equation for :math:`v_r` we obtain the volume cleared as a function of time.

.. math::
   \bar \Lambda^3 = {\rlap{\kern.08em--}V_{\rm{Surround}}} = {\rlap{\kern.08em--}V_{\rm{Cleared}}} \approx \pi \bar d_P^2 \bar \Lambda \bar G_{CS} \bar t_c

| Where:
| :math:`\bar t_c` is the average time required for a collision between two particles

Solving for the collision time we obtain

.. math::
   :label: tc

   \bar t_c \approx \frac{\bar \Lambda^2}{\pi \bar d_P^2 \bar G_{CS}}

In summary, a relationship for the mean time between collisions :math:`\bar{t_{c}}` was found by proposing an average condition for a collision, successful or unsuccessful, to occur. To define this condition, it was assumed that each primary particle on average occupies a fraction of the reactor volume, :math:`\bar{V}_{Surround}`, inversely proportional to the number concentration of particles. Furthermore, prior to a collision, a particle on average sweeps a volume, :math:`\bar{V}_{Cleared}`, proportional to :math:`\bar{t_c}` and to the mean relative velocity between approaching particles, :math:`\bar{v}_r`. As an average condition, it was posited that for each collision, :math:`\bar{V}_{Cleared}` must equal :math:`\bar{V}_{Surround}`. From this, a relationship for a characteristic collision time, :math:`\bar{t_c}`, was obtained:

.. _heading_Collision_Rates:

Collision Rates
---------------

The change in the number of successful collisions (from a single particle's perspective) with respect to time is equal to the mean probability that a collision will result in an attachment, :math:`\bar{\alpha}`, divided by time for one collision, :math:`\bar t_c`.

.. math::
   :label: dNc

	 \frac{dN_c}{dt}=\frac{\bar{\alpha}}{\bar{t_{c}}},


| Where
| :math:`\frac{dN_c}{dt}` is the rate of successful collisions between primary particles,
| :math:`\bar{\alpha}` is the mean probability that a collision will result in an attachment,
| :math:`\bar{t_{c}}` is the mean time between collisions of primary particles.

The probability that two primary particles attach is expected to be equal to the probability that at least one of the colliding particles has a precipitated coagulant nanoparticle at the initial contact point. It is simpler to derive the probability of attachment from the probability that neither particle has a coagulant nanoparticle at the point where the two particles collide, since the probability of a successful collision includes the probabilities of one particle and of both particles having a coagulant precipitate. The probability of one particle colliding at a point without a coagulant precipitate is :math:`(1-\bar{\Gamma})`, so the probability of neither particle having a coagulant precipitate at the point of collision is :math:`(1-\bar{\Gamma})^2`. As this is the probability of a failed collision, the probability of a successful collision is :math:`1-(1-\bar{\Gamma})^2`.

Since the model assumes an initially monodisperse population of primary particles and that collisions between differently-sized particles are unfavorable, differential sedimentation is considered negligible.  Brownian motion is only significant for particles smaller than 1 :math:`\mu m` :cite:`Floc_Model-benjamin_water_2013`, and so this model makes the assumption that primary particles are larger than 1 :math:`\mu m`.

The collision rate :cite:`Floc_Model-pennock_theoretical_2016` can be obtained by substituting equation :eq:`tc` into equation :eq:`dNc`.

.. math::
   :label: Nclam

	  \frac{dN_{c}}{dt}=\pi\bar{\alpha}\frac{\bar{d}_{P}^2}{\bar \Lambda^2} \bar G_{CS}


where :math:`\bar G_{CS}` is the Camp Stein velocity gradient.

Because the flocculation performance equation will ultimately track particle concentration, the concentration of primary particles, :math:`C_{P}`, was substituted for :math:`\bar \Lambda` using

.. math::
   :label: Ld

	 \bar \Lambda^3=\frac{\pi}{6}\frac{\rho_{P}}{C_{P}}\bar{d}_P^3,


where :math:`\rho_{P}` is the characteristic density of primary particles. Equation :eq:`Ld` can be substituted into Equation :eq:`Nclam` to result in:

.. math::
   :label: Ncld

   dN_{c}=\pi\bar{\alpha}\left(\frac{6}{\pi}\frac{C_{P}}{\rho_P}\right)^{2/3}\bar G_{CS}dt.



Equation :eq:`Ncld` reveals that :math:`\frac{dN_c}{dt}` increases with :math:`C_P` and :math:`\bar{\Gamma}`. During flocculation
:math:`C_P` will decrease and thus :math:`\frac{dN_c}{dt}` will also decrease.

Coagulant nanoparticle and primary particle Model
-------------------------------------------------

Continuing from :cite:`Floc_Model-pennock_theoretical_2016`, the above Lagrangian differential relationships are further developed to become integrated performance prediction equations. Equation :eq:`Ncld` cannot be integrated as written because the concentration of primary particles is expected to change with each collision, and thus that relationship must be specified. During the average time required for one collision it is expected that approximately :math:`e^{-1}` of the particles will undergo at least one collision. The time required for a collision will change as flocculation proceeds as the average distance between primary particles increases. The rate of loss of primary particles due to successful collisions will be first order with respect to the number of successful collisions.

.. math::
   :label: dCP

	 \frac{dC_{P}}{dN_{c}}=-kC_{P},


where :math:`k` is an experimentally-derived constant that physically represents the portion of the primary particles that become settleable particles on average after each collision time, :math:`\bar{t_c}`, and will depend, in part, upon the design capture velocity, :math:`v_c`, used for sedimentation. Since :math:`\bar{t_c}` increases over time as :math:`\bar \Lambda` increases, the above formulation is not proportional to :math:`\frac{dC_P}{dt}`. Physically, Equation :eq:`dCP` states that, with each progressive primary particle collision, :math:`C_P` decreases by some proportion. Further, Equation :eq:`dCP` states that this decrease is directly proportional to :math:`C_P`. With each successive successful collision, the absolute reduction in :math:`C_P` is less than the prior one. The value of :math:`k` is expected to be less than 1, because not all primary particles will have a collision and grow to a size with a sedimentation velocity greater than :math:`v_c` in the average time required for a collision.

Having Equation :eq:`dCP`, the next step is to substitute it into Equation :eq:`Ncld` and integrate. Solving Equation :eq:`dCP` for :math:`dN_{c}`, substituting it into Equation :eq:`Ncld` and rewriting the equations in terms of primary particles results in Equation :eq:`dCPlam`,

.. math::
   :label: dCPlam

	 \frac{dC_{P}}{-kC_{P}}=\pi\bar{\alpha}\left(\frac{6}{\pi}\frac{C_{P}}{\rho_P}\right)^{2/3}\bar G_{CS}dt,


It is interesting to note that rearranging Equation :eq:`dCPlam` in terms of :math:`\frac{dC_P}{dt}` gives a :math:`C_P` exponent of :math:`\frac{5}{3}`. Previous flocculation rate equations were second-order, but the observed flocculation rate was less than second-order :cite:`Floc_Model-benjamin_water_2013`. The slight deviation from an exponent of two comes from the assumption of :cite:`Floc_Model-pennock_theoretical_2016` that relative velocity between colliding particles scales with :math:`\Lambda` rather than :math:`d_P`. This is to say that, in dilute suspensions characteristic of raw water, where particles are separated by :math:`\bar \Lambda\gg \bar{d}_P`, the majority of :math:`\bar{t_c}` is spent with the distance between particles characterized by :math:`\bar \Lambda` instead of :math:`\bar{d}_P`. The time required for the final approach for a collision is hypothesized to be insignificant compared the time for :math:`\bar{V}_Cleared` to equal :math:`\bar{V}_Surround`.

From Equation :eq:`dCPlam` it is possible to integrate and obtain equations for flocculation performance. After separation of variables, one side of the equation is integrated with respect to time from the initial time (:math:`t=0`) to the time of interest, generally taken to be the mean hydraulic residence time (:math:`t=\theta`). The other side of the equation is integrated with respect to the concentration of primary particles from the value at the initial time (:math:`C_{P_0}`), equivalent to the initial concentration of primary particles, to the concentration of primary particles at the time of interest (:math:`C_{P}`). The integral becomes:

.. math::
   :label: intdCPlam

	 \frac{1}{\pi}\left(\rho_{P}\frac{\pi}{6}\right)^{2/3}\int_{C_{P_0}}^{C_{P}}C_{P}^{-5/3}dC_{P}=-k\bar{\alpha}\bar G_{CS}\int_0^\theta dt.


The integral on the left hand side assumes that :math:`\rho_{P}` does not change as :math:`C_P` changes. One assumption on the right side is that :math:`\bar{\Gamma}`, of which :math:`\bar{\alpha}` is a function, does not vary with :math:`t`. This requires that adsorption of coagulant to colloidal particles in rapid mix be fast enough to be approximated as completed by the beginning of flocculation. This assumption may not be valid for high rate flocculators especially under conditions of low :math:`C_{P_0}`. Further work on the rate and efficacy of coagulant nanoparticle attachment to primary particle surfaces is needed.

The other assumption on the right hand side is that the mean velocity gradient, :math:`\bar G_{CS}`, does not change over the course of the flocculation process. In mechanically-mixed flocculators, the use of a simple spatial average is not reasonable, as the velocity gradient changes dramatically from the bulk flow to the tip of the impeller blade and individual particles follow different paths that expose them to different velocity gradient zones in different sequences and durations :cite:`Floc_Model-boller_particles_1998`. The distribution of residence times in a mechanical flocculator would also need to be taken into account for the integration. For baffled hydraulic flocculators, on the other hand, the use of the spatial average, :math:`\bar G_{CS}`, and considering it constant with :math:`t` is generally a reasonable approximation, as mixing energy in a well-designed hydraulic flocculator is rather uniformly distributed spatially, the zones of higher energy dissipation rate after the baffles do not vary appreciably with time when operating at a constant flow rate, and all particles have similar residence times in the flocculator.

Integration of Equation :eq:`intdCPlam` gives:

.. math::
   :label: CPlamint

	 \frac{3}{2\pi}\left(\rho_{P}\frac{\pi}{6}\right)^{2/3}\left(C_{P}^{-2/3}-C_{P_0}^{-2/3}\right)=k\bar{\alpha}\bar G_{CS}\theta.


This can be put in terms of :math:`\bar \Lambda` for simplicity by using Equation :eq:`Ld` and rearranging in terms of the familiar Camp-Stein parameter, :math:`\bar G_{CS}\theta`, to be

.. math::
   :label: Gtlam

	 \bar G_{CS}\theta = \frac{3}{2}\frac{{\left( {{\bar \Lambda ^2} - \bar \Lambda _0^2} \right)}}{{k\pi\bar{\alpha} \bar{d}_P^2}}.


Equation :eq:`Gtlam` gives guidance for flocculator design in that higher values of :math:`\bar G_{CS}\theta` are needed for flocculators to achieve greater changes in :math:`\bar \Lambda` (or :math:`C_P`) or to overcome low :math:`\bar{\Gamma}`. It should be noted that the :math:`\bar \Lambda_0` term in Equation :eq:`Gtlam` will generally be very small compared to the :math:`\bar \Lambda` term for most flocculation scenarios. In this case the initial particle separation distance, :math:`\bar \Lambda_0` can be considered negligible. While simplifying the equation, this also gives the result that **flocculators must be designed** not so much for the particle concentrations they will receive but **for the particle concentrations they are intended to produce**.

Modifying Equation :eq:`Gtlam` to be in terms of :math:`C_P` produces:

.. math::
   :label: GtlamSim

	 \bar G_{CS}\theta = \frac{3}{2k\pi\bar{\alpha}}\left(\frac{\pi}{6}\frac{\rho_P}{C_P}\right)^{2/3}.


A desirable way to represent flocculation performance is with the negative log of the fraction of particles remaining (also often referred to as log removal), :math:`pC^*`, given in :cite:`Floc_Model-swetland_flocculation-sedimentation_2014` as:

.. math::
   :label: pC

	 p{C^*}=-\log_{10}\left(\frac{C_{P}}{C_{P_0}}\right)


Likewise, a way to simplify Equation :eq:`CPlamint` is to put it in terms of the particle volume fraction, :math:`\phi`, defined as:

.. math::
   :label: phi

	 \phi=\frac{C_P}{\rho_P}=\frac{\pi}{6}\left(\frac{\bar{d}_P}{\bar \Lambda}\right)^3.


Putting Equation :eq:`CPlamint` in terms of :math:`pC^*` and
:math:`\phi` results in:

.. math::
   :label: pClam

	 p{C^*}=\frac{3}{2}\log_{10}\left[\frac{2}{3}\left(\frac{6}{\pi}\right)^{2/3}k\pi\bar{\alpha}\bar G_{CS}\theta\phi_0^{2/3}+1\right].


Equation :eq:`pClam` is a predictive performance model for flocculation in flows with long range particle transport toward collisions dominated by viscous forces. It is proposed as applicable to both laminar and turbulent hydraulic flocculators. Given the properties of the flocculator (:math:`\bar G_{CS}` and :math:`\theta`) and its influent (:math:`\phi_0` and :math:`\bar{\alpha}`), flocculation performance can be predicted in terms of :math:`pC^*`. The development of Equation :eq:`pClam` was the result of a team effort of Cornell University's AguaClara program and hence it will be subsequently referred to as the AguaClara flocculation model.


Experimental Protocols
----------------------

Equation :eq:`pClam` was tested under turbulent conditions. The design scheme chosen to meet these requirements was a tube flocculator, illustrated in :numref:`figure_apparatus` and described in :cite:`Floc_Model-pennock_theoretical_2016`. This tube flocculator operated in the turbulent flow regime, which for pipe flow means that :math:`Re>4,000` :cite:`Floc_Model-granger_fluid_1995`. The change in mean energy dissipation rate due to any modification to the system was approximated by

.. math::
   :label: EDR

	 \bar{\varepsilon}=\frac{gh_\ell}{\theta},


where :math:`g` is the acceleration due to gravitational force and :math:`h_\ell` is the head loss across the flocculator. As mentioned previously, the use of :math:`\bar{\varepsilon}` assumes that the energy dissipation rate throughout the flocculator is completely uniform so that it can be represented with a simple spatial average rather than a weighted average accounting for the proportion of the flow passing through different zones of energy dissipation rate. This approximation requires that the majority of energy dissipation (represented by head loss) is due to fluid shear (minor loss) in the bulk flow. If the head loss across a flocculator were primarily as a result of shear on the reactor walls (major loss), only a small fraction of the flow would experience this energy dissipation rate in the near-wall zone, and estimating the mean energy dissipation rate by this method would be invalid.

It is hypothesized, however, that the constrictions in the tube flocculator created submerged free jets downstream, generating fluid shear across the cross section of the flow :cite:`Floc_Model-pennock_theoretical_2016`. This hypothesis is supported by a calculation of the head loss due to wall shear using the Darcy-Weisbach equation :cite:`Floc_Model-granger_fluid_1995`. The turbulent tube flocculator would be expected to have a total head loss of around 7 cm if only wall shear were present, but an average head loss of 90 cm was measured across the flocculator by means of a differential pressure sensor, indicating that significant fluid shear is present.

Referring to Equation :eq:`EDR`, changing the head loss by changing the constriction of the tubes or changing the water elevation difference across the flocculator would change the energy dissipation rate. Likewise, either of the above two modifications would change the mean hydraulic residence time in the flocculator. This could also be accomplished by changing the length of the flocculator.

.. _figure_apparatus:

.. figure:: Images/PennockFig1.png
   :width: 400px
   :align: center
   :alt: Experimental apparatus

   Diagram of Turbulent Tube Flocculator adapted from :cite:`Floc_Model-pennock_theoretical_2016` with modifications made to the outlet weir system and the addition of strong base solution.



:numref:`figure_apparatus` illustrates the process sequence used in this study. At the beginning of the process, tap water from the Cornell University Water Filtration Plant came into the system with, on average, a pH of 7.67, a turbidity of 0.056 nephelometric turbidity units (NTU), a total hardness of 150 mg/L, a total alkalinity of 140 mg/L, and a dissolved organic carbon (DOC) concentration of 1.80 mg/L :cite:`Floc_Model-bp-mws_drinking_2016`. This water was temperature-controlled by means of a PID (proportional-integral-derivative) controller, which regulated the relative fractions of hot water and cold water used to maintain the level in the constant head tank. The temperature-controlled water was passed through a granular activated carbon (GAC) filter to reduce the effect of dissolved organic matter (DOM) on experimental results. The water was then sent to the constant head tank, where it was bubbled with air to strip out supersaturated dissolved gases that might come out of solution during the experiment, resulting in formation of bubbles.

From the constant head tank, this conditioned water was delivered to the turbulent tube flocculator. Before entry to the flocculator, the water was set at a constant primary particle concentration by means of a computer-controlled peristaltic pump that introduced a concentrated kaolinite clay suspension (R.T. Vanderbilt Co., Inc., Norwalk, Connecticut) of about 250 g/L. A fraction of the mixed flow was sampled by a peristaltic pump and analyzed for turbidity with an HF Scientific MicroTOL turbidimeter at a distance of greater than ten diameters downstream from the clay input and then reintroduced at the point where clay suspension was added. This turbidity reading was input into a PID control system which determined the speed of the clay pump according to the discrepancy between the influent turbidity and the experimental target value.

Along with the clay, strong base (NaOH) manufactured by Sigma-Aldrich (St. Louis, MO) was added upstream of the flocculator with a peristaltic pump to keep the pH of the water at :math:`7.5\pm0.5`, which was the criterion set for the pH in these experiments. In the winter, the pH of the tap water dropped close to 7, and so sufficient NaOH was added to account for seasonal variations in the natural base-neutralizing capacity (BNC) of the water and to raise the pH above 7 to around 7.5.  This base addition was also sufficient to neutralize the acidity of the polyaluminum chloride (PACl) coagulant used for this study, which had been found to impact the solubility of PACl at high doses. Base doses were calculated to account for the normality of the PACl solution, based on a titration which found that the PACl solution was approximately 0.025 equivalents of strong acid per gram as Al.

Just prior to entering the flocculator,  PACl coagulant (PCH-180) manufactured by the Holland Company, Inc. (Adams, Massachusetts) was added to the flow by a computer-controlled peristaltic pump which varied the coagulant dose between experiments. After entering the system, the coagulant then entered a small orifice used to accomplish rapid mix by forming a jet downstream. From there, the suspension traveled up through the flocculator made of 3.18 cm (1.25 in) inner diameter tubing. Within the flocculator, the fluid passed through constrictions in the tubing that caused the flow to contract, resulting in flow expansions afterward and achieving increased mixing and energy dissipation.

After leaving the flocculator, the flow passed a vertical tube with a free surface that served as an air release. This removed bubbles in the system so that they would not interfere with settling or analysis of the flocs. A portion of the flow was then diverted for sedimentation by means of a peristaltic pump up a clear one-inch PVC pipe angled at :math:`60^{\circ}`. The flow rate through the pump was selected based on the dimensions of the tube and its angle to achieve a desired capture velocity, :math:`v_c`. The supernatant from this tube settler was passed through an HF Scientific MicroTOL nephelometric turbidimeter to record the effluent turbidity for the duration of the experiment. Recording the settled effluent turbidity made it possible to calculate the :math:`pC^*` term in Equations :eq:`pClam` (in terms of primary particles) and also made possible comparison with data from :cite:`Floc_Model-swetland_flocculation-sedimentation_2014`.

After data from the settled flocs had been collected, the flow from the effluent turbidimeter was sent to the drain along with the bulk flow. The bulk flow traveled past a second air release before exiting the drain. The air release gave the flow exiting the drain a free surface as it flowed over the exit weir so that the exiting water developed into a supercritical flow. Thus, the flow over the weir was not influenced by the flow downstream of the free surface, and the flow rate could be controlled by adjusting the elevation of the free surface before the drain. The outlet weir was a 1-1/4" PVC pipe within an upright 3" clear pipe, which were joined by a flexible coupling adapter. The effluent water accumulated in the clear outer pipe until it reached the elevation of the top of the inner pipe and flowed down through it. The flow rate could be adjusted by loosening the flexible coupling so that the elevation of the top of the inner pipe could be adjusted. As the bulk flow exited down out of the inner pipe to the drain, it passed over a glass electrode sensor to
measure pH.

Results
-------

The above process was used to conduct the experiments to test the applicability of Equation :eq:`pClam` in turbulent flocculation. The influent turbidity was set at a constant of 900 NTU. The mean energy dissipation rate was about 21.5 mW/kg, which resulted from choosing a flow rate of about 110 mL/s so that the Reynolds number was just above 4,000. These values were chosen to ensure viscous-dominated turbulent initial conditions. For these experiments, coagulant doses ranged from 0.05 to 98 mg/L as Al. A :math:`v_c` of 0.12 mm/s was used for all experiments. Data from these nominally viscous experiments are shown in :numref:`figure_PennockFig2` as a function of coagulant dose.


.. _figure_PennockFig2:

.. figure:: Images/PennockFig2.png
   :width: 400px
   :align: center
   :alt: internal figure

   Effluent turbidity as a function of coagulant dose for experiments performed with influent turbidity of 900 NTU, velocity gradient of 147/s, and hydraulic residence time of about 413 s.


The data shown in :numref:`figure_PennockFig2` were compared with the viscous model, as shown in :numref:`figure_PennockFig3`.
In this graph, the data are plotted in terms of Equation :eq:`pClam` and its corresponding composite parameter taken from Equation :eq:`Nclam`,

.. math::
   :label: Paramlam

	 N_{c}\propto\bar{\alpha}\theta \bar G_{CS}\phi_0^{2/3}.

.. _figure_PennockFig3:

.. figure:: Images/PennockFig3.png
   :width: 400px
   :align: center
   :alt: internal figure

   Fit of Equation :eq:`pClam` to data from :math:`Re\approx 4,000` experiments. Hollow points indicate data not used in fitting the model.

At the highest values, however, a marked decrease begins. For these graphs, the model fits were done for all points where increasing performance was seen, because the model does not currently include a mechanism for the decreasing performance. The values for :math:`k` were determined by the Levenberg-Marquardt algorithm, and the value for the model was 0.030. The :math:`R^2` value for the fit is 0.958 and the sum of squared errors is 0.228 (mean pC* error of 0.128).

From the values given previously, the ratio :math:`\frac{\bar \Lambda_0}{\bar{\eta}}` can be calculated for the experimental conditions. Equation :eq:`Ld` can be used to compute (:math:`\bar \Lambda_0`). For these experiments, :math:`\bar{d}_P` is taken to be the average diameter of kaolinite clay particles, found by :cite:`Floc_Model-wei_coagulation_2015` and :cite:`Floc_Model-sun_characterization_2015` to be 7 :math:`\mu m`. The concentration can be converted from NTU to the necessary mass/volume (mg/L) unit by using as a proportion the measurement reported by :cite:`Floc_Model-wei_coagulation_2015` of 68 NTU for 100 mg/L of kaolinite clay. Last, the density was assumed to be 2.65 g/:math:`cm^3` for kaolinite.

For flocculation in laminar flows, data were used from the work of :cite:`Floc_Model-swetland_flocculation-sedimentation_2014`. :numref:`figure_PennockFig5` shows Equation :eq:`pClam` fit to results for a capture velocity of 0.12 mm/s at two hydraulic residence times, five influent turbidity values and a range of coagulant doses. :cite:`Floc_Model-swetland_flocculation-sedimentation_2014` showed that the projected x-axis intercept of the linear region of the data (with a log-log slope of 1 according to her plotting of the data) was proportional to the capture velocity used for sedimentation. Correspondingly, :math:`k` is expected to be a function of capture velocity.

.. _figure_PennockFig5:

.. figure:: Images/PennockFig5.png
   :width: 400px
   :align: center
   :alt: internal figure

   Fit of Equation :eq:`pClam` to laminar flocculation data from :cite:`Floc_Model-swetland_flocculation-sedimentation_2014`.


Referring to :numref:`figure_PennockFig5`, Equation :eq:`pClam` fits the data from :cite:`Floc_Model-swetland_flocculation-sedimentation_2014` well with a :math:`k` value of 0.027. The resulting :math:`R^2` for this fit is 0.844. The sum-squared error is 5.03, giving an average pC* error of 0.034 for the fit.

Discussion
----------

The goodness of fit seen in :numref:`figure_PennockFig3` and :numref:`figure_PennockFig5` indicate that the model captures the important mechanisms governing flocculation performance for a wide range of coagulant doses in both laminar and turbulent hydraulic flocculation. One of the challenges in fitting the data pertained to the assumption made for the characteristic diameter of PACl precipitate clusters, :math:`\bar{d}_C`. This value has significant influence on the value of :math:`\bar{\Gamma}`, which in turn influences the values of the composite parameter (Equation :eq:`Paramlam`).

It is known that PACl contains aluminum monomers and oligomers as well as :math:`\mathrm{Al_{13}}` and :math:`\mathrm{Al_{30}}` nanoclusters, with the larger :math:`\mathrm{Al_{30}}` nanoclusters having a diameter of 1 nm and a length of 2 nm :cite:`Floc_Model-mertens_polyaluminum_2012`. It has been found, however, that the components of PACl self-aggregate and go on to form larger clusters :cite:`Floc_Model-swetland_influence_2013`. For these experiments, the value of :math:`\bar{d}_\mathrm{C}` was chosen based on sizing experiments performed by Garland (2015) with a Malvern Zetasizer Nano-ZS to analyze a 138.5 mg/L (as Aluminum) solution of PACl.

A limitation of the model can be seen in the data in :numref:`figure_PennockFig3` at higher values of the composite parameters. After increasing steadily for all of the preceding range of coagulant doses, the performance began to decline after the dose of 10.9 mg/L as Aluminum. A simple hypothesis for the decline in performance (which corresponds with an effluent turbidity increase over the five data points from 2.7 NTU to 11.1 NTU) is that an increase in free PACl nanoparticles made a significant contribution to the effluent turbidity. As the PACl concentration increased, the coverage of reactor and clay platelet surfaces by coagulant became more complete and the free coagulant concentration also increased. With very high coagulant doses like the ones used in the upper end of the experimental range, it is possible that the formation of PACl self-aggregates was favorable, increasing the turbidity of the suspension. Indeed, calculation of the volume fraction for the 10.9 mg/L experimental PACl dose gives a volume fraction value (for clay and coagulant combined) of :math:`6.1\times10^{-4}`, while for the highest dose of 98 mg/L as Al, the value was :math:`8.3\times10^{-4}`, a 37\% increase due solely to the increased contribution of PACl precipitates.

Another possibility is that as :math:`\bar{\Gamma}` increases above 0.5, the resulting flocs are increasingly formed by PACl-PACl bonds instead of by PACl-kaolinite bonds. If the PACl-PACl bonds are weaker than PACl-kaolinite bonds, it is possible that attachment efficiency decreases for high :math:`\bar{\Gamma}`. The weakness of PACl-PACl bonds compared with PACl-kaolinite bonds is suggested by the relative charges of PACl and kaolinite. While PACl precipitate surfaces are positively charged, the surfaces of kaolinite are mostly negatively charged :cite:`Floc_Model-wei_coagulation_2015`. Therefore, it follows that PACl precipitates will likely have more affinity for kaolinite surfaces than for other PACl precipitates. The :math:`\bar{\Gamma}` calculated for the peak performance was 0.52, and so it is possible that performance decreased past this point because the strength of bonds for experiments at higher doses were weaker.

Applying the AguaClara flocculation model to the design of a hydraulic flocculator indeed gives reasonable results. Assuming that a flocculator is expected to receive sufficiently high turbidities that the influent concentration can be neglected, Equation :eq:`GtlamSim` can be used. In order for it to treat to a settled effluent of 3 NTU (pre-filtration) with sufficient PACl to achieve a surface area coverage fraction of 0.5, it would need to have a :math:`\bar G_{CS}\theta` of 99,600. :cite:`Floc_Model-davis_introduction_2008` give the range of  :math:`\bar G_{CS}\theta` values pertinent to flocculation of high turbidities as between 36,000 and 96,000, so this result is reasonable. This analysis does not account for removal of particles in a floc blanket that would enable use of a lower value of :math:`\bar G_{CS}\theta`.

Regarding flocculator design, recommended values of :math:`\bar G_{CS}` in flocculation range from :math:`10\:\mathrm{\frac{1}{s}}` to :math:`100\:\mathrm{\frac{1}{s}}`, which correspond to :math:`\bar{\varepsilon}` values of about 0.1 to 10 mW/kg :cite:`Floc_Model-mcconnachie_design_2000`. However, there is evidence that higher velocity gradients are advantageous, as found by :cite:`Floc_Model-garland_revisiting_2016` as well as the work done in this study, which made use of energy dissipation rates of about 22 mW/kg. For hydraulic flocculators, at least, designers should consider using higher energy dissipation rates than conventionally used, since they have a much lower ratio of maximum to average energy dissipation rate, leading to less floc breakup at high energy dissipation rates compared to mechanically mixed flocculators.

The assumption that nonsettleable particle removal is proportional to primary particle removal appears to be supported by the goodness of fit supplied by the AguaClara  flocculation model to the data (see :numref:`figure_PennockFig3`). This assumption is likely included in the values of :math:`k` fit by the model. A mechanistic understanding of :math:`k` will require that the proportionality between nonsettleable and primary particles be understood explicitly. It is possible that :math:`k` is a function of rapid mix effectiveness, and since :math:`k` predicts :math:`pC^*`, it will also be dependent on :math:`v_c`. Future experiments at varying :math:`v_c` are planned. Currently, :math:`\bar{\alpha}` is calculated assuming that coagulant nanoparticle attachment to the primary particles was accomplished very early on in the flocculator, but if colloid coating by coagulant nanoparticles is dependent upon diffusion rather than exclusively on hydraulic shear, it will be a function of time in addition to :math:`\bar G_{CS}\theta`, making flocculation less effective at high flow rates. Additionally, the use of :math:`\bar{\varepsilon}` (or :math:`\bar G_{CS}`) assumes a uniform energy dissipation rate in the flocculator. Any spatial deviation in the laboratory flocculator from a uniform energy dissipation rate would have had an impact on the values of :math:`k` relative to their theoretical values, which are dictated by the rate of conversion of primary particles to flocs.

Summaries
---------

We developed a model that predicts hydraulic flocculator performance. Regardless of whether the flow is laminar or turbulent, viscous forces control the relative velocities between particles on a collision path, and the performance equation is :math:`pC^*=\frac{3}{2}\log_{10}\left[\frac{2}{3}\left(\frac{6}{\pi}\right)^{2/3}\pi k\bar{\alpha}\bar G_{CS}\theta\phi_0^{2/3}+1\right]`.

Model predictions were compared with data from :cite:`Floc_Model-swetland_flocculation-sedimentation_2014`. To validate the first equation and the second equation in turbulent flow, experiments were conducted in turbulent flow for initial conditions of :math:`\frac{\bar \Lambda}{\bar{\eta}}<1`. It was found that the viscous equation was slightly more suitable in these conditions. Until further work is done on delineating the relative predominance of viscous and inertial forces over the range of turbulent flocculation conditions, the authors recommend using the AguaClara flocculation model. For design purposes, this model indicates that flocculator design is more sensitive to the desired effluent concentration of particles than the range of influent concentrations that might be encountered. This study also supports the use of higher energy dissipation rates (or velocity gradients) than conventionally recommended for hydraulic flocculators. Further work is needed to characterize the functional dependence of :math:`k` on capture velocity and energy dissipation rate, as well as the relationship between the final concentrations of primary and primary
particles.


Geometric Explanation of the Effects of Humic Acid on Flocculation
==================================================================

Dissolved organic matter (DOM) is found in all surface and ground waters and has a significant effect on drinking water treatment, since the presence of DOM can create a need for increased coagulant doses in addition to being a precursor of disinfection byproducts (DBPs). This work evaluated use of polyaluminum chloride (PACl) as a coagulant for a synthetic surface water to determine the effect of DOM on the settled effluent turbidity. Mechanistically-based, scalable algorithms for operation of hydraulic flocculators were developed in this research based on observations of residual turbidity. Data were obtained using a laminar-flow tube flocculator and a lamellar tube settler. The research employed a flocculation model previously published by some of the authors and considered modifications to the model algorithm to incorporate the effects of humic acid. Two adjustable model parameters were used to fit data, one related to at incorporated the capture velocity used for sedimentation, and one that estimated the average size of dissolved humic acid molecules. The modified model that accounted for the presence of humic acid was able to independently predict the experimental results from 60 experiments at a different influent turbidity. The predictive model is expected to provide insights into the interactions between dissolved species and the coagulant nanoparticles.

Introduction
------------

The main objective of this research was to observe and model the effects of dissolved organic matter (DOM) on flocculation to enhance the performance of a hydraulic flocculator in the context of a process train with subsequent unit processes (i.e., sedimentation). Prior research has shown that multiple variables influence the performance of hydraulic flocculators in drinking water treatment, including the concentration and type of suspended particles in the raw water, the concentration of DOM, coagulant type and dose, and hydraulic residence time and energy dissipation rate in the flocculator (Kawamura, 1991).

The design and operation of hydraulic flocculators would be assisted by a predictive model that can characterize performance of flocculator designs. A general scalable model which uses dimensionally correct relationships that are based upon relevant flocculation mechanisms was created by Pennock et al. (2018) and successfully applied to quantify the effect of varying flocculator design and operational parameters on the post-sedimentation residual turbidity that corresponded to a selected sedimentation capture velocity. However, this model did not account for the presence of varying levels of DOM.

Previous researchers have hypothesized that DOM interacts with coagulants through various mechanisms. Jarvis and Jefferson (2007) state that the aggregation mechanisms through which DOM is removed include a combination of charge neutralization, entrapment, adsorption, and complexation with coagulant metal ions into insoluble particulate aggregates. Optimal conditions for turbidity or pathogen removal are not always the same as those for DOM removal (Hua and Reckhow, 2008). Because of the variable composition of DOM, the mechanisms of removal could be different for different types of DOM in water (Sharp and Jarvis, 2006). The hydrophobic fraction of DOM, which includes humic acids, is generally removed in coagulation more effectively than the hydrophilic fraction (Marhaba et al., 2003; Matilainen and Vepsalainen, 2010).

Prehydrolyzed polymer coagulants, such as polyaluminum chloride (PACl), have been reported to have advantages over conventional coagulants such as alum, including less temperature or pH dependence, as well as smaller alkalinity consumption, but the characteristics of the water to be treated (e.g., alkalinity, pH, and DOM content) play a major role in the choice of a proper coagulant. Consequently, prehydrolyzed coagulants have not been consistently observed to enhance the removal efficiency of DOM (Hu, 2006).

The research described in this paper builds on the AguaClara hydraulic flocculation model developed by Pennock et al. (2018) and adds detail to the attachment efficiency coefficient describing geometric and probabilistic interactions between clay, coagulant, DOM, and reactor walls. The synthetic raw water used in experiments added one type of DOM, humic acid, to a previously studied synthetic system (Swetland et al., 2014) with the expectation that the resulting system would be sufficiently well-characterized to develop a predictive model.

The AguaClara flocculation model is based on the observation that coagulant precipitates form nanoparticles that attach to the surfaces of suspended particles (clay) and reactor walls. Swetland et al. (2014) found particle attachment efficiency in a hydraulic flocculator to be proportional to the fractional surface coverage of suspended clay by precipitated coagulant (alum and PACl) nanoparticles. The success of the surface coverage model in explaining the interactions between clay, coagulant nanoparticles, and reactor walls led to the hypothesis that hydrophobic DOM macromolecules may attach to the coagulant nanoparticles and reduce the amount of PACl surface area that is available for attachment.

Experimental Protocols
----------------------

Experiments were conducted using the laboratory apparatus illustrated in :numref:`figure_Du_Fig1`. Cornell University tap water was pumped from an aerated and temperature-controlled reservoir and mixed with a concentrated stock suspension of kaolinite clay (R.T. Vanderbilt Co., Inc.) to form a feed-back regulated constant turbidity raw water source (Weber-Shirk, 2016).

.. _figure_Du_Fig1:

.. figure:: Images/Du_Fig1.png
   :width: 400px
   :align: center
   :alt: Experimental Apparatus

   Experimental System Schematic


Reported Cornell University tap water characteristics are listed in :numref:`table_CUWTP_Water_Quality`. A concentrated suspension of humic acids was mixed with the raw water source to produce humic acid concentrations ranging from 0 to 15 mg/L. The humic substances used in experiments were obtained in the form of sodium salt from Sigma-Aldrich (H16752).  Polyaluminum chloride (PACl) coagulant doses (Holland Company, Adams, MA) ranging from 0.53 to 2.65 mg/L as Al were used to treat the synthetic raw water.

.. _table_CUWTP_Water_Quality:

.. csv-table:: Average properties of tap water provided by Cornell University Water Filtration Plant as reported by BP-MWS, et al., 2016.
   :header: Property, Average Value
   :widths: 20, 20
   :align: center

   Turbidity, 0.056 NTU
   Total Hardness, 150 mg/L
   Total Alkalinity (as :math:`CaCO_3`), 140 mg/L
   Dissolved Organic Carbon (DOC), 1.80 mg/L

The coagulant dosage and humic acid concentrations were regulated by adjusting the rotation speed of separate peristaltic pumps. The pH of the treated effluent was monitored in each experiment and was 7.5 :math:`\pm` 0.3. Influent turbidities of 50 NTU and 100 NTU were tested.  Flocculation was accomplished by laminar flow through a coiled 9.52 mm inner diameter tube. The average velocity gradient in the coiled flocculator,:math:`\bar G_{CS}` was calculated according the equation derived by Tse et al. (2011) as

.. math::

   \bar G_{CS}= {{\bar G}_{CS_{Straight}}} \sqrt{1+{{0.033 \left[log\left(\frac{4Q_{Plant}}{\pi D\nu }\sqrt{\frac{D}{R_{c}}}\right)\right]\ }}^{4}}


where :math:`{\bar G_{CS_{Straight}}}` is fluid velocity gradient in a straight tube, :math:`Q_{Plant}` is the experimental flow rate, :math:`D` is the inner diameter of the flocculator tube, :math:`R_{c}` is the diameter of curvature of the flocculator coils, and :math:`v` is the kinematic viscosity of water, which is about :math:`1\times \ {10}^{-6}\ \frac{{m}^2}{s}` at 20 :math:`{}^\circ` C (Kundu and Cohen, 2008). The overall experimental flow rate was 6 mL/s and the radius of curvature of the coiled tubing (:math:`R_{c}` ) was 15 cm.

The value of :math:`{\bar G_{CS_{Straight}}}` was calculated by first estimating the head loss in a straight tube of the equivalent diameter and material using the Hagen-Poiseuille equation for laminar flow:

.. math::

   h_{L}=\frac{32\nu \bar{u}L}{D^2g}

where :math:`L` is the length of the tube (25.45 m in these experiments), :math:`\bar{u}` is the mean velocity (84 mm/s) of the flow, and :math:`g` is the acceleration due to gravity (Granger, 1995). From this head loss, an average rate of the loss of kinetic energy, :math:`\bar{\varepsilon }` can be estimated using

.. math::

   \bar{\varepsilon }=\frac{gh_{L}}{\theta }

where :math:`\theta` is the mean hydraulic residence time (Pennock et al., 2018). The hydraulic residence time was 302 s as calculated by

.. math::

   \theta =\frac{L}{\bar{u}}

The energy dissipation rate, which was calculated to be 2.24 mW/kg, can be converted to velocity gradient, :math:`\bar G_{CS}`, by

.. math::

   \bar G_{CS}=\sqrt{\frac{\bar{\varepsilon }}{\nu }}

which gave a velocity gradient of 50.1 :math:`{s}^{-1}` . Using this value for :math:`{\bar G_{CS_{Straight}}}` in Equation 1 resulted in a value of 71.1 :math:`{s}^{-1\ }` for :math:`\bar G_{CS}` .

A coiled tube flocculator was used in this research because it is a high-Peclet-number reactor much like a baffled hydraulic flocculator and also because the average velocity gradient in laminar tube flow is well defined (Weber-Shirk and Lion, 2010). After flowing through the flocculator, a fraction of the flow was passed through a tube settler and the settled water turbidity was recorded continuously for each experiment. The 1.37 m (4.5 ft) tube settler, with an inner diameter of 2.66 cm, had an entry port diameter of 0.95 cm (3/8 in) near the bottom and an exit port diameter of 0.635 cm (1/4 in) near the top. The capture velocity was controlled at 0.102 mm/s using a peristaltic pump with flow set by

.. math::

   Q_S=\frac{\pi}{4} D^2_{S}V_{c}\left(\frac{L_{S}}{D_{S}}{cos {\alpha }_{S}\ }+{sin {\alpha }_{S}\ }\right)

where :math:`V_{c}` is the capture velocity, :math:`L_{S}` is the length of the tube settler, :math:`D_{S}` is the diameter of the tube settler, and :math:`{\alpha }_{S}` is the angle of inclination of the tube settler, which was set at 60 degrees (Schulz and Okun, 1984).


Model Formation
---------------

A flocculation model considering the effects of humic acid should predict the effective collisions between colloids for a given set of conditions. The dimensionless product of the fluid velocity gradient and mean hydraulic residence time, :math:`\bar G_{CS}\theta`, has been used as a measure of the collision potential provided by a flocculator that experiences laminar flow (Camp, 1955; Cleasby, 1984). It is well known that not all collisions between suspended particles result in aggregation, and average attachment efficiency, :math:`\bar{\alpha }`, has been used to denote the fraction of successful collisions (AWWA, 1999).

The initial primary particle volume fraction, :math:`{\phi }_0`, also influences coagulation (Ives, 1968; O'Melia, 1972) and gives the fraction of the volume of the suspension occupied by the influent primary particles,

.. math::

   {\phi }_0=\frac{C_0}{{\rho }_{P}}

where :math:`C_0` is the influent particle concentration (kaolinite clay in these experiments) and :math:`{\rho }_{P}` is the density of influent particles (Swetland et al., 2014).

In laminar-flow flocculators, the velocity of one floc relative to another scales with the average separation distance between flocs (Swetland et al., 2014). The time between floc collisions is inversely proportional to both :math:`\phi` and the relative velocity between flocs. Because the relative velocity between flocs is proportional to separation distance, the time between collisions is proportional to :math:`{\phi }^{\frac{1}{3}}`, since the average separation distance, :math:`\bar \Lambda`, is given by

.. math::

   \bar \Lambda=d_{P}{\left(\frac{\pi }{6\phi }\right)}^{\frac{1}{3}}

The result is that, for laminar flow, the average time for primary particle collisions scales with :math:`{\phi }^{-\frac{2}{3}}` (Weber-Shirk and Lion 2010).

A laminar-flow hydraulic flocculator model was developed and validated based on the above analysis in Pennock et al. (2018) with the form

.. math::
   :label: eq_AguaClara_Flocculation_Model

   pC^{*}=\frac{3}{2}{{log}_{10} \left[\frac{2}{3}{\left(\frac{6}{\pi }\right)}^{\frac{2}{3}}\pi k\bar{\alpha }\bar G_{CS}\theta {\phi }^{\frac{2}{3}}_0+1\right]\ }

where  :math:`k` is a fitting parameter dependent on the value of :math:`V_{c}` used for sedimentation, :math:`\bar{\alpha }` is the mean fraction of collisions that are successful (i.e., result in aggregation), and :math:`pC^*` is defined as

.. math::
   :label: eq_pC_AguaClara_Flocculation_Model

   pC^*=-{log \left(\frac{\mathrm{Effluent\ turbidity}}{\mathrm{Influent\ Turbidity}}\right)\ }

Equation :eq:`eq_AguaClara_Flocculation_Model`, referred to as the AguaClara flocculation model in Pennock et al. (2018), is a Lagrangian hydrodynamic model that assumes that the aggregation of primary particles is rate-limiting. It further assumes that these particles, on average, will collide when the volume of fluid swept out as one particle approaches the other is equal to the average volume occupied by a single particle in the suspension. The time for these collisions to occur increases as flocculation proceeds, since the concentration of primary particles decreases in a way that is assumed to be first order with respect to collisions. Thus, with each successive collision, the average volume occupied by primary particles increases, and it takes longer for the next collision to occur. In Equation :eq:`eq_AguaClara_Flocculation_Model`, performance is linearly proportional to the logarithm of the effective collision potential, :math:`log(\bar{\alpha }\bar G_{CS}\theta {\phi }^{2/3}_0)`.

This group of parameters is the same as the group first described by Swetland et al. (2014), with the exception that they used the estimated fractional coverage of the colloid surface by coagulant, :math:`{\bar{\Gamma}}_{PACl-Clay}`, as a measure of attachment efficiency instead of :math:`\bar{\alpha }`. Pennock et al. (2018) recognized that surface coverage of both particles participating in a collision matters, and introduced :math:`\bar{\alpha }` to convert the geometric information contained in :math:`{\bar{\Gamma}}_{PACl-Clay}` to a probability of a successful collision. Using data gathered by Swetland et al. (2014), Pennock et al. (2018) were able to predict the results of independent laminar flocculation experiments with no adjustable parameters in the absence of added DOM.

Experimental results obtained with added humic acid present are shown in :numref:`figure_Du_Fig3` along with predictions based on the AguaClara flocculation model [Equation :eq:`eq_AguaClara_Flocculation_Model`].  It was evident that the attachment efficiency was adversely affected by the addition of humic acid.  Referencing adsorption measurements by Davis (1982), a minority (his study found 20\%) of added DOM would be adsorbed by kaolinite at the experimental pH of 7.5. Thus, most humic acid macromolecules were available to attach to the added coagulant nanoparticles. The following simplifying assumptions were made to account for the presence of humic acids: 1) humic acid macromolecules attach to coagulant nanoparticles to form nanoaggregates, 2) nanoaggregates attach to clay and to the reactor walls, and 3) the surfaces of precipitated coagulant nanoparticles promote adhesion, while the surfaces of bound humic acids prevent adhesion.

In this study, humic acid macromolecules and PACl nanoparticles were modeled as spheres. Based on the size of coagulant nanoparticles and humic acid macromolecules, their number concentrations, :math:`N_{HA}` and :math:`N_{PACl}` respectively, can be estimated by

.. math::

   N_{HA}=\ \frac{C_{HA}}{{\rho }_{HA}\frac{\pi }{6}{d_{HA}}^3}

and

.. math::

   N_{PACl}=\ \frac{C_{PACl}}{{\rho }_{PACl}\frac{\pi }{6}{d_{PACl}}^3}

where :math:`C_{PACl}` is the dose of coagulant in mg/L as Al; :math:`C_{HA}` is the concentration of humic acid in mg/L; :math:`{\rho }_{PACl}` is the density of the coagulant (Swetland et al. (2013) found :math:`1,138 \frac{kg}{m^3}`); :math:`{\rho }_{HA}` is the density of humic acid, :math:`1,520\frac{kg}{m^3}` (Sigma-Aldrich, 2014); :math:`d_{HA}` is the diameter of humic acid macromolecules (an adjustable model parameter); and :math:`d_{PACl}` is the diameter of precipitated PACl coagulant nanoparticles, taken to be 90 nm as found by Dr. Casey Garland (personal communication).

A key model assumption was that humic acid macromolecules cannot adhere to a coagulant surface that is occupied by a humic acid macromolecule, since humic acid macromolecules are assumed to not appreciably self-aggregate. The outcome of this assumption is that humic acid macromolecules attach to an uncovered surface of coagulant and do not stack on top of one another. The available surface area of the PACl nanoparticle was modeled as the surface area of an equivalent sphere. The amount of that area that is occupied by an attached humic acid macromolecule was estimated as the projected area of a sphere with volume equivalent to a humic acid macromolecule.  A new variable describing the coverage of coagulant nanoparticle surface area by humic acid macromolecules,

.. math::
   :label: eq_Gamma_HA-PACl

   {\bar{\Gamma}\mathrm{\ }}_{HA-PACl}=\frac{{{\frac{\pi }{4}d}_{HA}}^2}{{{\pi d}_{PACl}}^2}\frac{N_{HA}}{N_{PACl}}

was created to be incorporated into the model (within :math:`\bar{\alpha }`) to represent the fraction of the PACl nanoparticle surface area that is covered by humic acid macromolecules.

The first two steps in particle aggregation, where humic acid macromolecules attach to coagulant nanoparticles and then the resulting nanoaggregates attach to clay surfaces, were assumed to be rapid because diffusion is an effective transport process for nanoparticles (Benjamin and Lawler, 2013). Subsequent to rapid mix, the clay particles with attached nanoaggregates undergo collisions during the flocculation process and the aggregation process is governed by fluid shear (Pennock et al., 2018). The success of a collision between clay particles is hypothesized to be dependent on the properties of the contact surfaces at the initial point of contact.

The three types of surfaces (PACl, humic acid, clay) have 6 (3!) potential interactions as illustrated in :numref:`figure_Du_Fig2`.

.. _figure_Du_Fig2:

.. figure:: Images/Du_Fig2.png
   :width: 400px
   :align: center
   :alt: Experimental Apparatus

   Modes of collision between particles during flocculation.


Of these interactions considered in the model, the collisions that will result in attachment are assumed to involve at least one PACl nanoparticle surface (:numref:`figure_Du_Fig2` A, B, C). The attachment efficiency is hypothesized to be the sum of probability of these three types of collisions, formally expressed as

.. math::

   \bar{\alpha }\ ={\bar{\alpha }}_{PACl-Clay}+{\bar{\alpha }}_{PACl-PACl}+{\bar{\alpha }}_{HA-PACl}

where the subscripts define the two surfaces that are interacting. The overbars indicate that all of these represent mean probabilities for an entire suspension rather than the probabilities for specific particles.

The probability of a clay surface colliding with a PACl surface (:numref:`figure_Du_Fig2` A) is equal to twice the probability that the first surface is clay (:math:`1-{\bar{\Gamma}}_{PACl-Clay}`) and the second surface is the PACl surface of a PACl-HA nanoaggregate (:math:`\left(1-{\bar{\Gamma}}_{HA-PACl}\right){\bar{\Gamma}}_{PACl-Clay}`), since either of two colliding particles could provide the clay surface or the PACl surface,

.. math::

   {\bar{\alpha }}_{PACl-Clay}=2\left(1-{\bar{\Gamma}}_{PACl-Clay}\right)\left[\left(1-{\bar{\Gamma}}_{HA-PACl}\right){\bar{\Gamma}}_{PACl-Clay}\right]

The probability of a collision between the PACl surfaces of two PACl-HA nanoaggregates (:math:`\left(1-{\bar{\Gamma}}_{HA-PACl}\right){\bar{\Gamma}}_{PACl-Clay}`) (:numref:`figure_Du_Fig2` B) is given by

.. math::

   {\bar{\alpha}}_{PACl-PACl}={\left[\left(1-{\bar{\Gamma}}_{HA-PACl}\right){\bar{\Gamma}}_{PACl-Clay}\right]}^2

The probability of a collision between a PACl surface of a PACl-HA nanoaggregate (:math:`\left(1-{\bar{\Gamma}}_{HA-PACl}\right){\bar{\Gamma}}_{PACl-Clay}`) and an HA surface of a PACl-HA nanoaggregate (:math:`{\bar{\Gamma}}_{HA-PACl}{\bar{\Gamma}}_{PACl-Clay}`) (:numref:`figure_Du_Fig2` C), or vice versa, is given by

.. math::

   {\bar{\alpha }}_{HA-PACl}=2\left[{\bar{\Gamma}}_{PACl-Clay}\left(1-{\bar{\Gamma}}_{HA-PACl}\right)\right]\left[{\bar{\Gamma}}_{HA-PACl}{\bar{\Gamma}}_{PACl-Clay}\right]

where the factor of 2 accounts for the possibility that either colliding particle could contribute either surface type.

The model accounting for the presence of humic acids is modified from the Pennock et al. (2018) model by redefining the attachment efficiency, :math:`\bar{\alpha }`, using Eq. 14 to account for the presence of humic acid.

The physical properties of humic acid vary with composition. The diameter of humic acid macromolecules is estimated to range from 4 nm to 110 nm (\"{O}sterberg, 1993). Because of the variation in the size of humic acid macromolecules, the characteristic diameter of the humic acid macromolecules was used as a fitting parameter. Thus, there are two adjustable model parameters, :math:`k`(Equation :eq:`eq_AguaClara_Flocculation_Model`,), which accounts for the sedimentation capture velocity, and :math:`d_{HA}`, which accounts for coagulant precipitate surface coverage by humic acid.  These parameters were fit to results from observations taken with an influent turbidity of 50 NTU; the model was then validated by independently predicting results from experiments with an influent turbidity of 100 NTU.

Results
-------

The results from 60 experiments, transformed by :eq:`eq_pC_AguaClara_Flocculation_Model`, are shown in :numref:`figure_Du_Fig3` for an inflow turbidity of 50 NTU with PACl doses ranging from 0.53 to 2.65 mg/L as Al and humic acid concentration ranging from 0 to 15 mg/L. A capture velocity of 0.120 mm/s was used in the experiments, which is a conservatively designed lamellar settler capture velocity (Willis, 1978).  Experiments were replicated for each combination of humic acid and PACl dose.

The data show that increased coagulant dose is positively correlated with turbidity removal. The effluent turbidity was greatly increased by the presence of humic acid.  Also shown is a model fit using the AguaClara flocculation model given by Pennock et al. (2018). As shown, the model can fit the performance of the 0 mg/L HA data and even the 3 mg/L HA data reasonably well, but increasing doses of humic acid decrease performance appreciably, necessitating a modification to the original model.

.. _figure_Du_Fig3:

.. figure:: Images/Du_Fig3.png
   :width: 400px
   :align: center
   :alt: Experimental Apparatus

   :math:`\boldsymbol{p}{\boldsymbol{C}}^{\boldsymbol{*}}` as a function of coagulant dose for 50 NTU influent turbidity.


To apply the modified model to the raw data, the data points with 0 mg/L HA were fit by :math:`k`, since their performance was not influenced by :math:`d_{HA}`, resulting in :math:`k` = 0.16. Then, the remaining data were fit using :math:`d_{HA}` (with the previously determined :math:`k` value) to minimize the sum squared error, resulting in :math:`d_{HA}=77 \mathrm{nm}` with a :math:`pC^*` (dimensionless) root-mean-square error, RMSE, of 0.08. Because their performance was essentially indistinguishable from the 0 mg/L HA data. Additionally, to avoid biasing the fit by data for which the coagulant dose was insufficient to overcome the effect of humic acid, data for which performance was lower than :math:`pC^*=0.25` were neglected for the fitting. :numref:`figure_Du_Fig4` shows the fit of the model to the observations for the 50 NTU experiments.

.. _figure_Du_Fig4:

.. figure:: Images/Du_Fig4.png
   :width: 400px
   :align: center
   :alt: Experimental Apparatus

   Model fit for :math:`\boldsymbol{p}{\boldsymbol{C}}^{\boldsymbol{*}}` as function of coagulant dose for 50 NTU raw water turbidity.


With the given fitted value of :math:`d_{HA}=75 nm` for the 50 NTU influent turbidity data set, the coverage of coagulant nanoparticle surfaces by humic acid (:math:`{\bar{\Gamma}}_{HA-PACl}`) changed as shown in :numref:`figure_Du_Fig5`. According to Equation :eq:`eq_Gamma_HA-PACl`, the maximum number of humic acid macromolecules with a diameter of 75 nm that will fit on a PACl precipitate particle of 90 nm diameter when fully coating it is about 6. The model predicted complete coverage of the PACl nanoparticles by humic acid for low PACl concentrations, which correlated with very low observed turbidity removal efficiency.

.. _figure_Du_Fig5:

.. figure:: Images/Du_Fig5.png
   :width: 400px
   :align: center
   :alt: Experimental Apparatus

   Coverage of coagulant surface by humic acid as a function of coagulant dose.


The relationships between the three terms included in attachment efficiency are shown in :numref:`figure_Du_Fig6`.  The term corresponding to collisions between a clean coagulant nanoparticle surface and clay (:math:`{\bar{\alpha }}_{PACl-Clay}`) was always dominant for the experimental conditions in this dataset, and the other terms became relatively more important but still insignificant small with respect to :math:`{\bar{\alpha }}_{PACl-Clay}` with increasing coagulant dose.

.. _figure_Du_Fig6:

.. figure:: Images/Du_Fig6.png
   :width: 400px
   :align: center
   :alt: Experimental Apparatus

   Attachment efficiency as a function of coagulant dose.

The model was validated by using it to predict turbidity removal efficiency for different experimental conditions. The predicted :math:`pC^*` and the measured :math:`pC^*` are compared in :numref:`figure_Du_Fig7` for an additional 60 experiments with inflow turbidity of 100 NTU, PACl doses ranging from 0.53 to 2.65 mg/L, and humic acid concentration ranging from 0 to 15 mg/L. The resulting fit is almost as good as for the 50 NTU data, with :math:`pC^*` RMSE of 0.11.

.. _figure_Du_Fig7:

.. figure:: Images/Du_Fig7.png
   :width: 400px
   :align: center
   :alt: Experimental Apparatus

   Comparison graph between predicted data and observed data for 100 NTU influent turbidity.

When the coagulant dose in :numref:`figure_Du_Fig4` and :numref:`figure_Du_Fig7` was replaced with the dimensionless group :math:`\bar{\alpha }\bar G_{CS}\theta {\phi }^{\frac{2}{3}}` , the data collapsed to a much narrower band, implying that the composite parameter, :math:`\bar{\alpha }\bar G_{CS}\theta {\phi }^{\frac{2}{3}}`, captures a large fraction of the trends present in the data, as seen in :numref:`figure_Du_Fig8`.

.. _figure_Du_Fig8:

.. figure:: Images/Du_Fig8.png
   :width: 400px
   :align: center
   :alt: Experimental Apparatus

   Model fit of 50 and 100 NTU data for :math:`\boldsymbol{p}{\boldsymbol{C}}^{\boldsymbol{*}}` as function of effective collision potential. The data plotted include two replicates for each experiment.

In summary, the laminar flow hydraulic flocculation model of Pennock et al. (2018) was modified to incorporate the effects of humic acid with the addition of a single fitting parameter: a characteristic dimension of the humic acid macromolecules. The required coagulant dose can be predicted based on the flocculator parameters, humic acid characteristic size and concentration, and influent turbidity. The addition of humic acid to the flocculation model increases the model applicability since natural organic matter is found in all surface and ground waters and influences the coagulant dose needed for effective turbidity removal.

Discussion
----------

For the range of experimental conditions considered in the research, the observed influence of humic acid on flocculation performance could be explained by the fractional coverage of the coagulant nanoparticle surfaces by humic acid macromolecules. It is noteworthy that under the experimental conditions, the predictive success of the model was achieved without incorporating the charges of colloids, coagulant, and humic acids. The reader is cautioned that the observations and predictions were obtained with one test particle, one coagulant, and one form of DOM in the mixed electrolyte represented by Cornell tap water, kept within the narrow pH range where coagulant precipitation is very favorable. While the experimental pH favored PACl precipitation, pH-dependent PACl solubility is accounted for in the model with

.. math::

   N_{perClay}=\frac{\left[C_{PACl}-C_{PACl\left(aq\right)}\right]V_{P}{\rho }_{P}}{\frac{\pi }{6}d^{3}_{PACl}{\rho }_{PACl}C_0}

where :math:`N_{perClay}` is the number of precipitated coagulant aggregates per clay particle, :math:`C_{\mathrm{PACl\ (aq)}}` is the fraction of the coagulant dose that has remained in solution after precipitation using the PACl solubility observed by Van Benschoten and Edzwald (1990), and :math:`V_{P}` is the volume of a single clay platelet (Swetland et al., 2014). Within the model, :math:`N_{perClay}` is used to calculate :math:`\bar{\alpha }`, since it is a component of the calculation for :math:`{\bar{\Gamma}}_{PACl-Clay}`:

.. math::

   {\bar{\Gamma}}_{PACl-Clay}=1-e^{-\frac{d^{2}_{PACl}}{{SA}_{Clay}}\ N_{perClay}R_{Clay}\ }

where :math:`{SA}_{Clay}` is the surface area of the surface area of the suspended clay particles and :math:`R_{Clay}` is the fraction of the available surface area in the reactor (including the surface area of reactor walls) that belongs to suspended clay particles (Swetland et al., 2014).

The solubility of humic acid also is highly pH-dependent, and additional experimental results are needed to test the applicability of the model approach as a function of varying pH. The experimental conditions were designed to keep the pH relatively constant, and the pH change in the experiments was small (7.5 :math:`\pm` 0.3).

The model considered flocculation in the presence of humic acid as a two-step process. Firstly, humic acid macromolecules attached to precipitated coagulant nanoparticles. Then, the partially-coated coagulant nanoaggregates could bind to clay and reactor wall surfaces.  Humic acid and coagulant nanoparticles were treated as spheres when estimating the attachment efficiency based on surface coverage and probability. The diameter of precipitated PACl nanoparticles was experimentally measured to be 90 nm (Garland, 2016), and a humic acid macromolecule diameter of 77 nm best fit the observations. Wall loss of coagulant precipitates with humic acid nanoaggregates was considered while direct wall loss of humic acid macromolecules was not considered.

The characteristic humic acid dimension, :math:`d_{HA}`, has a physical meaning, with the fitted value, 77 nm, falling within the range (4-110 nm) reported by \"{O}sterberg (1993), and the model fits are well correlated to the observations. The predictive capability of the model was verified by predicting results under different experimental conditions with no additional adjustable parameters.

The flocculation model without the effects of humic acid shows that :math:`pC^*` is directly proportional to the log of the effective collision potential, :math:`log(\bar{\alpha }\bar G_{CS}\theta {\phi }^{\frac{2}{3}})`, and this relationship is still present in the model with a modified attachment efficiency, :math:`\bar{\alpha },` based on clay surface coverage by coagulant nanoparticles as adjusted for the presence of humic acids.

Under experimental conditions, the modified flocculation model provides the fundamental basis for the relationship between coagulant dose, synthetic raw water clay, and humic acid concentrations.  Extension to natural waters will undoubtedly require additional research.

The form of the flocculation model equation sets the interactions between raw water properties (:math:`{\phi }_0`), influent particle surface area (which contributes to :math:`{\bar{\Gamma}}_{PACl-Clay}`), coagulant precipitate size and dose (which contributes to :math:`{\bar{\Gamma}}_{PACl-Clay}` and :math:`{\bar{\Gamma}}_{HA-PACl}`) , humic acid molecule size and concentration (which contribute to :math:`{\bar{\Gamma}}_{HA-PACl}`), flocculator design (:math:`\bar G_{CS}\theta`), and sedimentation tank design (:math:`k`). In a gravity-powered water treatment plant operating at constant flow rate, the flocculator and sedimentation tank parameters are constant. An increase in concentration of humic acid causes an increase in :math:`{\bar{\Gamma}}_{HA-PACl}`, which decreases :math:`pC^*` but can be compensated for by increasing coagulant dose.

Summary
-------

The development of a predictive model for laminar flow hydraulic flocculation of water containing clay and humic acid is described. The study results increase the flexibility and generality of the AguaClara hydraulic flocculation model, and the modified model provides insight into the mechanism by which humic acid causes a decrease in performance of coupled flocculation-sedimentation processes.

The model was able to predict independent experimental results for a different raw water turbidity with no additional adjustable parameters. Further tests should be done to fully validate the laminar-flow model including consideration of different experimental surrogates for DOM, different colloidal surfaces, alternative coagulants and varying solution compositions, including pH.


References
==========

Amin, M., Safari, M., Maleki, A., Ghasemian, M., Rezaee, R., & Hashemi, H. (2012). Feasibility of humic substances removal by enhanced coagulation process in surface water. International Journal of Environmental Health Engineering. http://www.ijehe.org/text.asp?2012/1/1/29/99323

Benjamin, M. M., & Lawler, D. F. (2013). Water quality engineering: physical / chemical treatment processes. Hoboken, N.J.: Wiley.

BP-MWS, CIWS, & CUWS. (2016). Drinking Water Quality Report 2016. Ithaca, NY: Bolton Point Municipal Water System, City of Ithaca Water System, Cornell University Water System. Retrieved from https://fcs.cornell.edu/content/water-system-updates-and-water-quality-reports

Camp, T. R. (1953). Flocculation and Flocculation Basins. American Society of Civil Engineers.

Chow, C. W. K., Fabris, R., Leeuwen, J. van, Wang, D., & Drikas, M. (2008). Assessing Natural Organic Matter Treatability Using High Performance Size Exclusion Chromatography. Environmental Science & Technology, 42(17), 6683–6689. https://doi.org/10.1021/es800794r

Cleasby, J. (1984). Is Velocity Gradient a Valid Turbulent Flocculation Parameter? Journal of Environmental Engineering, 110(5), 875–897. https://doi-org.proxy.library.cornell.edu/10.1061/(ASCE)0733-9372(1984)110:5(875)

Davis, J. A. (1982). Adsorption of natural dissolved organic matter at the oxide/water interface. Geochimica et Cosmochimica Acta, 46(11), 2381–2393. https://doi.org/10.1016/0016-7037(82)90209-5

Fosso-Kankeu, E., Webster, A., Ntwampe, I. O., & Waanders, F. B. (2017). Coagulation/Flocculation Potential of Polyaluminium Chloride and Bentonite Clay Tested in the Removal of Methyl Red and Crystal Violet. Arabian Journal for Science and Engineering, 42(4), 1389–1397. https://doi.org/10.1007/s13369-016-2244-x

Granger, R. A. (1995). Fluid Mechanics. New York: Dover Publications.

Hu, C., Hu, X., Wang, L., Qu, J., & Wang, A. (2006). Visible-Light-Induced Photocatalytic Degradation of Azodyes in Aqueous AgI/TiO 2 Dispersion. Environmental Science & Technology, 40(24), 7903–7907. https://doi.org/10.1021/es061599r

Hua, G., & Reckhow, D. A. (2008). Relationship between Brominated THMs, HAAs, and Total Organic Bromine during Drinking Water Chlorination. In T. Karanfil, S. W. Krasner, P. Westerhoff, & Y. Xie (Eds.), Disinfection By-Products in Drinking Water (Vol. 995, pp. 109–123). Washington, DC: American Chemical Society. https://doi.org/10.1021/bk-2008-0995.ch008

Integrated design of water treatment facilities: Susumu Kawamura. John Wiley & Sons, Inc.: New York, NY 1991. (pp. 658, ISBN 0-471-61591-9) $69.95 hardcover. (1992). Waste Management, 12(1), 101. https://doi.org/10.1016/0956-053X(92)90024-D

Ives, K. J. (1968). Theory of operation of sludge blanket clarifiers. Proceedings of the Institution of Civil Engineers, 39(2), 243–260. https://doi.org/10.1680/iicep.1968.8090

Jarvis, P., Jefferson, B., Gregory, J., & Parsons, S. A. (2005). A review of floc strength and breakage. Water Research, 39(14), 3121–3137. https://doi.org/10.1016/j.watres.2005.05.022

Kundu, P. K., & Cohen, I. M. (2008). Fluid mechanics. Amsterdam; Boston: Academic Press.
Letterman, R. D. (1999). Water quality and treatment: a handbook of community water supplies (5th ed.). New York: McGraw-Hill.

Matilainen, A., Vepsäläinen, M., & Sillanpää, M. (2010). Natural organic matter removal by coagulation during drinking water treatment: A review. Advances in Colloid and Interface Science, 159(2), 189–197. https://doi.org/10.1016/j.cis.2010.06.007

Marhaba, T. F., Pu, Y., & Bengraine, K. (2003). Modified dissolved organic matter fractionation technique for natural water. Journal of Hazardous Materials, 101(1), 43–53. https://doi.org/10.1016/S0304-3894(03)00133-X

O’Melia, C. R. (1972). Coagulation and flocculation. In W. J. Weber (Ed.), Physicochemical processes for water quality control. New York: Wiley-Interscience.

Österberg, R., Lindovist, I., & Mortensen, K. (1993). Particle Size of Humic Acid. Soil Science Society of America Journal, 57(1), 283–285. https://doi.org/10.2136/sssaj1993.03615995005700010048x

Pennock, William H., Weber-Shirk, Monroe, & Lion, Leonard W. (2018). A Hydrodynamic and Surface Coverage Model Capable of Predicting Settled Effluent Turbidity Subsequent to Hydraulic Flocculation. Environmental Engineering Science, 35(12). https://doi.org/10.1089/ees.2017.0332

Schulz, C. R., & Okun, D. A. (1984). Surface water treatment for communities in developing countries. New York: Wiley.

Sharp, E. L., Jarvis, P., Parsons, S. A., & Jefferson, B. (2006). Impact of fractional character on the coagulation of NOM. Colloids and Surfaces A: Physicochemical and Engineering Aspects, 286(1–3), 104–111. https://doi.org/10.1016/j.colsurfa.2006.03.009

Sigma-Aldrich. (2014). Humic acid sodium salt (H16752) (Safety Data Sheet) (p. 7). St. Louis, MO. Retrieved from https://www.sigmaaldrich.com/MSDS/MSDS/DisplayMSDSPage.do?country=US&language=en&productNumber=H16752&brand=ALDRICH&PageToGoToURL=https%3A%2F%2Fwww.sigmaaldrich.com%2Fcatalog%2Fproduct%2Faldrich%2Fh16752%3Flang%3Den

Soh, Y. C., Roddick, F., & Leeuwen, J. van. (2008). The impact of alum coagulation on the character, biodegradability and disinfection by-product formation potential of reservoir natural organic matter (NOM) fractions. Water Science and Technology; London, 58(6), 1173–1179. http://dx.doi.org/10.2166/wst.2008.475

Swetland, K. A., Weber-Shirk, M. L., & Lion, L. W. (2013). Influence of Polymeric Aluminum Oxyhydroxide Precipitate-Aggregation on Flocculation Performance. Environmental Engineering Science, 30(9), 536–545. https://doi.org/10.1089/ees.2012.0199

Swetland, K. A., Weber-Shirk, M. L., & Lion, L. W. (2014). Flocculation-Sedimentation Performance Model for Laminar-Flow Hydraulic Flocculation with Polyaluminum Chloride and Aluminum Sulfate Coagulants. Journal of Environmental Engineering, 140(3), 04014002. https://doi.org/10.1061/(ASCE)EE.1943-7870.0000814

Tse, I. C., Swetland, K., Weber-Shirk, M. L., & Lion, L. W. (2011). Method for quantitative analysis of flocculation performance. Water Research, 45(10), 3075–3084. https://doi.org/10.1016/j.watres.2011.03.021

Van Benschoten, J. E., & Edzwald, J. K. (1990). Chemical aspects of coagulation using aluminum salts—I. Hydrolytic reactions of alum and polyaluminum chloride. Water Research, 24(12), 1519–1526. https://doi.org/10.1016/0043-1354(90)90086-L

Weber-Shirk, M. L. (2016). ProCoDA: An Automated Method for Testing Process Parameters. Retrieved October 30, 2015, from https://confluence.cornell.edu/display/AGUACLARA/ProCoDA

Weber-Shirk, M. L., & Lion, L. W. (2010). Flocculation model and collision potential for reactors with flows characterized by high Peclet numbers. Water Research, 44(18), 5180–5187. https://doi.org/10.1016/j.watres.2010.06.026

Willis, R. M. (1978). Tubular Settlers—A Technical Review. Journal (American Water Works Association), 70(6), 331–335.

.. bibliography:: /references.bib
   :cited:
   :keyprefix: Floc_Model-