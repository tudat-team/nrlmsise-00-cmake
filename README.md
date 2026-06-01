# nrlmsise-00

This repository has been copied directly from https://www.brodo.de/space/nrlmsise/ for use in the TU Delft Astrodynamics Toolbox (Tudat) ecosystem. The copyright of the code belongs to Dominik Brodowski.

* For more details on Tudat, we refer to the [project website](https://docs.tudat.space/en/latest/) and our [project Github main page](https://github.com/tudat-team)
* Conda package for this repository are available on [anaconda](anaconda.org/tudat-team/nrlmsise-00/), which is built through the [tudat-feedstock](https://github.com/tudat-team/nrlmsise-00-feedstock) on [Azure](https://dev.azure.com/tudat-team/feedstock-builds/_build?definitionId=7)

## Build, test and install

To build and test this code locally:

```bash
cmake -S . -B build
cmake --build build
cmake --build build --target test
```

This creates both the `libnrlmsise_00.so` shared library and the `nrlmsise_00_test` executable.

To install (for example in `~/.local`), run

```bash
cmake -S . -B build -DCMAKE_INSTALL_PREFIX=${HOME}/.local
cmake --build build --target install
```

## Using in another CMake project

### Option 1: `find_package` (installed library)

After installing the library, use `find_package` in your project:

```cmake
find_package(nrlmsise_00 REQUIRED)
target_link_libraries(your_target PRIVATE nrlmsise_00::nrlmsise_00)
```

### Option 2: `FetchContent` (source dependency)

Pull the library directly into your build:

```cmake
include(FetchContent)
FetchContent_Declare(
    nrlmsise_00
    GIT_REPOSITORY https://github.com/tudat-team/nrlmsise-00-cmake
    GIT_TAG        master
)
FetchContent_MakeAvailable(nrlmsise_00)

target_link_libraries(your_target PRIVATE nrlmsise_00::nrlmsise_00)
```

## License

FIXME. There is currently no proper license attached to this work.
