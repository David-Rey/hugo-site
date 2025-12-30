---
  title: "Golf Ball UKF Tracker"
  date: 2023-12-27T12:00:00+06:00
  featured: false
  tags: "software"
  tranding: true
  readTime: "6 min"
  thumbnail: /images/blog/iron-dome/iron-dome-thumbnail.png
  featureImage: /images/blog/iron-dome/iron-dome-thumbnail.png
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

Recently my parents moved into a beautiful new house directly on a golf course. This is heaven for my Dad who loves golf but there is one major problem of living directly on a golf course: Golf Balls! Unfortunately a few terrible golfers have hit our house as seen in the picture below.

[Add Image Here]

This got me thinking what if I made my own Iron Dome system to defend my house against golf balls? Well, there are two major parts to this problem. This first is tracking and the second is the interception. I focused my energy into the first problem of tracking and simulated it in MATLAB to see if it was possible.

## Golf Ball Dynamics

Before we can track a golf ball we first must be able to simulate it as a dynamical system. Almost all dynamical systems can be written in the form of $\dot{\mathbf{x}}=f(\mathbf{x})$ and a golf ball is no exception. The state vector $\mathbf{x}$ holds position, velocity and angular rate totaling 9 states and 6 degrees of freedom.

<p align="center">
  <img src="https://latex.codecogs.com/svg.image?\mathbf{x} = 
\begin{bmatrix}
    \vec{x} \\
    \vec{v} \\
    \vec{\omega} \\
\end{bmatrix}" />
</p>


where:
* $\vec{x} = [x, y, z]^T$ is the position vector in the inertial frame.
* $\vec{v} = [v_x, v_y, v_z]^T$ is the velocity vector in the inertial frame.
* $\vec{\omega} = [\omega_x, \omega_y, \omega_z]^T$ is the angular rotation in the body frame

<table class="styled-table">  <thead>
    <tr>
      <th style="border: 1px solid black;"></th>
      <th style="border: 1px solid black;">Dynamics</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid black;"><b>Position Dynamics</b></td>
      <td style="border: 1px solid black;"><p align="center">
      <img src="https://latex.codecogs.com/svg.image?\dot{\vec{x}}=\vec{v}" />
        </p>
    </td>
    </tr>
    <tr>
      <td style="border: 1px solid black;"><b>Velocity Dynamics</b></td>
      <td style="border: 1px solid black;"><img src="https://latex.codecogs.com/svg.image?\dot{\vec{v}}=\frac{1}{2m}\rho AC_D |\vec{v}-\vec{w}|(\vec{v}-\vec{w}) + \frac{1}{2m} \rho A C_L  |\vec{v}-\vec{w}| \left(\frac{\vec{\omega} \times (\vec{v}-\vec{w})}{|\vec{\omega}|} \right) + \vec{g} " />
      </p>
    </td>
    </tr>
    <tr>
      <td style="border: 1px solid black;"><b>Rotational Dynamics</b></td>
      <td style="border: 1px solid black;"><img src="https://latex.codecogs.com/svg.image?\dot{\vec{\omega}}=\tau \vec{\omega}" />
        </p></td>
    </tr>
  </tbody>
</table>

The complexity of the trajectory arises from the velocity dynamics, which can be broken down into three major components: 

