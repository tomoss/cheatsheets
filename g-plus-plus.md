# GCC / G++ Cheat Sheet

### 🔳 Common "One-Liner" for Development

```sh
g++ -std=c++20 -Wall -Wextra -Werror -g main.cpp -o myapp
```

### 🔳 Compile a single C++ file

```sh
# Compiles and names the output executable myapp.
g++ main.cpp -o myapp
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

### 🔳 Setting C++ standards

```sh
g++ -std=c++11 main.cpp
g++ -std=c++14 main.cpp
g++ -std=c++17 main.cpp
g++ -std=c++20 main.cpp
g++ -std=c++23 main.cpp

# GNU extensions (common on Linux)
g++ -std=gnu++20 main.cpp
```

### 🔳 Warning & Error Handling

* `-Wall` → common warnings

* `-Wextra` → additional warnings

* `-Werror` → treat warnings as errors

* `-Wpedantic` → warn on non-ISO extensions

* `-Wconversion` → warns on potentially lossy conversions

* `-Wsign-conversion` → signed/unsigned conversion warnings

* `-Wshadow` → variable name shadowing

* `-Wnull-dereference` → warns on possible null deref (GCC-specific)

* `-fdiagnostics-color=always` → colored diagnostics even when piped (GCC)

### 🔳 Debugging

* `-g` → Adds debugging symbols. Required if you want to use tools like `gdb` or `lldb`.

* `-Og` → Optimize for debugging — roughly between `-O0` and `-O1`. Enables some optimizations that don't interfere with the debugger, giving faster debug builds without losing step accuracy.

### 🔳 Optimization

* `-O0` → No optimization (default).

* `-O1` / `-O2` → Balanced optimization; `-O2` is the standard for release builds.

* `-O3` → Aggressive optimization.

* `-Ofast` → Breaks strict standard compliance for maximum speed.

* `-march=native` → Optimize for the CPU of the machine compiling the code (uses all available SIMD/instruction set extensions). Not portable across different CPU families.

* `-flto` → Link Time Optimization. Allows the compiler to optimize across translation unit boundaries. Significant performance gains on release builds.

### 🔳 Disable debugging assertions

```sh
g++ -DNDEBUG main.cpp -o myapp
```

### 🔳 Verbose build

```sh
g++ -v main.cpp -o myapp # verbose: shows compile + link steps
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

### 🔳 Inclusion and Linking

* `-I<dir>` → Adds a directory to the include path (where the compiler looks for `.h` files).

* `-L<dir>` → Adds a directory to the library search path.

* `-l<library>` → Links a specific library (e.g., `-lpthread` for threads).

In g++, the syntax for linking a static library (`.a`) and a shared library (`.so`) is exactly the same. When you have both `libfoo.a` and `libfoo.so` available, the linker prefers the shared  (`.so`) version by default.

### 🔳 Standard System Libraries

If the library is already installed in your system folders (like `/usr/include` and `/usr/lib`), you only need the name.

```sh
# Link against the math library (libm.so)
g++ main.cpp -lm -o myapp

# Link against the threading library (libpthread.so)
g++ main.cpp -lpthread -o myapp
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

### 🔳 Define macros

```sh
g++ -DDEBUG -DVERSION=3 main.cpp -o myapp
```

### 🔳 Sanitizers

#### Address + Undefined (most common combo)

```sh
g++ -fsanitize=address,undefined -g main.cpp -o myapp
```

#### ThreadSanitizer (data races)

```sh
g++ -fsanitize=thread -g main.cpp -o myapp
```

Tip: don’t mix TSan with ASan in the same build.

### 🔳 The Ultimate "Ironclad" Debug Command

```sh
g++ -std=c++20 -O0 -g \
    -Wall -Wextra -Wpedantic -Wshadow -Wconversion \
    -fsanitize=address,undefined \
    -fno-omit-frame-pointer \
    main.cpp -o myapp
```

Adding `-fno-omit-frame-pointer` makes the *Stack Trace* much more accurate when ASan triggers.

### 🔳 Modern Safety: The Stack Protector

* `-fstack-protector-strong` → Adds guard values to the stack to detect overflows before a function returns.





