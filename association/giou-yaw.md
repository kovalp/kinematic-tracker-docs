# GIoU for oriented cuboids 

Our implementation works only for 3D case. The version for the cuboids 
aligned with Cartesian axes needs the detection vectors to have
at least 8 variables.

In difference to the Mahalanobis metric where the variables are treated uniformly,
we need to identify which variables represent *position* of the cuboids,
which encode the orientation and which variables represent *sizes* of the cuboids.
In order to define this, we use the keyword argument
`ind_pos_w_rz_size` of the method `.set_association_metric()`

```python
...
tracker.set_association_metric('giou-yaw', ind_pos_w_rz_size=(0,1,2,3,6,7,8,9))
```

The keyword argument `ind_pos_w_rz_size` defines indices of three position variables,
the position of scalar part of the rotation quaternion $w$, position of the 
z-projection of the rotation quaternion $r_z$ and three size variables.
When the argument is omitted, the sequence is `ind_pos_w_rz_size=(0,1,2,3,6,-3,-2,-1)`.
Such a choice fits the data layout of the popular nuScenes dataset.
See [our suggestion](track-nu-scenes) for tracking nuScenes annotations.

