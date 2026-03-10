# 3D tracking (for nuScenes format)

[nuScenes](https://www.nuscenes.org/) is a popular self-driving dataset.

The 3D annotations in nuScenes adhere to the format

$$
\mathbf{z} = (p_x, p_y, p_z, w, r_x, r_y, r_z, s_x, s_y, s_z)
$$

Here first three variables $p_x, p_y, p_z$ refer to the Cartesian coordinates of the box center
(position). Next four variables $w, r_x, r_y, r_z$ encode the orientation quaternion.
Finally, the last three variables $s_x, s_y, s_z$ carry the sizes of the bounding box.

A popular choice to track such detections is to use the constant-acceleration Kalman filter
for coordinates $p_x, p_y, p_z$ and constant-position filters for quaternions
$w, r_x, r_y, r_z$ and sizes $s_x, s_y, s_z$. We can define the corresponding tracker

```python
from kinematic_tracker import NdKkfTracker

tracker = NdKkfTracker([3, 1, 1], [3, 4, 3])
```

To start tracking we need to set the measurement covariance

```python
...
tracker.set_measurement_cov( 0.25 * np.eye(10))
```

A popular association metric for 3D bounding boxes is generalized intersection over union (GIoU).
We provide two flavors of GIoU: with yaw angle and axes-aligned.   

```python
...
tracker.set_association_metric('giou-yaw')
```

We call the `advance` method to track detections

```python
...
stamp_ns = 1000_000_000
det_rz = np.array([[0., 0., 0., 1., 0., 0., 0., 4., 3., 2.]])
tracker.advance(stamp_ns, det_rz)
```

The tracks can be inspected as usual

```python
...
print(tracker.tracks_c[0][:])
```

Note: the quaternions are suitable for tracking with simple linear Kalman filters.
However, the normalization of the orientation part of the state vector is not guaranteed.
On another side, normalization of the quaternions does not affect orientation, 
you might not need to normalize the tracked quaternions after all.
