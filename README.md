# bdk-python
The Python language bindings for the [bitcoindevkit](https://github.com/bitcoindevkit).

See the [package on PyPI](https://pypi.org/project/bdkpython/).

Install the latest release using
```shell
pip install bdkpython
```

## Run the tests
```shell
python -m tox
```

## Build the package
```shell
python -m build
```

## Install locally
```shell
pip install ./dist/bdkpython-0.0.1-py3-none-any.whl
```

## Known issues
Note that until the fix is merged upstream in [uniffi-rs](https://github.com/mozilla/uniffi-rs), the `loadIndirect()` function in the `bdk.py` module must be replaced with the following:
```python
def loadIndirect():
    if sys.platform == "linux":
        # libname = "lib{}.so"
        libname = os.path.join(os.path.dirname(__file__), "lib{}.so")
    elif sys.platform == "darwin":
        # libname = "lib{}.dylib"
        libname = os.path.join(os.path.dirname(__file__), "lib{}.dylib")
    elif sys.platform.startswith("win"):
        # As of python3.8, ctypes does not seem to search $PATH when loading DLLs.
        # We could use `os.add_dll_directory` to configure the search path, but
        # it doesn't feel right to mess with application-wide settings. Let's
        # assume that the `.dll` is next to the `.py` file and load by full path.
        libname = os.path.join(
            os.path.dirname(__file__),
            "{}.dll",
        )
    return getattr(ctypes.cdll, libname.format("bdkffi"))
```
