# Generalized intersection over union between axes-aligned objects

Under **objects** we understand cuboids in any dimension: segments, bounding boxes, cuboids, etc.

The version for the cuboids aligned with Cartesian axes needs the detection vectors to have
at least 6 variables.

In difference to the Mahalanobis metric where the variables are treated uniformly,
we need to identify which variables represent *position* of the cuboids and which
variables represent *sizes* of the cuboids. In order to define this, we 
use the keyword argument `ind_pos_size` of the method `.set_association_metric()`

```python
...
tracker.set_association_metric('giou-axes-aligned', ind_pos_size=(0,1,2,3,4,5))
```

The keyword argument `ind_pos_size` defines indices of three position variables
and three size variables. If the programmer omits the keyword argument `ind_pos_size`, 
then sequence falls back to `ind_pos_size=(0,1,2,-3,-2,-1)` in 3D case.
This choice encodes first variables in the measurement vector being positions and last three variables corresponding to sizes

$$
\mathbf{z} = p_x, p_y, p_z, \text{any number of variables}, s_x, s_y, s_z.
$$

Such a choice fits the data layout of several datasets 
including the popular nuScenes dataset. See [our suggestion](../track-nu-scenes)
for tracking nuScenes annotations. The performance of the axes-aligned GIoU
is suboptimal comparing to GIoU for oriented cuboids.

In general case, the default value of the fancy indices `ind_pos_size` are constructed based on the dimensionality of the first variable.
For example, if the tracker is constructed `tracker = NdKkfTracker([3, 1, 1], [2, 1, 2])`, then the dimensionality of the first variable is `2`.
In this case, the default fancy indices will be `ind_pos_size=(0, 1, -2, -1)`.
Note, that this convention is similar to the 3D case, which is suitable for nuScenes dataset. 