---
  title: "Robust Control for TVC Rocket"
  date: 2026-05-20T12:00:00+06:00
  featured: false
  tags: "software"
  tranding: true
  readTime: "4 min"
  thumbnail: /images/blog/robust-control/robust-control.png
  featureImage: /images/blog/robust-control/robust-control.png
  math: true
---

<style>
  /* Basic CSS for the tables */
  .styled-table {
    border-collapse: collapse;
    width: 100%;
    margin: 20px 0;
    font-size: 1em;
    min-width: 400px;
  }

  .styled-table th, .styled-table td {
    border: 1px solid #dddddd; /* Explicit border color */
    padding: 12px 15px; /* Increased padding for spacing */
    text-align: center; /* Center alignment */
  }

  .styled-table th {
    background-color: #f2f2f2; /* Light background for headers */
    font-weight: bold;
  }

</style>

This past semester at Georgia Tech I had the opportunity to lead the controls team for the Guidance, Navigation and Controls club. One of the main focuses for the club this semester was the launch of a thrust vector control rocket to keep it vertical throughout the flight. During this same time I was also taking a Robust Control class which is the math behind linear control systems in the presence of uncertainty. This blog post explores how I applied the abstract theory of Robust Control to a thrust vector control rocket.

The first step in any control project is the modeling phase. Since Robust Control theory uses linear techniques, we require a linear time-invariant (LTI) system model of the rocket. In our analysis, we will use the pitch dynamics of the rocket because that is what we want to control. There is a fantastic paper by the Applied Physics Laboratory (APL) called “Overview of Missile Flight Control Systems” that goes into detail in deriving the linear model of a rocket. The linear model is shown below:

<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
\begin{bmatrix}
    \dot{\alpha} \\
    \dot{q}
    \end{bmatrix}
     = 
    \begin{bmatrix}
    -\tilde{Z}_{\alpha} & 1 \\
    M_\alpha & 0
    \end{bmatrix}
    \begin{bmatrix}
    \alpha \\
    q
    \end{bmatrix}
    + 
    \begin{bmatrix}
    -\tilde{Z}_{\delta} \\
    M_\delta
    \end{bmatrix}
    \delta" />
</p>



The states in model are angle of attack and pitch rate with the input being gimbal deflection. The coefficients can be derived from the flight conditions and properties about the rocket. Below are the formulas to calculate the aerodynamics coefficients.

The Dimensional Pitch Control Effectiveness coefficient defines the angular pitching acceleration generated per radian of control/thrust deflection.
<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
  M_{\delta}=\frac{Tl}{J}" />
</p>

The Dimensional Pitch Force Derivative with respect to Control Deflection represents the normal force acceleration generated per radian of control/thrust deflection.

<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
  \tilde{Z}_{\delta} = \frac{T}{mv}" />
</p>

The dynamic pressure is defined as the aerodynamic pressure acting on the rocket.
<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
    q=\frac{1}{2}\rho v^2" />
</p>

The pitching moment derivative with respect to alpha defines the change in aerodynamic moment generated per radian change in angle of attack.
<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
    \frac{\partial M}{\partial \alpha} = qSd C_{M_{\alpha}}" />
</p>

The Dimensional Pitch Moment Derivative with respect to Angle of Attack scales the physical aerodynamic pitching moment by the vehicle's moment of inertia to yield an angular acceleration.
<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
    M_{\alpha} = \frac{1}{J} \frac{\partial M}{\partial \alpha}" />
</p>

The non-dimensional pitching moment coefficient derivative calculates the aerodynamic stability margin by scaling the normal force coefficient relative to the distance between the center of gravity and the center of pressure.
<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
    C_{M_{\alpha}}=C_{N_{\alpha}} (\frac{x_{cg}-x_{cp}}{d})" />
</p>

The normal force derivative with respect to alpha defines the change in physical aerodynamic normal force generated per radian change in angle of attack.
<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
    \frac{\partial F}{\partial \alpha} = qS C_{N_{\alpha}}" />
</p>

The Dimensional Pitch Force Derivative with respect to Angle of Attack normalizes the aerodynamic normal force by the vehicle's momentum to express the resulting change in flight path rate.
<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
    \tilde{Z}_{\alpha} = \frac{1}{mv} \frac{\partial F}{\partial \alpha}" />
</p>



