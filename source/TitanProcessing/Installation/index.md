# Building

## OS Support

Currently, Titan Processing only supports Linux/glibc builds for AArch64 and x86_64 with no plans to support Windows or MacOS, but feel free to try. All package requirements should be cross-platform. There are issues with configuring CMake using Visual Studio solutions and building with MSVC so consider using MinGW or Clang.

The conda packages are only built for Linux/glibc on AArch64 and x86_64. See [titan-forge-feedstocks](https://github.com/titan2022/titan-forge-feedstocks) for the package recipes. You can try to update the recipes to build on Windows or macOS.

## Instructions (new - Conda/Mamba)

### Installing the Conda environment
First, please have Conda or Mamba installed on your computer. If it's not installed, please install [Miniforge3](https://conda-forge.org/miniforge/), which includes Conda, Mamba and a conda-forge based Python environment. You can install Miniforge3 using the following command:

```bash
wget "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
bash Miniforge3-$(uname)-$(uname -m).sh
rm Miniforge3-$(uname)-$(uname -m).sh
```

Close and reopen your shell, and run:

```bash
# Prevent Conda from polluting your environment when you're not working on Conda-managed projects.
conda config --set auto_activate_base false
```

Now, you can use Conda to install the dependencies.

```bash
mamba env create -f environment.yml
mamba activate Titan-Processing
```

If you modify `environment.yml`, please run

```bash
mamba env update -f environment.yml
```

Occassionally, you should update all the packages to the latest versions:

```bash
mamba activate Titan-Processing
mamba update --all -c https://repo.prefix.dev/titan-forge
```

### Building the library and examples

```bash
mamba activate Titan-Processing

mkdir build && cd build
export CC=clang
export CXX=clang++
export CXXFLAGS="-pthread"

# Configure the build
cmake -B . -S .. -DCMAKE_EXPORT_COMPILE_COMMANDS=1 -DBUILD_EXAMPLES=1 -DCMAKE_BUILD_TYPE=RelWithDebInfo -GNinja

# Actually run the build
ninja
```

## Instructions (old - Debian packages)

1. Install OpenCV 4.8.x or above with the Aruco contrib module. Python 3.8.x or above support is optional but necessary to run calibration scripts. Follow the official guide to build OpenCV contrib [here](https://github.com/opencv/opencv_contrib), but only the ArUco submodule is used.
2. (Optional) Install [Intel RealSense SDK](https://github.com/IntelRealSense/librealsense) to build RealSense modules.
3. Install [GoogleTest testing framework](https://github.com/google/googletest/blob/main/googletest/README.md).
4. Install [JSON for Modern C++](https://github.com/nlohmann/json) using [vcpkg](https://github.com/Microsoft/vcpkg) or build it from scratch.
To install vcpkg with our own Debian binaries, execute the following.

```bash
# Install vcpkg binary (optional)
curl -OL https://github.com/ethanc8/titanian-repo/raw/master/debian/pool/main/v/vcpkg/vcpkg_2023.12.10-2+titan2022_amd64.deb
sudo apt install ./vcpkg_2023.12.10-2+titan2022_amd64.deb

# Install nlohmann json library using vcpkg
vcpkg install nlohmann-json
```

3. Configure the CMake script using Makefile or Ninja (both build systems supported) using the following commands. Link vcpkg toolchain file to include vcpkg packages or alternatively link `nlohmann-json` manually.

```bash
mkdir build && cd build

# Config to build examples
cmake -B . -S .. -DCMAKE_TOOLCHAIN_FILE=[PATH TO VCPKG]/scripts/buildsystems/vcpkg.cmake -DBUILD_EXAMPLES=1

# Full debug config
# - forces CMake to use Ninja
# - allows for Clangd usage
cmake -B . -S .. -DCMAKE_TOOLCHAIN_FILE=[PATH TO VCPKG]/scripts/buildsystems/vcpkg.cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1 -DBUILD_EXAMPLES=1 -DCMAKE_BUILD_TYPE=RelWithDebInfo -GNinja

# Production config
cmake -B . -S .. -DCMAKE_TOOLCHAIN_FILE=[PATH TO VCPKG]/scripts/buildsystems/vcpkg.cmake -DBUILD_EXAMPLES=1
```

5. Build and install the project using your build system.

```bash
# For Makefile
make -j
sudo make install

# For Ninja
ninja
sudo ninja install
```

### Build Options

* `-DBUILD_EXAMPLES`: builds example files (found in `example` directory)
* `-DNO_REALSENSE`: builds without RealSense code for compatibility
