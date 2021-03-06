# Build minimal PyTorch using setup.py
This CMakeLists.txt manages the building of a minimal PyTorch library from [PyTorch 1.7.0](https://github.com/pytorch/pytorch/tree/v1.7.0) by setting environment variables and calling [pytorch/setup.py](https://github.com/pytorch/pytorch/blob/v1.7.0/setup.py)
## Prerequisites
1. Clone [PyTorch 1.7.0](https://github.com/pytorch/pytorch/tree/1.7.0) and adjust the [CMakeLists.txt](CMakeLists.txt) variable `PYTORCH_SRC_DIR` to point to the local repository, for example `set(PYTORCH_SRC_DIR ../pytorch)`
2. Install the [PyTorch prerequisites](https://github.com/pytorch/pytorch/tree/1.7.0#from-source)
## Equivalent bash script [inspired by C++ development tips](https://github.com/pytorch/pytorch/blob/v1.7.0/CONTRIBUTING.md#c-development-tips)
    cd pytorch
    export DEBUG=1
    export BUILD_TEST=0
    export USE_CUDA=0
    export USE_DISTRIBUTED=0
    export USE_MKLDNN=0
    export USE_FBGEMM=0
    export USE_NNPACK=0
    export USE_QNNPACK=0
    export USE_XNNPACK=0
    export BUILD_CAFFE2_OPS=0
    [see below for minor edits to PyTorch tree]
    python setup.py build --cmake-only
    cmake --build build
## Usage
### Generate the project buildsystem
    cmake -S . -B build
#### [CMakeLists.txt](CMakeLists.txt) adjusts the following in `${PYTORCH_SRC_DIR}/CMakeLists.txt`
##### GCC
`-Og` enables optimizations that do not interfere with debugging. This replaces `-O2` in `CMAKE_CXX_FLAGS`
#### [CMakeLists.txt](CMakeLists.txt) adjusts the following in `${PYTORCH_SRC_DIR}/cmake/Dependencies.cmake`
#### Remove deprecated `find_package(PythonLibs 3.0)`
This avoids the following error -

    CMake Error at cmake/Dependencies.cmake:926 (if):
      if given arguments:

        "VERSION_LESS" "3"

      Unknown arguments specified
### Optionally adjust `${PYTORCH_SRC_DIR}/build/CMakeCache.txt`
#### CMAKE_BUILD_TYPE 
The environmental variable `DEBUG=1`, sets `CMAKE_BUILD_TYPE=Debug` to build libraries with debug symbols
#### BUILD_SHARED_LIBS
The build generates shared libraries by default. This can be disabled by setting `BUILD_SHARED_LIBS=OFF`
### Build the PyTorch project
    cmake --build ${PYTORCH_SRC_DIR}/build
### Example
    cmake ---S . -B build
    cmake --build ../pytorch/build
### Location of built libraries
    pytorch/build/lib
### Cleaning / trouble-shooting
#### Linux
    rm build/CMakeCache.txt
    rm -rf build
#### Windows
    del build/CMakeCache.txt
    rmdir /s /q build
    mklink /d build d:\build
#### Clean PyTorch source without using standard ignore rules
    git clean -dfx
