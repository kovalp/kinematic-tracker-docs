# Generalized intersection over union between axes-aligned cuboids

Our implementation works only for 3D case. The version for the cuboids 
aligned with Cartesian axes needs the detection vectors to have
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
and three size variables. When the argument is omitted, the sequence is 
`ind_pos_size=(0,1,2,-3,-2,-1)`. This choice encodes first variables in the 
measurement vector being position and last three variables corresponding dimensions

$$
\mathbf{z} = p_x, p_y, p_z, \text{any number of variables}, s_x, s_y, s_z.
$$

Such a choice fits the data layout of several datasets 
including the popular nuScenes dataset. See [our suggestion](../track-nu-scenes)
for tracking nuScenes annotations. The performance of the axes-aligned GIoU
is suboptimal comparing to GIoU for oriented cuboids.


