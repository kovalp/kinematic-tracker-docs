# Changelog

# 24.0.0

  - Using `BufView` class from the external package.

# 23.0.0

  - Any dimension in the IoU for aligned cuboids.

# 22.0.0

  - Vectorize the aligned IoU association metric. 


# 21.0.0

  - No `max_targets` or `max_reports` attributes, removed the setter.


# 20.10.0

  - Add typechecking via `ty` and fix typehints.

# 20.9.0

  - Reset the creation ID.

# 20.8.0

  - Avoid inversion of covariance in the Mahalanobis metric by solving linear equation.


# 20.7.0

  - Correct the use of `arccos` function to be compatible with `numpy<=2.0.0`.
  - Test with `numpy<=2.0.0`.

# 20.6.0

  - Define, debug and optimize the implementation of IoU-like-two-yaw metric.
  - Make the new experimental metric (IoU-like-two-yaw) available.
  - Remove the dangling average dim IoU metric.
  - Add argument `ind_pos_w_rz_size` to customize the GIoU-yaw and IoU-like two yaw metrics.
  - One less temporary array in the real IoU yaw metric.

# 20.5.0 (yanked)

  - Add an experimental IoU-like two yaw association metric.
  - Update (development) dependencies.

# 20.4.1

  - Define range of Python versions >=3.11.
  - Remove slow code for GIoU, and GIoU-like.

# 20.3.0

  - Using vectorized convex hulls from `shapely`.
  - Using `shapely.area` u function.
  - Using `shapely.multipoints` u function.
  - Using `shapely.polygons` u function.
  - Vectorizing the intersection and union in 1D.
  - Do not repeat computation of the first volume.
  - Added `F3T` typing alias.

# 20.2.0

  - Start to exploit the vectorized capabilities of `shapely`.

# 20.1.0

  - Bugfix: ensure the number of detections can be zero.

# 20.0.0

  - Add yaw-dependent GIoU 3D metric `giou-yaw`.
  - Rename `size-modulated-mahalanobis` to `mahalanobis-size-modulated`.
  - Rename `giou` -> `giou-axes-aligned`. 
  - Remove the unknown metrics and matching algorithms.
  - Remove mean-dim GIoU metric.

# 19.3.0

  - Check the annotation indices for type, dimensions, data type and size.
  - Return deepcopy of the `ScoreCopy` objects.
  - Return `StubScoreCopy` if scores are not needed (faster).
  - Measurement covariance is allocated at instantiation. NaN on diagonal to enforce definition.
  - Possibility of association within given object classes.
  - Refactor the `ScoreCopy` class for multiple classes.
  - Demand `return_score` flag at tracker instantiation.
  - Remove `return_score` flag from the keyword arguments of the `.advance` method.
  - Remove `.tracks` (list of tracks) from tracker (breaking change for most consumers).
  - Add array of lists of tracks as `.tracks_c` attribute.

# 19.0.0, 19.1.0, 19.2.0

  - Yanked as not completely usable.

# 18.0.4

  - Include OpenCV via the mechanism of optional dependencies (extras).
  - Adjust the dependencies to tolerate "opencv-python>=4.11.0.86". 

# 18.0.3

  - Adjust the `pyproject.toml` to explicitly tolerate "numpy>=1.22".

# 18.0.2

  - Update `opencv-python-headless` dependency since it is not linking against X11 anymore.

# 18.0.1

  - Declare `opencv-python-headless` as dependency, not `opencv-python`.
  - Add "Use the kinematic-tracker as dependency" section in the README.

# 17.0.1

  - Optimize the dependencies: relax numpy version constrains. 

# 17.0.0

  - The advance method returns the score (metric) if the keyword argument `return_score=True`.
  - The advance method returns either `ScoreCopy` object or `None`.
  - The advance method does not return the list of tracks. Tracks are always available via the attribute.
  - Update the dependencies.

# 16.1.0

  - Reset method in the tracker.
  - Shortcut for importing the tracker class `from kinematic_tracker import NdKkfTracker`.
  - Update the dependencies.

# 16.0.0

  - Assert the number of variables in the measurement vector is greater than 5 in GIoU aligned.
  - Change the default association metric to be Mahalanobis (previously GIoU).
  - Link to the documentation on GitHub Pages.
  - Make the annotation IDs optional.
  - Improve typing hints in the method `NdKkfTracker.advance()`.

