# Housekeeping

# Development Environment

My C++ development environment runs on Ubuntu Linux, using:

- **Visual Studio Code** for code editing
- **Clang/LLVM** for native C++ compilation
- **CMake** for project configuration and build generation
- **Ninja** for build execution
- **GDB** for debugging native applications
- **Emscripten** for compiling C++ into WebAssembly

## Install Development Tools

```bash
sudo apt update
sudo apt install build-essential clang cmake ninja-build gdb
```

## Install Emscripten

Install the official Emscripten SDK:

```bash
git clone https://github.com/emscripten-core/emsdk.git
cd emsdk

./emsdk install latest
./emsdk activate latest
source ./emsdk_env.sh
```

The environment script must be sourced in each new terminal session unless the environment has been configured persistently.

## Verify Installation

```bash
clang++ --version
cmake --version
ninja --version
gdb --version
em++ --version
```

## Compile C++

**Native Linux executable using Clang:**

```bash
clang++ -std=c++20 main.cpp -o main
./main
```

**WebAssembly using Emscripten:**

```bash
em++ -std=c++20 main.cpp -o main.js
```

## Build with CMake and Ninja

**Native Linux using Clang:**

```bash
cmake -S . -B build-native -G Ninja -DCMAKE_CXX_COMPILER=clang++
cmake --build build-native
```

**WebAssembly using Emscripten:**

```bash
emcmake cmake -S . -B build-wasm -G Ninja
cmake --build build-wasm
```

Both configurations use CMake and Ninja, with separate compiler toolchains and build directories for native Linux and WebAssembly targets.

**Note:** Emscripten uses its own Clang/LLVM toolchain. It does not require GCC or Ubuntu's system Clang to generate WebAssembly.