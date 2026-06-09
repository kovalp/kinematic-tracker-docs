# 2D tracking of bounding boxes

Bounding boxes are used in computer vision applications extensively.
The `kinematic-tracker` can be used to track the bounding boxes.

Let's assume we encoded bounding boxes via coordinates of their centers and their sizes

$$
\mathbf{z} = (p_x, p_y, s_x, s_y)
$$

Here first two variables $p_x, p_y$ refer to the Cartesian coordinates of the box center (position).
The last two variables $s_x, s_y$ carry the sizes of the bounding box.

A popular choice to track such detections is to use the constant-velocity Kalman filter
for coordinates $p_x, p_y$ and constant-position filters for sizes $s_x, s_y$.
We can define the corresponding tracker

```python
from kinematic_tracker import NdKkfTracker

tracker = NdKkfTracker([2, 1], [2, 2])
```

To start tracking we need to set the measurement covariance

```python
...
tracker.set_measurement_cov( 5.0 * np.eye(4))
```

A popular association metric for 2D bounding boxes is generalized intersection over union (GIoU).

```python
...
tracker.set_association_metric('giou-axes-aligned')
```

We call the `advance` method to track detections

```python
...
stamp_ns = 1000_000_000
det_rz = np.array([[0., 1., 2., 3.]])
tracker.advance(stamp_ns, det_rz)
```

The tracks can be inspected as usual

```python
...
print(tracker.tracks_c[0][:])
```
