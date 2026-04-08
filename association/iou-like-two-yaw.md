# IoU-yaw calibrated, Mahalanobis-based metric

This is another attempt to learn from the successes of the IoU-based metrics.
The first attempt is our [size-modulated Mahalanobis metric](mahalanobis-size-modulated).

The metric uses the concept of Mahalanobis distance between two measurements
$a_i$ and $b_j$

$$D_{ij} = \sqrt{(a_i - b_j) C_{ij}^{-1} (a_i - b_j)},$$

where the covariance $C_{ij}$ is assumed to be diagonal matrix, and we omit the measurement
dimension indices for brevity. In order to imitate the IoU-based metrics, we modulate
the covariance with average size of the objects. The size of the objects should be a part
of measurement vectors. Instead of covariance, we parametrize its inverse
(precision) matrix $P$. This replaces the definition of the Mahalanobis distance
trivially

$$D_{ij} = \sqrt{(a_i - b_j)^2 \cdot P_{ij}}.$$

The precision matrix $P_{ij}$ depends on the pair of the measurement vectors $ij$
and is diagonal. The measurement vectors $a$ and $b$ have three positional components,
two yaw quaternion components and three size components

$$
a = p_x, p_y, p_z, w, r_z, s_x, s_y, s_z.
$$

The yaw $\theta$ quaternion is vector of size 2 composed of scalar and z-component of the whole quaternion,
i.e. $w = \cos(\theta / 2), r_z = \sin(\theta / 2)$.

The precision matrix $P_{ij}$ is scaled with inverse square sum sizes in its
position- and size parts with position and size calibration constants.
The quaternion part is given by a quaternion calibration constant.

Let's define a scale vector of three elements

$$\mathrm{scale}_{ij} = \frac{1}{(si_x + sj_x)^2}, \frac{1}{(si_y + sj_y)^2}, \frac{1}{(si_z + sj_z)^2}.$$

The precision matrix expressed in terms of the scale vectors

$$
P_ij = \mathrm{pos}_c \mathrm{scale}_{ij}, \mathrm{quat}_c, \mathrm{quat}_c,
\mathrm{size}_c \mathrm{scale}_{ij},
$$

where $\mathrm{pos}_c$, $\mathrm{quat}_c$, and $\mathrm{size}_c$, are calibration constants.

The square difference $(a - b)^2$ is computed element-wise for the positions and
sizes, while the quaternion part is computed as a doubly wrapped geodesic distance
$d_{ij}$ between two quaternions:

$$
(a - b)^2_{ij} = (pi_x - pj_x)^2, (pi_y - pj_y)^2, (pi_z - pj_z)^2,
               d_{ij}^2, d_{ij}^2,
               (si_x - sj_x)^2, (si_y - sj_y)^2, (si_z - sj_z)^2,
$$

where $d_{ij}$ is the wrapped double geodesic distance

$$
d_{ij} = 4 * \arccos(|w_i w_j + rz_i rz_j|),
$$
$$
d_{ij} = d_{ij} \text{ if } d_{ij} < \pi \text{ else } 2 \pi - d_{ij}.
$$

The reason for wrapping is to imitate the behavior of IoU metric with
respect to rotation. Namely, the IoU metrics give the same values for
yaw angle $\theta$ and $\theta + \pi$.

Using the Mahalanobis distance $D_{ij}$, the association metric $A_{ij}$ is
computed using the negative exponent (not Gaussian) of the distance

$$
A_{ij} = \exp(-D_{ij}).
$$

The choice of exponent function instead of the regular, bell-shaped dependency
is for the sake of better mimicking the IoU metric. Namely, the IoU metrics
decrease faster at small shifts than the Gaussian delivers.

We calibrate the metric using three reference pairs of points:

  - for translation: a = (3,0,0,1,0,1,1,1), b = (0,0,0,1,0,1,1,1)
  - for rotation: a = (0,0,0,1,0,4,2,1), b = (0,0,0,$\sqrt{2}/2$,$\sqrt{2}/2$,4,2,1)
  - for scale: a = (0,0,0,1,0,1,1,1), b = (0,0,0,1,0,2,2,1)

The condition for the calibration constants $\mathrm{pos}_c$, $\mathrm{quat}_c$ and
$\mathrm{size}_c$ is the same

$$
A_{ij} = GIoU_{ij}.
$$

Advantages of the IoU-yaw calibrated metric:

  - Easy to extend to full 3D case.
  - Simple definition enables a fast implementation vectorized in both indices.
  - It is easy to tune the metric in difference to the reference IoU metric.
  - Better than the reference GIoU-yaw metric (4x less false associations in nuScenes mini subset).
