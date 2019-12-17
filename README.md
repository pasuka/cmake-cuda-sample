# CMake for CUDA samples

## Purpose

As official CUDA samples only support Visual Studio for Windows OS, this is a in-house project which try to build these samples with CMake on Windows OS.

## Usage

1. Install Visual Studio(at least 2019), NV's CUDA samples(10.1+) and CMake tool(3.14+), i.e. anaconda, MSYS2.
2. Modify the CMakeLists.txt to set correct root of CUDA samples and libraries.
3. Run command
   1. `mkdir build`
   2. `cd build`
   3. `cmake ..`
   4. `cmake --build .`

## TODO

Currently only limited cases were added.