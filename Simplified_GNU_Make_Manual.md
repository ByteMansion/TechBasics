# Makefile manual
Makefile is a program building tool which runs on Unix, linux, and their flavors. It aids in simplifying building the program executables that may need various modules. To determine how the modules need to be compiled or recompiled together, **make** takes the help of user-defined makefiles.
Makefile guides the **make** utility while compiling and linking program modules.

## Why Makefile?
Makefiles are special format files that help build and manage the projects automatically.

For example, we have following 4 source files:
- main.cpp
- hello.cpp
- factorial.cpp
- factorial.hpp

The trivial way to compile the files and obtain an executable is by running the following command:
```
CC main.cpp hello.cpp factorial.cpp -o hello
```
This command generates *hello* binary. There are only 4 files in the example, above command is simple and affordable. But if we have a large project containing thousands of files, it becomes difficult to maintain the binary builds.

The **make** command allows you to manage large programs or groups of programs. As you begin to write large programs, you notice that re-compiling large programs takes longer time than re-compiling short programs. Moreover, you notice that you usually only work on a small section of the pragram such as a single function, and much of the remaining program is unchanged.

## MACROS
The **make** program allows you to use macros, which are similar to variables. Macros are defined in the Makefile as `=` pairs.

### Spacial Macros
- `$@` is the name of the file to be made
- `$?` is the name of the changed dependents
- `$<` is the name of the related file that caused the action
- `$*` is the prefix shared by target and dependent files

For example:
```Makefile
hello: main.cpp hello.cpp factorial.cpp
    ${CC} ${CFLAGS} $? ${LDFLAGS} -o $@
```
**alternatively:**
```Makefile
hello: main.cpp hello.cpp factorial.cpp
    ${CC} ${CFLAGS} $@.cpp ${LDFLAGS} -o $@
```
Common implicit rule is given below for the construction of .o(object) files out of .cpp(source file) files.
```Makefile
.o.cpp:
    ${CC ${CFLAGS} -c $<
```
**alternatively:**
```Makefile
.o.cpp:
    ${CC} ${CFLAGS} -c $*.c
```