# 15.7.1

  - Improve the definition of the dependencies to avoid compilation on Python 3.14


# 15.7.0

  - Introduce generator of measurement vectors in `MetricDriverBase` class.
  - Precompute the GIoU auxiliaries in aligned GIoU driver.
  - Setter for the max number of reports and targets.
  - Suggest a dimension-averaged GIoU association metric.
  - Improve type annotations in metric drivers.
  - Improve testing of the current definition of the size-modulated Mahalanobis.

# 15.4.0

  - Suggest a size-modulated Mahalanobis metric.
  - Improve the computation of the regular Mahalanobis metric.
  - Relax requirements for fancy indexes in the tracker.
  - Add possibility to set the fancy indices and metric type at once.
  - Assert that empty detections are acceptable for all metric drivers.

# 15.0.1

  - Using the package `association-quality-clavia`.
  - Published in PyPI registry.

# 14.2.0

  - Using the package `binary-classification-ratios`.

# 14.1.2

  - Compatibility with nuScenes @ `evaluation-against-nuscenes` (Python >=3.10 & NumPy >=1.26.4).

# 14.1.0

  - Added an association-threshold setter in `NdKkfTracker`.

# 14.0.1

  - Corrected the example of tracking with yaw angle affected by the version 14.
  - Made the example of tracking association quality more robust.

# 14.0.0
    
This version bump is major because it breaks a silent convention on the distribution 
of variables in the measurement vector. In previous versions, the measurement vector
has been distributed as follows:

     $$z^T = (p_x, p_y, p_z, s_x, s_y, s_z, ...)$$

In this version, the distribution of variables in the measurement vector is as follows:

     $$z^T = (p_x, p_y, p_z, ..., s_x, s_y, s_z)$$

where the position variables are listed first, followed by the size variables.
This change makes the tracker more convenient to use with ASAM OpenLabel frames.

  - Possibility to adjust the distribution of variables in the measurement vector.
  - Added setter `NdKkfTracker.set_ind_pos_size`.

# 13.0.1
    
  - Proofread the README file.
  - Renamed `gen_z` --> `gen_xz` in tracker.

# 12.1.0

  - Remove the unfinished `GIoUWithYaw` class.
  - Add all non-trivial docstrings.
  - Eliminate the flip in Greedy matching.
  - Eliminate unused buffer in the Mahalanobis metric driver.
  - Remove the obsolete method `NdKkfTrack.get_det_cov()`. 

# 12.0.0

  - All dependencies are public from PyPI.

# 11.1.2

  - Add docstrings in `nd` modules
  - Use documented version of the kinematic-matrix package (viulib-kinematic-matrices>=0.3.0)


# 11.1.0

  - Allow both `int` and `np.int64` to be used as time stamps in the `Tracker.advance` method.
  - Add three tutorial examples:
    - tracking of cuboids aligned with Cartesian axes
    - tracking of cuboids with yaw angle
    - tracking and classifying the association quality

# 11.0.0

  - Introduced the (bipartite) matching drivers:
    - Maximal shape of the metric is rectangular
    - The returned match is in fixed-size numpy arrays which stay the same across time stamps
    - Auxiliary buffers are used instead of temporary (scrap) RAM
  - Remove the calculation of threshold from the association probability 

# 10.4.0

  - Added nuScenes example
  - Refactored the metric drivers:
    - Maximal shape of the metric is rectangular
    - Metric is computed in a chunk of memory at the beginning of the rectangular buffer
    - The returned matrix is rectangular, local and shares memory with the buffer
    - Removed O(N) buffers for $Hx$ and $HPH^T$
    - The order of indices is (reports, targets) resulting in wide matrices most of the time
    - Auxiliary buffers are used instead of temporary (scrap) RAM 

# 10.3.0

 - Add ClassificationRatios helper class
 - Add fusion-lab example of two almost colliding cuboids
 - Add padding to the Hungarian matching
 - Association-quality- and Cartesian-quality helper classes in unit tests

# 10.2.0

 - Add AssociationQuality classifier
 - Change default association threshold from 0.9 --> 0.25
 - Fix finite-diff adaptive noise to multiply with the variance factor
 - Add setter NdKkfTracker.set_measurement_std_dev(...)

# 10.0.0

 - Removed GatherAtStart in favor of DerivativeSorted

# 2.0.2

 - Started GIoU with yaw

# 1.1.2
  
 - Initial release
