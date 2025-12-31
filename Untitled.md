#### Ubuntu LTS

[](https://github.com/realsenseai/librealsense/tree/development/wrappers/python#ubuntu-lts)

1. Ensure apt-get is up to date

- `sudo apt-get update && sudo apt-get upgrade` **Note:** Use `sudo apt-get dist-upgrade`, instead of `sudo apt-get upgrade`, in case you have an older Ubuntu 14.04 version

2. Install Python and its development files via apt-get

- `sudo apt-get install python3 python3-dev`

3. Clone the librealsense repository and navigate into the directory:
    - `git clone https://github.com/realsenseai/librealsense.git`
    - `cd librealsense`
4. Configure and make:

- `mkdir build`
- `cd build`
- `cmake ../ -DBUILD_PYTHON_BINDINGS:bool=true -DPYTHON_EXECUTABLE=$(which python3)`
- `make -j4`
- `sudo make install`

> **Note**: For building a self-contained (statically compiled) pyrealsense2 library add the CMake flag:
> 
> `-DBUILD_SHARED_LIBS=false`
> 
> **Note**: To force compilation with a specific version on a system with a few Python versions installed, add the following flag to CMake command:
> 
> `-DPYTHON_EXECUTABLE=[full path to the exact python executable]`

5. update your PYTHONPATH environment variable to add the path to the pyrealsense library

- `export PYTHONPATH=$PYTHONPATH:/usr/local/lib`

> **Note:** If this doesn't work, try using the following path instead: `export PYTHONPATH=$PYTHONPATH:/usr/local/lib/[python version]/pyrealsense2`

6. Alternatively, copy the build output (`librealsense2.so` and `pyrealsense2.so`) next to your script. **Note:** Python 3 module filenames may contain additional information, e.g. `pyrealsense2.cpython-35m-arm-linux-gnueabihf.so`)