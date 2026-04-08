# Matching algorithm

After calculation of the association likelihoods (metric) $A_{rt}$,
we establish a bipartite matching between reports and targets.
The bipartite matching is a list of report--targets pairs defining
which report will be used for which target in the update step
(of the Kalman filtering).

There are two matching algorithms available:
  
  - Greedy algorithm `greedy`.
  - Hungarian or linear-sum-assignment method `hungarian`.

See a brief discussion on the matching methods below.

By default, we select the Hungarian matching method. In order to select 
the greedy matching method, we should use the setter `.set_association_method(name)`

```python
from kinematic_tracker import NdKkfTracker

tracker = NdKkfTracker([2, 1], [2, 2])
tracker.set_association_method('greedy')
...
```

In order to set the association threshold alleviating the problem of clutter, it
is sufficient to directly redefine the corresponding attribute in the tracker

```python
...
tracker.association.threshold = 0.2345
```

Both matching algorithms are affected by the association threshold.

## Greedy

Greedy algorithm is a popular choice since its definition is way simpler 
than that for the linear-sum assignment.

Namely, we search maximum value of the association metric $A_{rt}$,
and mark the found row--column pair as associated excluding it from the 
search in the following steps.
[Greedy algorithm @ Wikipedia](https://en.wikipedia.org/wiki/Greedy_algorithm)

Greedy algorithm might perform better than Hungarian in rare cases when 
the association metric is too permissive or when the detections of low quality. 

## Hungarian

The linear-sum assignment is another popular choice. We refer to this matching 
method as `hungarian` because of the conventions accepted in the community.

The definition involves a combinatorial optimization of the determined 
correspondences such that the sum of the metric values is maximized.
[Hungarian algorithm @ Wikipedia](https://en.wikipedia.org/wiki/Hungarian_algorithm)

Needless to say that an acceptable implementation of the optimization problem involves
rather complicated methods of discrete mathematics.

Generally, the hungarian method provides superior performance comparing to
the greedy method.
