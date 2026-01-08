## Quick look around

The kinematic tracker is available [on PyPI.org](https://pypi.org/project/kinematic-tracker/),
therefore, you can install it using `pip`

```shell
pip install kinematic-tracker
```

Having the tracker installed, we can use the tracker in Pythons scripts.

The package provides one tracker class. We named the class `NdKkfTracker`
to convey its universality (`Nd` for n-dimensional) and the main active component 
(`Kkf` for kinematic Kalman filter). The objects of the `NdKkfTracker` class 
should provide sufficient flexibility to define the state vector composition,
noise covariances, association parameters and perform the tracking.

Consider a simple problem if we need to track rectangles aligned with Cartesian axes.
Casually, we decide to encode rectangles by positions of their centers and sizes.

$$
x = 5
$$


```python
from kinematic_tracker.tracker.tracker import NdKkfTracker
import numpy as np

tracker = NdKkfTracker([2, 1], [2, 2])
tracker.set_measurement_std_dev(0, 1.0)
tracker.set_measurement_std_dev(1, 2.0)

tracker.advance(0, [np.array([1.0, 2.0, 0.1, 0.2])], [0])

```
