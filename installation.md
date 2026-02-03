# Installation

## Quick and easy using `pip`

The package is uploaded to the PyPI registry [kinematic tracker](https://pypi.org/project/kinematic-tracker/)

So, we can easily install it using `pip`

```shell
pip install kinematic-tracker
```

## Best practice

To use the kinematic tracker in your projects we recommend adding it as 
a dependency. Using `uv` as the package manager, the adding can be done
as simple as running  

```shell
uv add kinematic-tracker
```

If you happen to need GUI-enabled version of OpenCV, then add `opencv-python` as dependency

```shell
uv add opencv-python
```

and add to your `pyproject.toml` this `exclude-dependencies` list

```toml
[tool.uv]
exclude-dependencies = ["opencv-python-headless"]
```


## Examples in unit tests

A number of simple examples are available in the `test/tutorials` folder.
The `test` folder is available in the source package 

```shell
pip download kinematic-tracker --no-binary :all: --no-deps
```
