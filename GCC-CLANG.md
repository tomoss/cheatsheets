# GCC / G++ & Clang / Clang++ Cheat Sheet

### 🔳 Common "One-Liner" for Development

```sh
g++ -std=c++20 -Wall -Wextra -Werror -g main.cpp -o myapp
clang++ -std=c++20 -Wall -Wextra -Werror -g main.cpp -o myapp
```

### 🔳 Compile a single C++ file

```sh
# Compiles and names the output executable myapp.
g++ main.cpp -o myapp
clang++ main.cpp -o myapp
```

### 🔳 Build stages

```sh
# 1) Preprocess only (expand includes/macros)
g++ -E main.cpp -o main.i

# 2) Compile to assembly
g++ -S main.cpp -o main.s

# 3) Assemble to object file
g++ -c main.cpp -o main.o

# 4) Link objects into executable
g++ main.o -o myapp
```

> All four stages work identically with `clang++`.

### 🔳 Setting C++ standards

```sh
g++ -std=c++11 main.cpp
g++ -std=c++14 main.cpp
g++ -std=c++17 main.cpp
g++ -std=c++20 main.cpp
g++ -std=c++23 main.cpp

# GNU extensions
g++ -std=gnu++20 main.cpp
```

> All `-std=c++XX` flags work identically with `clang++`.

### 🔳 Warning & Error Handling

<table border="1" cellpadding="6" cellspacing="0">
  <tr>
    <td><b>Flag</b></td>
    <td><b>Compiler</b></td>
    <td><b>Purpose</b></td>
  </tr>
  <tr>
    <td><code>-Wall</code></td>
    <td>Both</td>
    <td>Common warnings</td>
  </tr>
  <tr>
    <td><code>-Wextra</code></td>
    <td>Both</td>
    <td>Additional warnings</td>
  </tr>
  <tr>
    <td><code>-Werror</code></td>
    <td>Both</td>
    <td>Treat warnings as errors</td>
  </tr>
    <tr>
    <td><code>-Wpedantic</code></td>
    <td>Both</td>
    <td>Strict ISO C++ compliance</td>
  </tr>
  <tr>
    <td><code>-Wconversion</code></td>
    <td>Both</td>
    <td>Warns on potentially lossy conversions</td>
  </tr>
  <tr>
    <td><code>-Wsign-conversion</code></td>
    <td>Both</td>
    <td>Signed/Unsigned conversion warnings</td>
  </tr>
  <tr>
    <td><code>-Wshadow</code></td>
    <td>Both</td>
    <td>Variable name shadowing</td>
  </tr>
  <tr>
    <td><code>-Wnull-dereference</code></td>
    <td>Both</td>
    <td>Warns on possible null dereference</td>
  </tr>
  <tr>
    <td><code>-fdiagnostics-color=always</code></td>
    <td>GCC</td>
    <td>Forces colored output</td>
  </tr>
  <tr>
    <td><code>-fcolor-diagnostics</code></td>
    <td>Clang</td>
    <td>Forces colored output</td>
  </tr>
  </tr>
    <tr>
    <td><code>-Weverything</code></td>
    <td>Clang</td>
    <td>Enables every diagnostic</td>
  </tr>
    </tr>
  <tr>
    <td><code>-Wformat=2</code></td>
    <td>Both</td>
    <td>Strict type checking for <code>printf</code>/<code>scanf</code> family.</td>
  </tr>
    </tr>
  <tr>
    <td><code>-Wduplicated-cond</code></td>
    <td>GCC</td>
    <td>Catches duplicate else if conditions</td>
  </tr>
  <tr>
    <td><code>-Wduplicated-branches</code></td>
    <td>GCC</td>
    <td>Warns when both branches of an if/else contain identical code</td>
  </tr> 
</table>

### 🔳 Debugging

* `-g` → Adds debugging symbols. Required if you want to use tools like `gdb` or `lldb`.

* `-Og` → Optimize for debugging in GCC — roughly between `-O0` and `-O1`. Enables some optimizations that don't interfere with the debugger, giving faster debug builds without losing step accuracy.

* `-O0 -g` → The recommended combo for debug builds in Clang. Unlike GCC, Clang's `-Og` is just an alias for `-O1` with no special debug-friendly tuning, so prefer `-O0 -g` explicitly.

### 🔳 Optimization

* `-O0` → No optimization (default). Fastest compile time, slowest execution.

* `-O1` / `-O2` → Balanced optimization. Balanced speed and size. `-O2` is the standard for release builds.

* `-O3` → Aggressive optimization (vectorization, inlining).

* `-Ofast` → Breaks strict standard compliance for maximum speed.

* `-march=native` → Tailors the binary to your specific CPU instructions. Not portable across different CPU families.

* `-flto` → Link Time Optimization. Allows the compiler to optimize across translation unit boundaries. Massive speed gains for multi-file projects.

### 🔳 Disable debugging assertions

