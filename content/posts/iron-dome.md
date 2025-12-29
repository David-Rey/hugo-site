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
      <td style="border: 1px solid black;"><img src="https://latex.codecogs.com/svg.image?\dot{\vec{v}}=\frac{1}{2m}\rho AC_D |\vec{v}-\vec{\omega}|(\vec{v}-\vec{\omega}) + \frac{1}{2m} \rho A C_L \left(\frac{\vec{\omega} \times (\vec{v}-\vec{\omega})}{\vec{\omega}} \right) + \vec{g} " />
      </p>
    </td>
    </tr>
    <tr>
      <td style="border: 1px solid black;"><b>Angular Dynamics</b></td>
      <td style="border: 1px solid black;"><img src="https://latex.codecogs.com/svg.image?\dot{\vec{\omega}}=\tau \vec{\omega}" />
        </p></td>
    </tr>
  </tbody>
</table>

## Tracking

### camera projection

### UKF

## Results

### Monte Carlo


