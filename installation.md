# Installation

The tracker needs OpenCV (`cv2`), but does not force a particular OpenCV distribution.

## Quick and easy using `pip`

The package is uploaded to the PyPI registry [kinematic tracker](https://pypi.org/project/kinematic-tracker/)

If your environment already has OpenCV (`opencv-python`, `opencv-python-headless`,
`opencv-contrib-python`, ...), just install the tracker and it will use that OpenCV

```shell
pip install kinematic-tracker
```

Otherwise, let the tracker bring OpenCV via an extra: `headless` (no GUI) or `gui` (with HighGUI)

```shell
pip install kinematic-tracker[headless]
```

Do not install several OpenCV distributions side by side: they share the same `cv2` module
and break each other. Importing the tracker without any OpenCV raises an `ImportError`
explaining the options.

## Best practice

To use the kinematic tracker in your projects we recommend adding it as 
a dependency. Using `uv` as the package manager, if the project already depends
on an OpenCV distribution, then

```shell
uv add kinematic-tracker
```

Otherwise, add it with an extra providing OpenCV: `headless` or, if you need
a GUI-enabled OpenCV, `gui`

```shell
uv add kinematic-tracker --extra headless
```

## Examples in unit tests

A number of simple examples are available in the `test/tutorials` folder.
The `test` folder is available in the source package 

```shell
pip download kinematic-tracker --no-binary :all: --no-deps
```