<table class="styled-table">  <thead>
    <tr>
      <th style="border: 1px solid black;">Variable</th>
      <th style="border: 1px solid black;">Description</th>
      <th style="border: 1px solid black;">Nominal Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid black;">$S$</td>
      <td style="border: 1px solid black;">Aero reference area</td>
      <td style="border: 1px solid black;">0.00447 $m^2$</td>
    </tr>
    <tr>
      <td style="border: 1px solid black;"><b>$\rho$</b></td>
      <td style="border: 1px solid black;">Air density</td>
      <td style="border: 1px solid black;">1.225 $\frac{kg}{m^3}$</td>
    </tr>
    <tr>
      <td style="border: 1px solid black;">$d$</td>
      <td style="border: 1px solid black;">Aero Reference length</td>
      <td style="border: 1px solid black;">0.102108 $m$</td>
    </tr>
    <tr>
      <td style="border: 1px solid black;">$C_{N_{\alpha}}$</td>
      <td style="border: 1px solid black;">Normal force coefficient derivative</td>
      <td style="border: 1px solid black;">7</td>
    </tr>
    <tr>
      <td style="border: 1px solid black;">$x_{cp}-x_{cg}$</td>
      <td style="border: 1px solid black;">Aero moment arm</td>
      <td style="border: 1px solid black;">-0.14226 $m$</td>
    </tr>
    <tr>
      <td style="border: 1px solid black;">$J$</td>
      <td style="border: 1px solid black;">Moment of Inertia</td>
      <td style="border: 1px solid black;">0.4 $kg \cdot m^2$</td>
    </tr>
    <tr>
      <td style="border: 1px solid black;">$l$</td>
      <td style="border: 1px solid black;">Moment arm</td>
      <td style="border: 1px solid black;">0.3 $m$</td>
    </tr>
    <tr>
      <td style="border: 1px solid black;">$m$</td>
      <td style="border: 1px solid black;">Mass</td>
      <td style="border: 1px solid black;">3.15 $kg$</td>
    </tr>
    <tr>
      <td style="border: 1px solid black;">$T$</td>
      <td style="border: 1px solid black;">Thrust</td>
      <td style="border: 1px solid black;">38 $N$</td>
    </tr>
    <tr>
      <td style="border: 1px solid black;">$v$</td>
      <td style="border: 1px solid black;">Velocity</td>
      <td style="border: 1px solid black;">18 $\frac{m}{s}$</td>
    </tr>
  </tbody>
</table>

The state-space representation can be converted into a transfer function for frequency-domain analysis using the following relationship, where $A$ and $B$ are the system matrices defined previously:

<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
    G(s)=C(sI-A)^{-1}B+D" />
</p>

The output matrix $C$ and direct transmission matrix $D$ are defined as:

<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
    D=0" />
</p>

<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
    C =
    \begin{bmatrix}
    0 & 0 & 1
    \end{bmatrix}" />
</p>

The transfer function for our rocket becomes the following:

<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
    G_p(s) = \frac{ M_{\delta} \tilde{Z}_{\alpha} - M_{\alpha} \tilde{Z}_{\delta} + M_{\delta}s } {s^2 + \tilde{Z}_{\alpha}s-M_{\alpha}}" />
</p>

This transfer function relates the thrust vector control (TVC) deflection angle to the angular rate of the rocket. While this is a solid baseline model, capturing the full vehicle dynamics requires accounting for the servo actuator and the time delay between the command signal and servo response.

The servo can be modeled as a second-order system characterized by a specific natural frequency and damping ratio. The transfer function of a standard second-order system is shown below:

<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
    G_s(s) = \frac{\omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}" />
</p>

A second-order Padé approximation is used to represent the servo's time lag. This transfer function effectively simulates the delay of a signal within a specific bandwidth. Since the servo's time lag is approximately 20 milliseconds, a second-order approximation is sufficient for this model.The transfer function for a second-order Padé approximation of a time delay $\tau$ is expressed as:

<p align="center">
  <img src="https://latex.codecogs.com/svg.image? 
    $$T(s) = \frac{s^2\tau^2 - 6s\tau + 12}{s^2\tau^2 + 6s\tau + 12}$$" />
</p>

The nominal plant we are attempting to control is simply the combination of the three transfer functions derived above times a integrator (1/s) to convert from pitch rate to pitch. This combined transfer function relates the commanded thrust vector control (TVC) deflection to the actual rocket pitch.  Because the plant contains a pure integrator it is classified as a Type 1 system. For this type of system, a PD controller is an excellent choice. While more advanced robust controllers exist, a PD architecture offers a easy implementation on flight hardware. Below is a full block level diagram of the closed loop system with the controller and plant:

<p align="center">
  <img src="/images/blog/robust-control/nominal_block.jpg" alt="Alt text" width="600">
</p>

Now that the plant is defined, we can use linear techniques to tune the PD controller. However, there is one problem with this approach: it does not take into account uncertainty. If we tune our controller using the nominal plant, we assume that our aerodynamic coefficients, mass properties, and servo dynamics are exactly what we expect. In reality, we do not know the exact servo dynamics or aerodynamic coefficients. This is where robust control comes in. Robust control is the study of dynamical systems when uncertainty and disturbances are introduced. This theory allows us to develop a controller—or, in this case, find specific P and D gains—such that the controller can stabilize any combination of uncertain systems. To do this, we are going to use an algorithm called $\mu$-synthesis, utilizing the MATLAB Robust Control Toolbox. The first step is to define the disturbances and noise entering the system. In this analysis, noise enters the system in two places. The first is at the gimbal, which simulates disturbances acting on the gimbal mount during launch. The second is added to the output of the airframe to simulate IMU noise.

<p align="center">
  <img src="/images/blog/robust-control/augmented_sys.jpg" alt="Alt text" width="800">
</p>

