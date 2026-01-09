## Quick intro

The `kinematic-tracker` is available [on PyPI.org](https://pypi.org/project/kinematic-tracker/),

Having the tracker installed, we can use the tracker class `NdKkfTracker`.
The class `NdKkfTracker` allows to define a state $\mathbf{x}$ composed of a set of N-dimensional
point-motion models. For example, to track bounding boxes in 2D using constant-velocity
for their centers and constant-position for their sizes we could play with the following script 

```python
from kinematic_tracker.tracker.tracker import NdKkfTracker
import numpy as np

tracker = NdKkfTracker([2, 1], [2, 2])
tracker.set_measurement_cov( 4 * np.eye(4))
tracker.advance(12345, [np.array([1.0, 2.0, 0.1, 0.2])])
print(tracker.tracks)
```

In this script we defined the tracker to have states $\mathbf{x}$ consisting of two 2D *parts*: positions and sizes.
The layout of the state $\mathbf{x}$ will be $\mathbf{x} = (p_x, v_x, p_y, v_y, s_x, s_y)$.

The measurement vector $\mathbf{z}$ excludes any derivatives by default $\mathbf{z} = (p_x, p_y, s_x, s_y)$.   

We used a diagonal measurement noise covariance $\mathrm{R}=4.0 \mathrm{I}$.

At first step, we provided one measurement vector at time stamp `12345` nanoseconds.    

As a result we should be able to see current list of targets which includes just one track

```terminaloutput
[TrackNdKkf(x = [1.  0.  2.  0.  0.1 0.2])]
```

Internally, we use OpenCV Kalman filters. We can access other variables of the target
such as its prior and posterior error covariances

```python
...
print(tracker.tracks[0].kkf.kalman_filter.errorCovPre)
print(tracker.tracks[0].kkf.kalman_filter.errorCovPost)
```
