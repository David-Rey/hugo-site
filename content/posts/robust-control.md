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