* The first is drag acceleration which comes from the standard drag formula of ![](https://latex.codecogs.com/svg.image?\frac{1}{2m}%20\rho%20AC_D|\vec{v}-\vec{w}|(\vec{v}-\vec{w})) with the velocity being subtracted by the current wind. 

* The second section is calculating lift caused by the magnus effect. If you ever wonder why golf balls curve or climb in the air, this is why. The rotational spin on the ball produces a lift effect which causes the golf ball to accelerate. The magnus acceleration is modeled as: ![](https://latex.codecogs.com/svg.image?\frac{1}{2m}\rho%20A%20C_L%20|\vec{v}-\vec{w}|\left(\frac{\vec{\omega}%20\times%20(\vec{v}-\vec{w})}{|\vec{\omega}|}%20\right)). Looking at the formula the faster the ball spins the more lift the ball generates. 

* Finally the last part of the velocity dynamics is gravity which is a constant vector downward of -9.81 m/s.

For the Rotational dynamics the angular acceleration of the ball is modeled as a exponential decay with ![](https://latex.codecogs.com/svg.image?\tau) being a negative number ![](https://latex.codecogs.com/svg.image?\dot{\vec{\omega}}=\tau%20\vec{\omega}).

These dynamics can be written in the form $\dot{\mathbf{x}}=f(\mathbf{x})$ which when combined with a ode solver can calculate the trajectory of the entire golf ball given some initial condition.


## Camera Projection


In order to track the golf balls we need a sensor that can detect where the ball is. There are many sensors I could have picked but the one I used were simple optical cameras. The mathematical process to convert the position of the golf ball in global inertial frame to camera coordinates are as shown below where ![](https://latex.codecogs.com/svg.image?R) is a rotation matrix from local camera frame to the global frame and ![](https://latex.codecogs.com/svg.image?b) is the vector from the origin to the camera.


<p align="center">
  <img src="https://latex.codecogs.com/svg.image?P_{local} = R^T \cdot (P_{global}-b)" />
</p>

Once the position of the golf ball is in the local camera frame it can then be converted to a $u$ and $v$ frame defined below.

<p align="center">
  <img src="https://latex.codecogs.com/svg.image?\begin{aligned}
u &= \frac{x}{y} \\
v &= \frac{z}{y}
\end{aligned}" />
</p>

Below is are some results on what the 3d trajectory of the golf ball looks like along side what the camera views.

<table align="center">
  <tr>
    <td>
      <img src="/images/blog/iron-dome/tracking3d-no-est.png" alt="3D Tracking No Estimation" width="550">
    </td>
    <td>
      <img src="/images/blog/iron-dome/cam1-view.png" alt="Camera 1 View" width="350">
    </td>
  </tr>
</table>

## Tracking (UKF)

Now that we have a way to simulate our sensor we need to take that measurement and somehow figure out our state (position, velocity and spin). There are many ways to do this however the most advanced and robust is using something called a Kalman Filter. A Kalman Filter takes in noisy measurements and a system model to produce an optimal estimate of the current state, accounting for the uncertainty in both the dynamics and the measurements. Put simply a Kalman filter is just a tracker, in this case it's tracking golf balls.

There are two ways to implement the Kalman Filter. The first is using an Extended Kalman Filter (EKF) and the other uses a unscented Kalman Filter (UKF). There's few  technical differences between the two however the UKF is more efficient as if does not need to calculate a Jacobian at every time step.

Every Kalman Filter works by getting a state estimate and using measurements to update the uncertainty of that state. You can think of the uncertainty as some confidence "bubble" around the state for which that true state is some  likelihood of being in that "bubble". The UKF works by propagating a set of sigma points from the uncertainty distribution through the dynamics and using the output of those points to get a new uncertainty and estimate. The idea calculating uncertainty from passing points through a nonlinear function is called a Unscented Transform.

### UKF Initialization

The first step in the UKF is setting some tuning parameters. These parameters tell the UKF how to distribute the sigma points. Below is the formula for ![](https://latex.codecogs.com/svg.image?\lambda) which is the parameter for sigma point spacing.

$$
\lambda = \alpha^2 (N+\kappa)-N
$$

* ![](https://latex.codecogs.com/svg.image?\alpha) is a tuning parameter from 0 to 1 which controls the spread of sigma points
* ![](https://latex.codecogs.com/svg.image?N) is the number of states. In our example ![](https://latex.codecogs.com/svg.image?N=9)
* ![](https://latex.codecogs.com/svg.image?\kappa) is a secondary scaling parameter that is usually set to 0

From this the weighs of 

### UKF Predict

<p align="center">
  <img src="https://latex.codecogs.com/svg.image?\begin{aligned}
\chi_{n,n}^{(i)} &= \hat{\mathbf{x}}_{n,n} + \left(\sqrt{(N + \kappa) \mathbf{P}_{n,n}}\right)_i, & i &= 1, \dots, N \\
\chi_{n,n}^{(i)} &= \hat{\mathbf{x}}_{n,n} - \left(\sqrt{(N + \kappa) \mathbf{P}_{n,n}}\right)_{i-N}, & i &= N + 1, \dots, 2N
\end{aligned}" />
</p>

### UKF Update






## Results

### Monte Carlo


