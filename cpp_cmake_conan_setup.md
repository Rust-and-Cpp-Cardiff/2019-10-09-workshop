# C++ CMake Project setup (with integrated Conan package manager)

## Why are we doing it?

The configuration steps below assume you've followed [C++17 LLVM/Clang setup](./cpp_setup.md) first. At this stage you have C++ compiler (based on LLVM) and you've seen how to compile and link one file.

But... in more advanced project you compile more files, link objects and libraries, run some external programs. You need build tool that consumes your project specification and runs all necessary commands for you.

But... historically, there were different C++ build tools per each OS platform (e.g. make, nmake, build, etc. it changed with e.g. jom, ninja). So the projects were not really cross-compile (although the language was... apart from compiler flags ;) ... and conformance ;) ). Hence, CMake - the build generation tool. From CMakeLists.txt specification file, cmake generates build projects for a variety of build tools (alternatives [VCPKG](https://github.com/microsoft/vcpkg), [Hunter(for CMake)](https://github.com/ruslo/hunter), [Buckaroo](https://buckaroo.pm/),

But... there is no standard way of handling dependencies in C++. You might install 3rd party programs and libraries with system dependency manager (apt, yum, pacman, choco, brew etc.) but that affects the whole system and version management is difficult. You need per-project management of 3rd party tools. Again, do it manually, but it's daunting. So we need package manager. Probably with pre-built packages (building automatically if needed), with cache shared for many projects, version aware - our choice in this starter project is [conan](https://conan.io/), with [integration to cmake](https://github.com/conan-io/cmake-conan).

## What are we doing here?

So, additionally to clang/LLVM we need to install:
* CMake to generate build
* build tool
* conan (which is written in python -> python dependency; make it python3!)

1. [Windows](#1-windows)
2. [Linux (Ubuntu, Manjaro)](#2-linux-ubuntu-manjaro)
3. [MacOS](#3-macos)

## 1. Windows


## 2. Linux (Ubuntu, Manjaro)
## 3. MacOS
### 3.1 Tools installation

3.1.1 Install cmake

- In terminal run following command

```shell
brew install cmake
```
3.1.2 Install build tool

There is no need to install the build-tool, `make` is already installed by default. You might want to install jom or ninja (but it's out of scope).

3.1.3 Install conan:

- In terminal run commands:

```shell
brew install python
pip3 install --user conan
```

- Restart the termianl. Try running `conan --version` to see if it's added to the PATH. If not, run the following in the terminal (one off action):

```shell
#we installed conan with '--user' option; it might not be available on path
echo 'export PATH=${HOME}/Library/Python/3.7/bin/:$PATH' >> ~/.profile
. ~/.profile
```

