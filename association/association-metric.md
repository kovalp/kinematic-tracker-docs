# Association metric

Association metric is a matrix of likelihoods $A_{rt}$ between sets of detection
reports (vectors) $\mathbf{z} _r $ and target (predicted) states $\mathbf{x} _t $.
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

Defined via a so-called Mahalanobis distance [@ Wikipedia](https://en.wikipedia.org/wiki/Mahalanobis_distance).

See our discussion of the [Mahalanobis association metric](mahalanobis)

## GIoU for cuboids aligned with Cartesian axes

Name for the setter `giou-axes-aligned`.

Defined via a ratio of intersection and union volumes of cuboids
[@ Medium](https://medium.com/analytics-vidhya/iou-intersection-over-union-705a39e7acef).

See our discussion of the [GIoU axes aligned metric](giou-axes-aligned).


## GIoU for yaw-oriented cuboids

Name for the setter `giou-yaw`.

See our discussion of the [GIoU yaw metric](giou-yaw).


## Mahalanobis with size-modulated covariance

Name for the setter `mahalanobis-size-modulated`.

This metric is experimental. 
See our discussion of the [size-modulated Mahalanobis](mahalanobis-size-modulated).


## GIoU-calibrated diagonal size-modulated Mahalanobis distance inside the exponent function

Name for the setter `iou-like-two-yaw`.

This metric is experimental. 
See our discussion of the [iou-like-two-yaw](iou-like-two-yaw).
