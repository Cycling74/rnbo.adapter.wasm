# RNBO Wasm Adapter

## Debugging Build

We use cmake to build, emsdk comes with a cmake toolchain.. mine is at `/Users/xnor/local/src/emsdk//upstream/emscripten/cmake/Modules/Platform/Emscripten.cmake` so I can build with cmake via:

```
mkdir build && cd build && CMAKE_TOOLCHAIN_FILE=/Users/xnor/local/src/emsdk//upstream/emscripten/cmake/Modules/Platform/Emscripten.cmake cmake ..
```

For installing in the test directory to debug in chrome

```
mkdir build && cd build
CMAKE_TOOLCHAIN_FILE=/Users/xnor/local/src/emsdk//upstream/emscripten/cmake/Modules/Platform/Emscripten.cmake \
  cmake .. -DCMAKE_INSTALL_PREFIX=~/dev/rnbo.core/src/js/test/static/ \
  -DCMAKE_BUILD_TYPE=Debug \
  -DRNBO_CLASS_FILE=/Users/xnor/Documents/export/cycle-mod/rnbo_source.cpp \
  -DRNBO_DESCRIPTION_FILE=/Users/xnor/Documents/export/cycle-mod/description.json
cmake --build . && cmake --install .
```
