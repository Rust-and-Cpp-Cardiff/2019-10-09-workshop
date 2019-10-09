# C++17 LLVM/Clang Setup

Setup C++17 LLVM/Clang instructions for...

1. [Windows](##1.0.0-Windows)
2. [Linux (Ubuntu, Manjaro)](##2.0.0-Linux-(Ubuntu,-Manjaro))
3. [MacOS](##3.0.0-MacOS)
4. [Pi(armv7)](##4.0.0-Pi(armv7))

## 1. Windows

### 1.1. Toolchain setup

1.1.1. Download and install `Chocolatey` (https://chocolatey.org/install)

- **Why?**: Excellent Windows package manager that handles complex setups.
- Open `powershell` as _administrator_.
- Run command...

```ps
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

- Close and restart `powershell` as _administrator_.
  
1.1.2. Install the LLVM package (https://chocolatey.org/packages/llvm)

- Open `powershell` as _administrator_.

```ps
choco install llvm
```

1.1.3.(Optional) Update the installed LLVM package

- Open `powershell` as _administrator_.

```ps
choco upgrade llvm
```

1.1.3. Install Visual Studio Desktop development with C++ workload, which LLVM depends on:

- Open `powershell` as _administrator_.

```ps
choco install visualstudio2019community
choco install visualstudio2019-workload-nativedesktop
```

### 1.2. Project Setup

1.2.1. Create the project folder

- Open `powershell`

```ps
New-Item .\MyCppProject -ItemType directory 
New-Item .\MyCppProject\src -ItemType directory
New-Item .\MyCppProject\bin -ItemType directory
New-Item .\MyCppProject\src\main.cpp
```

1.2.2. Program `hello-world`

- Open `powershell`

```ps
notepad .\MyCppProject\src\main.cpp
```

- Inside `notepad` program our `hello-world` application

```c++
#include <iostream>

int main()
{
  std::cout << "hello world!" << std::endl;
  return 0;
}
```

- (Optional) I recommend you use better a text editor than `notepad`. [Notepad++](https://chocolatey.org/packages/notepadplusplus) or [Visual Studio Code](https://chocolatey.org/packages/vscode) are pretty code.

### 1.3. Build and Run

1.3.1. Build our C++ program using Clang

- Open `powershell`

```ps
clang++ .\MyCppProject\src\main.cpp -O2 -std=c++17 -o .\MyCppProject\bin\hello-world.exe
```

- `-O2` = Set the optimisation level to `2`.
- `-std=c++17` = Compile using the `C++17` standard.
- `-o` = The compiled export path.

1.3.2. Run your C++ program

- Open `powershell`

```ps
.\MyCppProject\bin\hello-world.exe
```

- The expected output should be

```ps
hello world!
```

## 2. Linux (Ubuntu, Manjaro)

2.1.1. Install the Clang+LLVM toolchain

- Open `terminal`

`Ubuntu`:
```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install clang
```

`Manjaro`:
```bash
sudo pacman -Syu
sudo pacman -S clang
```

### 2.2. Project Setup

2.2.1. Create the project folder

- Open `terminal`

```bash
mkdir ./MyCppProject
mkdir ./MyCppProject/src
mkdir ./MyCppProject/bin
touch ./MyCppProject/src/main.cpp
```

2.2.2. Program `hello-world`

- Open `terminal`

```bash
nano ./MyCppProject/src/main.cpp
```

- Inside `nano`

```c++
#include <iostream>

int main()
{
  std::cout << "hello world!" << std::endl;
  return 0;
}
```

- Don't forget to save with `ctrl+O`

### 2.3. Build and Run

2.3.1. Build our C++ program using Clang

- Open `terminal`

```bash
clang++ ./MyCppProject/src/main.cpp -O2 -std=c++17 -o ./MyCppProject/bin/hello-world
```

- `-O2` = Set the optimisation level to `2`.
- `-std=c++17` = Compile using the `C++17` standard.
- `-o` = The compiled export path.

2.3.2. Run your C++ program

- Open `terminal`

```bash
./MyCppProject/bin/hello-world
```

- The expected output should be

```bash
hello world!
```

## 3. MacOS

### 3.1. Toolchain setup

3.1.1. Download and install `Homebrew` (https://brew.sh/)

- **Why?**: Excellent MacOS package manager that handles complex setups.
- Open `terminal`
- Run command...

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
  
3.1.2. Install the LLVM package (https://chocolatey.org/packages/llvm)

- Open `terminal`

```bash
brew install llvm
```

3.1.3.(Optional) Update the installed LLVM package

- Open `terminal`

```bash
brew update llvm
```

### 3.2. Project Setup

3.2.1. Create the project folder

- Open `terminal`

```bash
mkdir ./MyCppProject
mkdir ./MyCppProject/src
mkdir ./MyCppProject/bin
touch ./MyCppProject/src/main.cpp
```

3.2.2. Program `hello-world`

- Open `terminal`

```bash
nano ./MyCppProject/src/main.cpp
```

- Inside `nano`

```c++
#include <iostream>

int main()
{
  std::cout << "hello world!" << std::endl;
  return 0;
}
```

- Don't forget to save with `ctrl+O`

### 3.3 Build and Run

3.3.1. Build our C++ program using Clang

- Open `terminal`

```bash
clang++ ./MyCppProject/src/main.cpp -O2 -std=c++17 -o ./MyCppProject/bin/hello-world
```

- `-O2` = Set the optimisation level to `2`.
- `-std=c++17` = Compile using the `C++17` standard.
- `-o` = The compiled export path.

3.3.2. Run your C++ program

- Open `terminal`

```bash
./MyCppProject/bin/hello-world
```

- The expected output should be

```bash
hello world!
```

## 4. Pi(armv7)

### 4.1. Toolchain setup

4.1.1 Install the Clang+LLVM toolchain for `armv7`

- Taken from: https://solarianprogrammer.com/2018/04/22/raspberry-pi-raspbian-install-clang-compile-cpp-17-programs/
- Open terminal

``` bash
cd ~
wget http://releases.llvm.org/9.0.0/clang+llvm-9.0.0-armv7a-linux-gnueabihf.tar.xz

tar -xvf clang+llvm-9.0.0-armv7a-linux-gnueabihf.tar.xz
rm clang+llvm-9.0.0-armv7a-linux-gnueabihf.tar.xz
mv clang+llvm-9.0.0-armv7a-linux-gnueabihf clang_9.0.0
sudo mv clang_9.0.0 /usr/local

export PATH=/usr/local/clang_9.0.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/clang_9.0.0/lib:$LD_LIBRARY_PATH
```

### 4.2. Project Setup

4.2.1. Create the project folder

- Open `terminal`

```bash
mkdir ./MyCppProject
mkdir ./MyCppProject/src
mkdir ./MyCppProject/bin
touch ./MyCppProject/src/main.cpp
```

4.2.2. Program `hello-world`

- Open `terminal`

```ps
nano ./MyCppProject/src/main.cpp
```

- Inside `nano`

```c++
#include <iostream>

int main()
{
  std::cout << "hello world!" << std::endl;
  return 0;
}
```

- Don't forget to save with `ctrl+O`

### 4.3. Build and Run

4.3.1. Build our C++ program using Clang

- Open `terminal`

```bash
clang++ ./MyCppProject/src/main.cpp -O2 -std=c++17 -o ./MyCppProject/bin/hello-world
```

- `-O2` = Set the optimisation level to `2`.
- `-std=c++17` = Compile using the `C++17` standard.
- `-o` = The compiled export path.

4.3.2. Run your C++ program

- Open `terminal`

```bash
./MyCppProject/bin/hello-world
```

- The expected output should be

```bash
hello world!
```
