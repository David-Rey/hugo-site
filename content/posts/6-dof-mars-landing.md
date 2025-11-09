---
  title: "6 Degree of Freedom Optimal Mars Landing"
  date: 2025-03-03T12:00:00+06:00
  featured: false
  tags: "tech"
  tranding: true
  readTime: "2 min"
  thumbnail: /images/blog/6-dof-mars-landing/featureImage.png
  featureImage: /images/blog/6-dof-mars-landing/featureImage.png
  math: true
---

While taking Senior Design at Georgia Tech me and a 8 other team members had to deign a spacecraft to land on mars. All the high level analysis needed for an actual mission

[Overview]

| Section | Description / Equation |
| :--- | :--- |
| **Cost Function:** | $\min_{t_f, \vec{u}} -m_f$ |
| **Initial Conditions:** | $\vec{x}(0) = \vec{x}_0$ |
| **Terminal Conditions:** | $\vec{x}(t_f) = \vec{x}_f$ |
| **Dynamics:** | $\dot{m} = -\frac{1}{I_{sp}g_0} \sum_{i=1}^{n_u} u_i$ |
| | $\dot{\vec{r}} = \vec{v}$ |
| | $\dot{\vec{v}} = \frac{1}{m} C_I^B \sum_{i=1}^{n_u} \hat{d}_i u_i + \vec{g}$ |
| | $\dot{\vec{q}} = \frac{1}{2} \Omega(\vec{\omega})\vec{q}$ |
| | $\dot{\vec{\omega}} = J_B^{-1} \sum_{i=1}^{n_u} \vec{l}_i \times (\hat{d}_i u_i) - \vec{\omega} \times (J_B \vec{\omega})$ |
| **Control Constraints:** | $u_{\min} < u_i < u_{\max} \quad \forall i \in [1, n_u]$ |

---

#### **Explanation of Variables:**

* $m$: Mass
* $t_f$: Final time
* $\vec{u}$: Control input vector (e.g., thrust)
* $\vec{x}$: State vector
* $I_{sp}$: Specific impulse
* $g_0$: Standard gravity
* $n_u$: Number of control inputs
* $\vec{r}$: Position vector
* $\vec{v}$: Velocity vector
* $C_I^B$: Direction Cosine Matrix (Inertial to Body frame)
* $\hat{d}_i$: Thrust direction unit vector for $i$-th thruster
* $\vec{g}$: Gravity vector
* $\vec{q}$: Quaternion (attitude representation)
* $\vec{\omega}$: Angular velocity vector
* $\Omega(\vec{\omega})$: Skew-symmetric matrix or equivalent operator for quaternion kinematics
* $J_B$: Inertia tensor (Body frame)
* $\vec{l}_i$: Lever arm vector for $i$-th thruster
* $u_{\min}, u_{\max}$: Minimum and maximum control limits

[easy explanation]

[SCvx and tutorial]
{{< video src="/images/blog/6-dof-mars-landing/rocket_animation.mp4" width="700" >}}


[Results]

