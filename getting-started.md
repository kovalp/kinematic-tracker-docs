## Quick look around

```shell
pip install kinematic-tracker
```

```python
from kinematic_tracker.tracker.tracker import NdKkfTracker

tracker = NdKkfTracker([2, 1], [2, 2])
tracker.set_measurement_std_dev(0, 1.0)
tracker.set_measurement_std_dev(1, 2.0)

tracker.advance(0, [[1.0, 2.0, 0.1, 0.2]], [0])
print(len(tracker.tracks))

```




## Best practice: add to your project as dependency

```shell
uv add kinematic-tracker
```


## 