```sh
g++ -DNDEBUG main.cpp -o myapp
clang++ -DNDEBUG main.cpp -o myapp
```

### 🔳 Verbose build

```sh
g++ -v main.cpp -o myapp
clang++ -v main.cpp -o myapp
```

### 🔳 Multi-File Compilation

```sh
# The quick way
g++ main.cpp functions.cpp -o myapp
```

```sh
# The professional way (Step-by-Step)
g++ -c main.cpp functions.cpp
g++ main.o functions.o -o myapp
```

> Works identically with `clang++`.

### 🔳 Inclusion and Linking

* `-I<dir>` → Adds a directory to the include path (where the compiler looks for `.h` files).
* `-L<dir>` → Adds a directory to the library search path.
* `-l<library>` → Links a specific library (e.g., `-lpthread` for threads).

In both `g++` and `clang++`, the syntax for static (`.a`) and shared (`.so`) libraries is identical. The linker prefers the shared (`.so`) version by default when both are available.

### 🔳 Standard System Libraries

If the library is already installed in your system folders (like `/usr/include` and `/usr/lib`), you only need the name.

```sh
# Link against the threading library (libpthread.so)
g++ main.cpp -lpthread -o myapp
clang++ main.cpp -lpthread -o myapp
```

### 🔳 Custom / Third-Party Libraries

If you have a library into a specific folder (e.g., `./libs`), you must provide the paths manually.

```sh
g++ main.cpp \
    -I ./include \
    -L ./libs \
    -lcustomlib \
    -o myapp
```

### 🔳 The "Runtime" Library Fix

When linking a custom library in a local folder (e.g., `./libs`), the compiler finds it, but the OS will not find it when you run the program. This flag fixes that.

```sh
# -L tells the compiler where the lib is.
# -Wl,-rpath tells the EXECUTABLE where the lib is at runtime.
g++ main.cpp -I./include -L./libs -lcustom -Wl,-rpath,'$ORIGIN/libs' -o myapp
clang++ main.cpp -I./include -L./libs -lcustom -Wl,-rpath,'$ORIGIN/libs' -o myapp
```

Alternative → `LD_LIBRARY_PATH` env var

Must be set in every shell/environment
```sh
export LD_LIBRARY_PATH="./libs:$LD_LIBRARY_PATH"
```

### 🔳 Define macros

```sh
g++ -DDEBUG -DVERSION=3 main.cpp -o myapp
clang++ -DDEBUG -DVERSION=3 main.cpp -o myapp
```

### 🔳 Sanitizers

Sanitizers instrument the program by adding extra checks, so errors can be detected while the program runs.

```sh
# 1. Address + Undefined (Finds leaks and logic errors)
g++ -fsanitize=address,undefined -g main.cpp -o myapp
clang++ -fsanitize=address,undefined -g main.cpp -o myapp

# 2. Memory Sanitizer (Clang Only: Finds uninitialized memory reads)
clang++ -fsanitize=memory -fno-omit-frame-pointer -g main.cpp -o myapp

# 3. Thread Sanitizer (Finds data races in multi-threaded code)
# Note: Cannot be used with ASan simultaneously.
g++ -fsanitize=thread -g main.cpp -o myapp
clang++ -fsanitize=thread -g main.cpp -o myapp
```

### 🔳 Specialized Tools (Clang Ecosystem)

If you are using Clang, you should leverage the LLVM ecosystem:

* `-fuse-ld=lld` → Use the LLVM linker. It is significantly faster than the default GNU ld.

* `scan-build clang++ ...` → Runs the Clang Static Analyzer to find logic bugs without running the code.

* `-emit-llvm` → Outputs LLVM Intermediate Representation (`.ll` or `.bc`) for analysis.

### 🔳 Static analyzer

#### `-fanalyzer` is GCC's built-in static analyzer

```sh
g++ -fanalyzer main.cpp -o myapp
```

#### `scan-build` is Clang tool that runs Clang's static analyzer

```sh
scan-build clang++ -std=c++20 -c main.cpp
```

### 🔳 The Ultimate Debug Command

**For GCC:**

```sh
g++ -std=c++20 -Og -g \
    -Wall -Wextra -Wpedantic -Wshadow -Wconversion -Wformat=2 \
    -Wnull-dereference -Wduplicated-cond -Wduplicated-branches \
    -fsanitize=address,undefined \
    -fno-omit-frame-pointer -fstack-protector-strong \
    main.cpp -o myapp
```

**For Clang:**

```sh
clang++ -std=c++20 -O0 -g \
    -Wall -Wextra -Wpedantic -Wshadow -Wconversion \
    -fsanitize=address,undefined \
    -fno-omit-frame-pointer -fstack-protector-strong \
    main.cpp -o myapp
```

`-fno-omit-frame-pointer` → Makes the *Stack Trace* much more accurate when ASan triggers.

`-fstack-protector-strong` → Adds guard values to the stack to detect overflows before a function returns.
