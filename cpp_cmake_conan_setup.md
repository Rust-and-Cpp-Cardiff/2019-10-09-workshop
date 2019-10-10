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

### 1.1 Tools Installation

1.1.1 Install cmake

- Open PowerShell with Administrator priviledges and run command below. This will install CMake to `C:\Program Files\CMake\`.

```ps
choco install cmake
```

1.1.2 Build tool

- There's no need to install any build tool. nmake is delivered with chocolatey visualstudio2019-workload-nativedesktop workload we installed earlier. We could install jom or ninja, which is out of scope of this tutorial.

1.1.3 Install conan:

- Open PowerShell with Administrator priviledges and run command below. This will install Pytho to e.g. `C:\Python37`:

```ps
choco install python
```

- Open PowerShell as normal user:

```ps
#might need to add python path: `$env:Path += ';C:\Python37\'`
pip3 install conan
```

1.1.4 Install git:

- Open PowerShell with Administrator priviledges and run:

```ps
choco install git
```

1.1.4 Build cmake starter project:

- Open PowerShell as normal user, clone the repository:
```ps
git clone https://github.com/Rust-and-Cpp-Cardiff/2019-10-09-workshop.git
cd .\2019-10-09-workshop\cpp-starter-project

```

- Create a build directory (note, it's worth having out-of-source build):

```
mkdir build
cd build
```

- Run CMake to configure the project (create Makefiles for nmake). We need to add CMake bin directory to the path, as it wasn't added automatically

TODO: standalone clang doesn't play well with cmake, need to figure out more

```ps
$env:Path += ';C:\Program Files\CMake\bin\'
cmake `
  -DCMAKE_C_COMPILER="clang-cl" -DCMAKE_CXX_COMPILER="clang-cl" `
  -DCMAKE_BUILD_TYPE=Debug `
  -G "Visual Studio 16 2019" `
  --configure ..
```

- Run CMake to build using appropriate build tool:

```ps
cmake --build .
```

## 2. Linux (Ubuntu, Manjaro)


2.1.1 Install cmake

- `Ubuntu`: Kitware provides Ubuntu [repository for cmake](https://apt.kitware.com). Following these commands:

```shell

wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | sudo apt-key add -
#Beaver 18.04, for Ubuntu Xenial 16.04 see the link above
sudo apt-add-repository 'deb https://apt.kitware.com/ubuntu/ bionic main'
sudo apt-get update
sudo apt-get install kitware-archive-keyring
sudo apt-key --keyring /etc/apt/trusted.gpg del C1F34CDD40CD72DA
sudo apt-get install cmake
```

- `Manjaro`: We download prebuilt binary

```shell
    export CMAKE_VERSION=3.15.4
    wget -qO- https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}-Linux-x86_64.tar.gz | \
        sudo tar -xvzf - -C /opt/
    echo "export PATH=\${PATH}:/opt/cmake-${CMAKE_VERSION}-Linux-x86_64/bin" >> ~/.bash_profile 
    . ~/.bash_profile
```

2.1.2 Install build tool

- We need to install `make` build tool.

`Ubuntu`:

```shell
sudo apt-get install make
```

`Manjaro`:

```shell
sudo pacman -S make
```

2.1.3 Install conan:

- In terminal run commands:

`Ubuntu`:

```shell
sudo apt-get install -y python3 python3-pip && \
pip3 install --user conan && \
```

- Restart the termianl. Try running `conan --version` to see if it's added to the PATH. If not, run the following in the terminal (one off action):

```shell
echo "export PATH=~/.local/bin:\$PATH" >> ~/.profile
~/.profile
```

`Manjaro`:

```shell
    sudo pacman -S python python-pip && \
    pip3 install conan
```


2.1.4 Build cmake starter project:

- In terminal

```shell
git clone https://github.com/Rust-and-Cpp-Cardiff/2019-10-09-workshop.git
cd ./2019-10-09-workshop/cpp-starter-project

```

- Create a build directory

```shell
mkdir build && cd build
```

- Run CMake to configure the project (create Makefiles for make).

```shell
CC=/usr/bin/clang \
CXX=/usr/bin/clang++ \
    cmake --configure -DCMAKE_BUILD_TYPE=Debug ..
```

- Run CMake to build using appropriate build tool:

```shell
cmake --build .
```

- Run resulting binaries:
```shell
# production binary
./bin/cpp-starter
# tests binary
./bin/cpp-starter-tests
```

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
find ~/Library/Python -name "conan" \
  -exec echo -n "export PATH=\${PATH}:" \; \
  -exec dirname "{}" \; 
```

- If you're happy with the result of the above action run this:

```shell
find ~/Library/Python -name "conan" \
  -exec echo -n "export PATH=\${PATH}:" \; \ 
  -exec dirname "{}" \; > ~/.profile

. ~/.profile
```

3.1.4 Build cmake starter project:

- In terminal

```shell
git clone https://github.com/Rust-and-Cpp-Cardiff/2019-10-09-workshop.git
cd ./2019-10-09-workshop/cpp-starter-project

```

- Create a build directory

```shell
mkdir build && cd build
```

- Run CMake to configure the project (create Makefiles for make).

```shell
export SDKROOT=$(xcrun --show-sdk-path)
cmake \
  -DCMAKE_BUILD_TYPE=Debug \
  --configure ..
```

- Run CMake to build using appropriate build tool:

```shell
cmake --build .
```

- Run resulting binaries:
```shell
# production binary
./bin/cpp-starter
# tests binary
./bin/cpp-starter-tests
```

# 4. MacOS CMake/Conan project description:

CMake project configuration is kept in files called CMakeLists.txt
The root CMakeLists.txt is responsible for:
* downloading conan.cmake if it doesn't exist - that's the conan plugin into cmake; after including it we have access to `conan_*` functions
* `conan_cmake_run(...)` function runs `conan` command line tool, passing it parameters being parameters to this function:
** REQUIRES followed by the list of packages we want to be managed by conan. If for instance we wanted to add another package, e.g. [cpprestsdk](https://bintray.com/bincrafters/public-conan/cpprestsdk%3Abincrafters), we'd have as following:
```conan_cmake_run(REQUIRES 
                  gtest/1.8.1@bincrafters/stable
                  cpprestsdk/2.10.14@bincrafters/stable
                BASIC_SETUP 
                CMAKE_TARGETS
                BUILD missing)
```
** CMAKE_TARGETS tells conan plugin to export built packages in form of cmake targets
** BUILD missing says to build packages for whcih binaries are not found in remotes
* `add_subdirectory` adds another subdirectory in which CMakeLists.txt should be defined
* `add_executable(cpp-starter-tests main.cpp)` defines CMake binary target `cpp-starter-tests` to be built from main.cpp file(s)
* `target_link_libraries(cpp-starter-tests CONAN_PKG::gtest)` says binary target `cpp-starter-tests` depends/links to the library CONAN_PKG:gtest provided by conan


