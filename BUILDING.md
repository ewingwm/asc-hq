CMake build notes for asc-hq

This repository historically used Autotools. A CMake build scaffold is provided to generate Makefiles.

Quick start (recommended):

1. Install dependencies (example on Debian/Ubuntu):

   sudo apt install build-essential cmake libsdl2-dev libboost-all-dev libfreetype6-dev libsigc++-2.0-dev

2. Configure and build:

   cmake -S . -B build-cmake
   cmake --build build-cmake --target asc -j$(nproc)

Notes and caveats:

- The CMakeLists provided are a scaffold to replace autotools. Not all optional libraries are auto-detected; if find_package fails, set variables like BOOST_ROOT or install the required -dev packages.
- Tools in `source/tools` are created as individual executables from each .cpp file.
- Some sub-libraries under `source/libs` may not yet have CMakeLists; those will be skipped for now.

If you want full feature parity with the autotools build, install the same development packages that were used historically (SDL1/SDL2, Boost, Loki headers shipped in source/libs/loki-0.1.6).
