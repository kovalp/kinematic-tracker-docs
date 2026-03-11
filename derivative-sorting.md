# Derivative sorting

Internally, we use a dimension-sorted distribution of variables in the state vector.
For example, if we track two-dimensional boxes with constant-acceleration model
for positions and constant-position model for sizes, then the state vector $\mathbf{x}$
will have the following format

$$
\mathbf{x} = p_x, v_x, a_x, p_y, v_x, a_x, s_x, s_y  \label{eq:a}
$$

This order of variables implies a block structure matrices in Kalman filter.
The block structure facilitates implementation and speeds up the computation
during runtime. However, we typically want the variables and their derivatives 
gathered according to their derivative order, i.e.

$$
\mathbf{x} = p_x, p_y, s_x, s_y, v_x, v_y, a_x, a_y
$$

Such derivative-sorted order is easier to interface with.

To achieve such a conversion (effectively a permutation), we provide a 
derivative-sorting class `DerivativeSorted`.

To instantiate a new object of the class `DerivativeSorted` we 
normally use the tracker object

```python
from kinematic_tracker.nd.derivative_sorted import DerivativeSorted
...
der_sorted = DerivativeSorted(tracker.gen_xz.gen_x)
```

To convert from dimension-sorted to derivative-sorted order, we call the method `.convert_vec`

```python
...
der_sorted.convert_vec(bd_x, ds_x)
```

where `bd_x` is 1D vector in the block-diagonal order as in the first equation, while
`ds_x` is 1D buffer for the derivative-sorted output vector.








