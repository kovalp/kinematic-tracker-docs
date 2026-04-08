# Association metric

Association metric is a matrix of likelihoods $A_{rt}$ between sets of detection
reports (vectors) $\mathbf{z}_r$ and target (predicted) states $\tilde{\mathbf{x}}_t$.
Number of detections and targets generally differ, i.e. the association metric is
a rectangular matrix $A_{rt}$.

There are several definitions of the association metrics implemented in the 
kinematic tracker.

  - Mahalanobis distance inside the gaussian function with sum of process and detection covariances.
  - Generalized intersection over union (GIoU) for cuboids aligned with cartesian axes.
  - GIoU for cuboids oriented with the yaw angle.
  - Mahalanobis distance inside the gaussian function with size-modulated covariance.
  - GIoU-calibrated diagonal size-modulated Mahalanobis distance inside the exponent function.

The association metrics are defined below. In order to select a metric, we provide
a setter method `.set_association_metric(...)`, e.g.

```python
from kinematic_tracker import NdKkfTracker

tracker = NdKkfTracker([2, 1], [2, 2])
tracker.set_association_metric('giou-axes-aligned')
```

The list of accepted metric names can be obtained from the enumerable class
`AssociationMetric`.

```python
from kinematic_tracker.association.association_metric import AssociationMetric

print([m.value for m in AssociationMetric])
```

## Mahalanobis association metric

Name for the setter `mahalanobis`.

Defined via [Mahalanobis distance](https://en.wikipedia.org/wiki/Mahalanobis_distance) $D$.

See our discussion of the [Mahalanobis association metric](association/mahalanobis)

## GIoU for cuboids aligned with Cartesian axes

Name for the setter `giou-axes-aligned`.




