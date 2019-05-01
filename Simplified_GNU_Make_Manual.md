# Makefile manual
Makefile is a program building tool which runs on Unix, linux, and their flavors. It aids in simplifying building the program executables that may need various modules. To determine how the modules need to be compiled or recompiled together, **make** takes the help of user-defined makefiles.
Makefile guides the **make** utility while compiling and linking program modules.

## 1. Why Makefile?
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

## 2. MACROS
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
### Conventional Macros
There are various default macros. You can see them by typing `make -p` to print out the defaults. Most are pretty obvious from the rules in which they are used.

These predefined variables, i.e., macros used in implicit rules fall into two classes. They are as follows:
1. Macros that are names of programs(such as CC);
2. Macros that contain arguments of the programs(such as CFLAGS).

Common variables used as names of programs in built-in rules of makefile:

---
|Macros|Comment|
|-|-|
|AS|Program to compile assembly files; default is "`as`".|
|CC|Program to compile C programs; default is "`cc`".|
|CXX|Program to compile C++ programs; default is "`g++`".|
|CPP|Program to run the C preprocessor, with results to standard output; default is "`${CC} -E`"|
|CTANGLE|Program to tanslate C web into C; default id "`ctangle`"|
|RM|Command to remove a file; default is "`rm -f`"|
---

Here is a table of variables whose value are additional arguments for the programs above. The **default values** for all of these is the **empty string**, unless otherwise noted.

---
|Macros|Comment|
|-|-|
|ASFLAGS|Extra flags to give to the assembler when explicitly invoked on a ".s" or ".S" file.|
|CFLAGS|Extra falgs to give to the C compiler.|
|CXXFLAGS|Extra flags to give to the C compiler.|
|CPPFLAGS|Extra flags to give to the C preprocessor and programs, which use it(such ad C and Fortran compiler).|
|LDFLAGS|Extra flags to give to compilers when they are supposed to invoke the linker, "ld".|
---

**NOTE:**
> You can cancel all variables used by implicit rules with the "`-R`" or "`--no-builtin-variables`" option.
> 
> You can also define macros at the command line as shown below:
> >`make CPP = /home/courses/cop4530/spring02`

## 3. Dependencies
It is very common that a file binary will be dependent on various source codes and source header files. Dependencies are important because they allow **make** to know about the source for any target.

For example:
```Makefile
hello: main.o factorial.o hello.o
    ${CC} main.o factorial.o hello.o -o hello
```
Here, we give an input to **make** that *hello* is dependent on main.o, factorial.o and hello.o files. Hence, whenever there is a change in any of these object files, **make** will take action.

At the same time, we need to tell the **make** how to prepare `.o` files.
```Makefile
main.o: main.cpp functions.h
    ${CC -c main.cpp
factorial.o: factorial.cpp functions.h
    ${CC} -c factorial.cpp
hello.o: hello.cpp functions.h
    ${CC} -c hello.cpp
```

## 4. Rules
The general sytax of a Makefile target rule is given below:
```
target [target...] : [dependent...]
    [command]
```
When you say "make target", **make** finds the target rule that applies; and if any of the dependents are newer than the target, make executes the commands one at a time(after macros substitution). If any dependents have to be made, that happens first(so you have a recursion).

**Make** terminates if any command returns a failure status. The following rule will be shown in such case:
```Makefile
clean:
    -rm *.o core *.core
```
**Make** ignores the returned status on command lines that begin with a dash(`-`).

**Make** echoes the commands, after macro substitution to show you what is happening. Sometimes you might want to turn that off.

For example:
```Makefile
install:
    @echo You must be root to install
```
People have come to expect certain targets in Makefile. You should always broese first. However, it is reasonable to expect that the targets `all`, `install`, and `clean` is found.
- make all - It compiles everything so that you can do local testing before installing applications.
- make install - It installs applications at right places
- make clean - It cleans applications, gets rid of the executables, any temporary files, objects files, etc.

### Makefile Implicit Rules
The command is one that is ought to work in all cases where we build an executable `x` out of the source code `x.cpp`. This can be stated as an implicit rule:
```Makefile
.cpp:
    ${CC} ${CFLAGS} $@.cpp ${LDFLAGS} -o $@
```
Another common implicit rule is for the construction of `.o`(object) files out of `.cpp`(source file).
```Makefile
.o.cpp
    ${CC} ${CFLAGS} -c $<
```
**alternatively:**
```Makefile
.o.cpp:
    ${CC} ${CFLAGS} -c $*.cpp
```
### Suffix Rules
**make** can automaticlly create a `.o` file, using `cc -c` on the corresponding `.c` file. These rules are built in the **make**, and you can take this advantage to shorten your Makefile. If you indicate just the `.h` files in the dependency line of the Makefile on which the current target is dependent on, **make** will know that corresponding `.c` file is already required. You don't heve to include the command for the compiler.

For example:
```Makefile
OBJECTS = main.o hello.o factorial.o
hello: ${OBJECTS}
    cc ${OBJECTS} -o hello
hello.o: functions.h
main.o: functions.h
factorial.o: functions.h
```
**make** uses a special target, named `.SUFFIXES` to allow you to define your own suffixes.

For example:
```Makefile
.SUFFIXES: .foo .bar
```
It informs **make** that you will be using these suffixes to make your own rules.

## 5. Directives
ToDo

## Other Features
### Recursive Use of Make
Recursive use of **make** means using **make** as a command in a makefile. This technique is useful when you want to separate makefile for various subsystems that composes a large system.

For example:
```Makefile
subsystem:
    cd subdir && ${MAKE}
```
**alternatively:***
```Makefile
subsystem:
    ${MAKE} -C subdir
```
### Communicating Variables to a Sub-Make
Variable values of the top-level **make** can be passed to the sub-make through the environment by explicit request. These variables are defined in the sub-make as defaults. You cannot override what is specified in the makefile used by the sub-make makefile unless you use the "`-e`" switch.

To pass down or export a variable, **make** adds the variable and its value to the environment for running each command. The sub-make, in turn, uses the environment to initialize its table of variable values.

The special variables `SHELL` and `MAKEFLAGS` are always exported(unless you unexported them). `MAKEFILES` is exported if you set it to anything.

If you want to export specific variables to a sub-make, use the export directive, as shown below:
```Makefile
export variable...
```
If you want to prevent a variable from being exported, use the unexport directive, as shown below:
```Makefile
unexport variable...
```
### Including Header File
Provide the path if header files can be done using `-I` option in makefile.

For example:
```Makefile
INCLUDES = -I "../include"
CC = gcc
LIBS = -lm
CFLAGS = -g -Wall
OBJ = main.o factorial.o hello.o

hello: ${OBJ}
    ${CC} ${CFLAGS} ${INCLUDES} -o $@ ${OBJS} ${LIBS}
.cpp.o:
    ${CC} ${CFLAGS} ${INCLUDES} -c $<
```
### Running Makefile from Command Prompt
If you have prepared the Makefile with name "`Makefile`", then simply write make at command prompt and it will run the Makefile file. But if you have given any other name to the Makefile, then use the following command:
```Makefile
make -f your-makefile-name
```

### Same as C/C++
Following syntaxes are similar to C/C++.
> +=
> ifeq ... else... endif
> ifneq ... else... endif
> include ...
> override variable = value
> \