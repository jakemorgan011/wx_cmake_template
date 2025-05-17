
```cmake
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

project(wx_cmake_fetchcontent_template LANGUAGES C CXX OBJCXX)

include(FetchContent)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(wxBUILD_SHARED OFF)

message(STATUS "Fetching wxWidgets...")

FetchContent_Declare(
   wxWidgets
   GIT_REPOSITORY https://github.com/wxWidgets/wxWidgets.git
   GIT_SHALLOW ON
)

message(STATUS "Fetching JUCE...")

FetchContent_Declare(
   juce
   GIT_REPOSITORY https://github.com/juce-framework/JUCE.git
   GIT_SHALLOW ON
)

FetchContent_MakeAvailable(wxWidgets juce)

set(SRCS main.cpp)

add_executable(main WIN32 ${SRCS})
target_link_libraries(main PRIVATE wxcore wxnet)
```

### System Requirements

To get started with this project, ensure you have the following tools and libraries:

- **CMake (Version 3.14 or later):** Essential for building and managing the project files.
- **C++ Compiler:** Compatible with Clang, GCC, or MSVC. Choose one based on your development environment.
- **GTK3 Development Libraries (for Linux users):** Necessary for GUI development on Linux platforms.

### Building the Project

#### Debug

Follow these steps to build the project:

1. **Create a build directory & configure the build:**
   ```bash
   cmake -S. -Bbuild
   ```

2. **Build the project:**
   ```bash
   cmake --build build -j
   ```

This will create a `build` directory and compile all necessary artifacts there. The main executable will be located in `build/`.

#### Release

For release build use `--config Release` on Windows:

```bash
cmake -S. -Bbuild
cmake --build build -j --config Release
```

Artifacts for both configurations will be generated in the `build` directory.

On Mac or Linux you'll need to maintain two build trees:

```bash
cmake -S. -Bbuild -DCMAKE_BUILD_TYPE=Debug
cmake --build build -j
cmake -S. -Bbuild-rel -DCMAKE_BUILD_TYPE=Release
cmake --build build-rel -j
```

### License

MIT License. Can be used in closed-source commercial products.

---
Check out the blog for more! [www.lukesdevtutorials.com](https://www.lukesdevtutorials.com)
---
