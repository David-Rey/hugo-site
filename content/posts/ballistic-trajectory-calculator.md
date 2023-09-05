---
  title: "MATLAB Ballistic Trajectory Calculator"
  date: 2023-08-23T12:00:00+06:00
  featured: false
  tags: "tech"
  tranding: true
  readTime: "4 min"
  thumbnail: /images/blog/ballistic-trajectory-calculator/thumbnail.PNG
  featureImage: /images/blog/ballistic-trajectory-calculator/featureImage.PNG
---

This MATLAB program was developed as a final project to an intro to MATLAB course. At its core this program uses an iterative method to calculate trajectory of projectiles with aerodynamic, gravitational and coriolis effects. All of the features of the calculator is listed below

**Accounting for Drag:** The program considers the effects of aerodynamic drag on the object.

**Earth's Rotation:** It also takes into account the rotation of the Earth, which can influence trajectories especially over long distances.

**Variable Gravity:** Recognizing that gravity's strength diminishes with altitude, the program adjusts calculations based on changing gravitational forces as an object's altitude shifts.

**Atmospheric Considerations:** Atmospheric properties, such as density and pressure, change as an object climbs or descends in altitude. This program considers these variations for more accurate predictions.

**Mach-Dependent Drag:** The program acknowledges that drag coefficients can change as an object approaches or exceeds the speed of sound (various Mach numbers).

**Buoyancy:** The program will take in the volume and based on the density at its altitude take into account the buoyant force however for low volume objects this force is negligible.

## How it works

The Ballistic Trajectory Calculator can be broken down into three different sections: input, computation and output. In fact the main function used to run the calculator is only three lines of code.

### Input

The input into the program consists of four different sections including Object Parameters, Environmental Parameters, Starting Condition, and Simulation Settings.

##### 1. Object Parameters
This section controls the parameters of the object being launched. Parameters include the object's mass, cross-sectional area, volume, drag coefficient, as well as options for a dynamic thrust curve and drag coefficient. The thrust data is stored in an Excel sheet, which contains the thrust value and the azimuth and elevation of the thrust vector at various times. The Calculator interpolates between these vectors to generate a continuous thrust vector over time. A similar technique is employed for the drag coefficient. Because drag can be challenging to model with precision, a simplified Newtonian model is utilized. To account for transonic and supersonic phenomena, the drag coefficient can be adjusted based on the Mach number.

##### 2. Environmental Parameters
The Environmental Parameters section addresses atmospheric effects such as density, temperature, wind speed, and wind direction. Additional options include a rotating Earth, constant gravity, and choices for the type of atmosphere: static, standard, or location-based.

**Static Atmosphere:** This does not vary, and all its characteristics remain constant.

**Standard Atmosphere:** This utilizes the U.S. 1976 standard atmospheric model, detailing how the pressure, temperature, density, and viscosity of Earth's atmosphere vary with altitude.

**Location-based Atmosphere:** This gleans data from the National Oceanic and Atmospheric Administration (NOAA), drawing on real-world measurements from sounding balloons. Each major US city is designated by a three-letter identifier, which the program uses to source the relevant data.


##### 3. Starting Condition
This section specifies the initial position and velocity of the projectile relative to the Earth's surface. The position is given in terms of latitude, longitude, and height. The velocity vector is defined by its azimuth, elevation, and speed.

##### 4. Simulation Settings
This section outlines parameters that govern the execution of the simulation. The most critical parameter is delta time, which represents the time interval between simulation steps. A smaller delta time results in a more accurate simulation but demands greater computational resources. The "max iteration" parameter sets a limit on the number of iterations the simulation will execute before terminating, ensuring that the simulation doesn't run indefinitely. Similarly, the "max compute time" parameter establishes a cap on the number of real-world seconds the simulation will operate, preventing excessively long runtimes.

### Computation
The computation in the simulation employs an iterative solver that calculates the sum of forces to determine the subsequent velocity and, consequently, the position. The forces acting on the object include: aerodynamic, gravitational, buoyant, and thrust (applied force). The combined effect of these forces is used to ascertain the object's next position and velocity.

### Output

The simulation offers two distinct modes of output presentation.

The first mode provides a series of graphs, as illustrated below:

![](/images/blog/ballistic-trajectory-calculator/plots.png)

The second mode showcases a 3D simulation. Various parameters can be adjusted at the bottom of the page for this mode. For an optimal viewing experience of the animation, it is recommended to watch the associated YouTube video linked below.


<p align="center">
<iframe width="600" height="350"
src="https://www.youtube.com/embed/2kiqtY2RG2o">
</iframe>
</p>


