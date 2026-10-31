
## Compiling C++

C++ applications can be compiled directly using a compiler or automatically using CMake and Ninja.

All generated files are stored in dedicated build directories excluded from Git by `.gitignore`.

### 1. Direct Compilation

Invoke the compiler directly. This approach is ideal for learning, experimenting and compiling simple programs.

#### Native Linux Using Clang

```bash
mkdir -p build-direct-native
clang++ -std=c++20 main.cpp -o build-direct-native/main
./build-direct-native/main
```

Clang compiles `main.cpp` into a native Linux executable named `main`, stored in `build-direct-native/`.

#### WebAssembly Using Emscripten

```bash
mkdir -p build-direct-wasm
em++ -std=c++20 main.cpp -o build-direct-wasm/main.js
```

Emscripten uses its own Clang/LLVM toolchain to compile the C++ source into WebAssembly.

This produces two files inside `build-direct-wasm/`:

- `main.wasm` contains the compiled WebAssembly module.
- `main.js` provides the JavaScript loader and runtime integration.

These files target a JavaScript environment and are not executed directly as native Linux applications.

### 2. Automated Compilation Using CMake and Ninja

CMake configures the project and generates build instructions. Ninja executes those instructions using the selected compiler.

This approach is recommended for larger projects, external dependencies and multiple compilation targets.

A `CMakeLists.txt` file is required in the project root.

#### Native Linux Using Clang

```bash
cmake -S . -B build-native -G Ninja -DCMAKE_CXX_COMPILER=clang++
cmake --build build-native
```

The first command:

- `-S .` specifies the current directory as the source directory.
- `-B build-native` stores generated build files in `build-native/`.
- `-G Ninja` selects Ninja as the build system.
- `-DCMAKE_CXX_COMPILER=clang++` selects Clang as the C++ compiler.

The second command executes the build using Ninja.

The resulting executable is stored in `build-native/`, with its name and location determined by `CMakeLists.txt`.

#### WebAssembly Using Emscripten

```bash
emcmake cmake -S . -B build-wasm -G Ninja
cmake --build build-wasm
```

The first command configures CMake to use Emscripten's WebAssembly toolchain and generates Ninja build instructions in `build-wasm/`.

The second command executes the build using Ninja.

Generated WebAssembly artifacts remain inside `build-wasm/`. Their filenames and runtime integration depend on the CMake configuration.

## What's the Difference?

### Direct Compilation

The compiler is invoked manually.

- Clang generates native Linux executables.
- Emscripten generates WebAssembly and supporting runtime files.
- Best suited to simple programs and experiments.

### Automated Compilation

CMake manages build configuration, while Ninja executes the generated build instructions.

- Supports multiple source files and dependencies.
- Rebuilds only what is necessary.
- Supports different compiler toolchains and target platforms.
- Best suited to larger projects and ongoing development.

Both approaches use Clang/LLVM for compilation in this environment. CMake and Ninja automate the build process rather than replace the compiler.

## Build Directory Organisation

All compiled output is kept separate from source code:

```text
cplusplus_examples/
├── .gitignore
├── CMakeLists.txt
├── main.cpp
├── build-direct-native/
├── build-direct-wasm/
├── build-native/
└── build-wasm/
```

The following `.gitignore` pattern excludes all four build directories:

```gitignore
/build-*/
```

**Note:** Use separate build directories for native and WebAssembly targets. CMake caches the selected compiler and toolchain, so switching targets should use a fresh build directory.
