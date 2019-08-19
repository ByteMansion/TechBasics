# CMake Introduction
cmake - Cross-Platform Makefile Generator.

The `cmake` executable is the CMake command-line interface. It may be used to configure projects in scripts. Project configuration settings may be specified on the command line with the `-D` option. The `-i` option will cause cmake to ineractively prompt for such settings.

CMake is a cross-platform build system generator. Projects specify their build process with platform-independent CMake listfiles included in each directory of a source tree with the name `CMakeLists.txt`. Users build a project by using CMake to generate a build system for a native tool on their platform.

## Organization
CMake input files are wroten in the "CMake Language" in source files named `CMakeLists.txt` or ending in a `.cmake` file name extension.

CMake Language source files in a project are organized into:
- Directories(`CMakeLists.txt`)
  When CMake processes a project source tree, the entry point is a source file called `CMakeLists.txt` in the top-level source directory. This file may contain the entire build specification or use the `add_subdirectory()` command to add subdirectories to the build. Each subdirectory added by the command **must** also contain a `CMakeLists.txt` file as the entry point to that directory. For each source directory whose `CMakeLists.txt` file is processed, CMake generates a corresponding directory in the build tree to act as the default working and output directory.
- Scripts(`<script>.cmake`)
  An individual `<script>.cmake` source file may be processed in *script mode* by using the `cmake` command-line tool with the `-P` option. Script mode simply runs in the given CMake Language source file and **does not** generate a build system. It **does not** allow CMake commands that define build targets or actions.
- Modules(`<module>.cmake`)
  CMake Language code is either **Directories** or **Script** may use the `include()` command to load a `<module>.cmake` source file in the scope of the including context. See the `cmake-modules` manual page for documentation of modules included with the CMake distribution. Project source trees may also provide their own modules and specify their location(s) in the `CMAKE_MODULE_PATH` variable.

In this manual, I only document some key points of `Directories`.

## Syntax


