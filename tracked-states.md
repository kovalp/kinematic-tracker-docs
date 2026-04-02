# Tracked states

After calling the `advance` method, the tracker updates the states $\lbrace\mathbf{x}\rbrace$.
To access the state vectors $\lbrace\mathbf{x}\rbrace$, we have to inspect the Kalman filter 
objects in the tracks stored in the `tracker` object.

For example, to access the first track of the first class  

```python
...
track = tracker.tracks_c[0][0]
```

Note, that there will be at least one object class in the tracker, even if
the tracker object was instantiated without specification of the maximal 
number of classes.

Each track possesses one Kalman filter object (OpenCV `cv2.KalmanFilter` class).
Therefore, we can inspect the posterior state as in the snippet below

```python
...
vec_x = track.kkf.kalman_filter.statePost[:, 0]
```

Note that the internal distribution of variables in the state vectors
are dimension-sorted. It is often more convenient to use [derivative-sorted](derivative-sorting.md)
states.

