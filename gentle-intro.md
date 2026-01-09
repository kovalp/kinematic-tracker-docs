## Gentle intro

The kinematic tracker is available [on PyPI.org](https://pypi.org/project/kinematic-tracker/),
therefore, you can install it using `pip`

```shell
pip install kinematic-tracker
```

Having the tracker installed, we can use the tracker in Pythons scripts.

The package provides one tracker class. We named the class `NdKkfTracker`
to convey its universality (`Nd` for n-dimensional) and the main active component 
(`Kkf` for kinematic Kalman filter). The objects of the `NdKkfTracker` class 
should provide sufficient flexibility to define the state vector composition,
noise covariances, association parameters and perform the tracking.

Consider a simple problem if we need to track rectangles aligned with Cartesian axes.
Casually, we decide to encode rectangles by positions of their centers and sizes.

<div align="center" style="background: white; padding: 20px; border-radius: 8px;">
{% include rectangle-aligned.svg %}
</div>

Moreover, we assume the sizes of the rectangles are mostly constant while the position is 
subject for change. Such rectangles could be representing bounding boxes for cars or 
pedestrians observed from the zenithal projection &#128161;

The class `NdKkfTracker` allows to define a state $\mathbf{x}$ composed of position, velocities
and sizes of such rectangles. Each state would represent one rectangle moving in the direction $(v_x, v_y)$.

$$
\mathbf{x} = (p_x, v_x, p_y, v_y, s_x, s_y)
$$

In order to define such a tracker, we will need to split the positions and sizes of the rectangles
and consider them separately with different point-motion models: the positions will obey
a constant-velocity (CV) model, while the sizes will obey a constant-position (CP) model. The CV 
point-motion model operates on two variables: coordinate and velocity. Therefore, we say
its kinematic order is 2. The CP point-motion model operates on one variable: coordinate. Therefore,
we say its kinematic order is 1. To describe the rectangles, we need two dimensions. In order 
to convey this choice, we initialize the tracker object with a list of kinematic orders `[2, 1]`
and a list of dimensions `[2, 2]` for positions and sizes:

```python
from kinematic_tracker.tracker.tracker import NdKkfTracker

tracker = NdKkfTracker([2, 1], [2, 2])
```

Note, that our state vector $\mathbf{x}$ will consist of two *parts*: positions and sizes.
Both parts are two-dimensional. However, the positions will be treated with CV point-motion model,
while the sizes will be treated with CP point-motion models.
Internally, the `tracker` will create one Kalman filter for each target.

By default, the measurement vectors $\mathbf{z}$ will include only coordinates for each part, i.e.

$$
\mathbf{z} = (p_x, p_y, s_x, s_y)
$$

A target (holding the state $\mathbf{x}$ among other things) will be initialized every time
a measurement is not matched with any targets. Since there are no targets in the freshly
initialized tracker, it will initialize one target per measurement at the first invocation.
However, before we can successfully invoke the tracking, we should first define a measurement
noise covariance $\mathbf{R}$. 

The measurement noise covariance $\mathbf{R}$ is a square 4-by-4 matrix in our case.
Typically, very little known about an optimal choice of the measurement covariance at first.
Moreover, it could be changing from step-to-step and from object-to-object in the most general case. 
The class `NdKkfTracker` provides a possibility to define one measurement covariance
for all objects and time steps. Moreover, the simplest setter of the measurement covariance
`tracker.set_measurement_cov` allows to define the matrix $\mathbf{R}$ directly.
So, the line 

```python
tracker.set_measurement_cov( 4 * np.eye(4))
```

would define $\mathbf{R}$ to be a diagonal matrix

$$
\mathbf{R} = \begin{pmatrix}
\sigma^2 & 0 & 0 & 0 \\
0 & \sigma^2 & 0 & 0 \\
0 & 0 & \sigma^2 & 0 \\
0 & 0 & 0 & \sigma^2
\end{pmatrix}
$$

with standard deviation $\sigma=2$.

Once the measurement noise covariance is defined, we can start tracking

```python
tracker.advance(0, [np.array([1.0, 2.0, 0.1, 0.2])])
```

The method `tracker.advance()` takes timestep in nanoseconds as first argument.
The list of measurement vectors $[\mathbf{z}_0, \mathbf{z}_1 \ldots \mathbf{z}_n]$ 
should be provided in the second argument. The list of measurements could be given
as two-dimensional NumPy array as well.

After the call, the tracker will have one target with the state $\mathbf{x} = (1.0, 0.0, 2.0, 0.0, 0.1, 0.2)$. 

```python
print(tracker.tracks)
```

```terminaloutput
[TrackNdKkf(x = [1.  0.  2.  0.  0.1 0.2])]
```

We can access other variables of the target such as its prior and posterior error covariances

```python
print(tracker.tracks[0].kkf.kalman_filter.errorCovPre)
```

```terminaloutput
array([[  4.,   0.,   0.,   0.,   0.,   0.],
       [  0., 100.,   0.,   0.,   0.,   0.],
       [  0.,   0.,   4.,   0.,   0.,   0.],
       [  0.,   0.,   0., 100.,   0.,   0.],
       [  0.,   0.,   0.,   0.,   4.,   0.],
       [  0.,   0.,   0.,   0.,   0.,   4.]])
```

Note, that we use OpenCV Kalman filters internally.

The output shows that the position part of the error covariance is initialized from the 
measurement noise covariance, while the velocity part got a large variance `100`.
The initial variances for the derivatives in all parts and dimensions is predefined 
in the tracker in a vector `tracker.ini_der_vars`. The size of the vector is 
equal to the number of variables in the measurement vector (4). If the vector 
is adjusted, then only the new targets will get updated initial variances.
