# Ifc.js on Windows with WSL2

I've used Windows/WSL2 as development environments for Ifc.js

For me, the easiest way to install and configure the environment on windows is to take advantage of WSL2.
I've got a WSL2 Ubuntu installation available.

## Setup environment

Emscripten in installed using the git download script

1. Install python (using apt)
2. Install cmake (using apt)
3. Install emsdk - following web instructions that start cloning a git repository.
4. Select a suitable folder for installation, for me it is /home/bonghi/bin/emsdk

## Building ifc.js

### using cmake

| Command or file              | Notes                                                      |
| ---------------------------- | ---------------------------------------------------------- |
| cmake -S wasm/ -B wasm-build | Creates the configuration file                             |
| CMakeCache.txt               | This file contains the configuration choices for the build |
| Cmake --build &lt;folder&gt; | Runs the compilation of the project                        |
| Cmake -G                     | Chose the generator (ninja, Unix Makefiles, etc)           |

### using Emscripten (alternative to cmake)

| Command or file                   | Notes                                                                               |
| --------------------------------- | ----------------------------------------------------------------------------------- |
| emcmake cmake -S wasm/ -B em-wasm | Run in src folder. Prepares the configuration folder                                |
| edit CMakeCache.txt               | to configure the build parameters                                                   |
| emmake make                       | Run in em-wasm. When we use emmake to compile all the wasm and js files are created |

### using vs2022

I've made a few changes to the web-ifc repository so that replacing the msvc compiler does not throw errors.
Changes are mostly around the specification of warning settings that is incompatible between gcc and msvc.
