# CMake Introduction
cmake - Cross-Platform Makefile Generator.

The `cmake` executable is the CMake command-line interface. It may be used to configure projects in scripts. Project configuration settings may be specified on the command line with the `-D` option. The `-i` option will cause `cmake` to ineractively prompt for such settings.

`CMake` is a cross-platform build system generator. Projects specify their build process with platform-independent `CMake` listfiles included in each directory of a source tree with the name `CMakeLists.txt`. Users build a project by using CMake to generate a build system for a native tool on their platform.

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

### Source Files
A CMake Language source file consists of zero or more [Command Invocations](#command-invocations) separated by newlines and optionally spaces and [Comments](#comments):
```
file         ::= file_element*
file_element ::= command_invocation line_ending | (bracket_comment | space)* line_ending
line_ending  ::= line_comment? newline
space        ::= <match '[ \t]+'>
newline      ::= <match '\n'>
```
Note that any source file line not inside [Command Arguments](#command-arguments) or a [Bracket Comment](#bracket-comment) can end in a [Line Comment](#line-comment).

### Command Invocations
A command invocation is a name followed by paren-enclosed arguments seperated by whitespace:
```
command_invocation  ::= space* identifier space* '(' arguments ')'
identifier          ::= <match 'A-Za-z_][A-Za-z0-9_]*'>
arguments           ::= argument? separated_arguments*
separated_arguments ::= separation+ argument? | separation* '(' arguments ')'
separation          ::= space | line_ending
```
For example:
```CMake
add_executable(hello hello.c)
```
Command nemes are case-insensitive. Nested unquoted parentheses in the arguments must balance. Each `(` or `)` is given to the command invocation as a literal [Unquoted Argument](#unquoted-argument). This may be in calls to the `if()` command to enclose conditions. For example:
```CMake
if(FALSE AND (FALSE OR TRUE)) # evaluates to FALSE
```

### Command Arguments
There are three types of arguments within [Command Invocations](#command-invocations):
```
argument  ::= bracket_argument | quoted_argument | unquoted_argument
```

#### Bracket Argument
A bracket argument, inspired by Lua long bracket systax, encloses content between opening and closing "brackets" of the same length:
```
bracket_argument ::= bracket_open bracket_content bracket_close
bracket_open     ::= '[' '='* '['
bracket_content  ::= <any text not containing a bracket_close with the same number of '=' as the bracket_open>
bracket_close    ::= ']' '='* ']'
```
An opening bracket is written `[` followed by zero or more `=` followed by `[`. The corresponding closing bracket is written `]` followed by the same number of `=` followed by `]`. Brackets do not nest. A unique length may always be chosen for the opening and closing brackets to contain closing brackets of other lengths.

Bracket argument content consists of all text between the opening and closing brackets, except that one newline immediately following the opening bracket, if any, is ignored. No evaluation of the enclosed content, such as [Escape Sequences](#escape-sequences) or [Variable References](#variable-references), is performed. A bracket argument is always given to the command invocation as exactly one argument.

For example:
```CMake
massage([=[
This is the first line in a bracket argument with bracket length 1.
No \-excape sequences or ${variable} references are evaluated.
This is always one argument even though it contains a ; character.
The text does not end on a closing bracket of length 0 like ]].
It dose end in a closing bracket of length 1.
]=])
```

#### Quoted Argument
A quoted augument encloses content between opening and closing double-quote characters:
```
quoted_argument     ::= '"' quoted_element* '"'
quoted_element      ::= <any character except '\' or '"'> | escape_sequence | quoted_continuation
quoted_continuation ::= '\' neline
```
Quoted augument content consists of all text between opening and closing quotes. Both [Escape Sequences](#escape-sequences) and [Variable References](#variable-references) are evaluated. A quoted argument is always given to the command invocation as exactly one argument.

For example:
```CMake
massage("This is a quoted argument containing multiple lines.
This is always one argument even though it contains a ; character.
Both \\-escape sequences and ${variable} references are evaluated.
The text does not end on an escaped double-quoted like \".
It does end in an unescapsed double quote.
")
```
The final `\` on any line ending in an odd number of backslashes is treated as a line continuation and ignored along with the immediately following newline character. 

For example:
```CMake
message("\
This is the first line of a quoted argument. \
In fact it is the only line but since it is long \
the source code uses line continuation.\
")
```

#### Unquoted Argument
An unquoted argument is not enclosed by any quoting syntax. It may not contain any whitespace, `(`, `)`, `#`, `"`, or `\` except when excaped by a backslash:
```
unquoted_argument  ::= unquoted_element+ | unquoted_legacy
unquoted_element   ::= <any character except whitespace or one of '()#"|'> | escape_sequence
unquoted_lagacy    ::= <see note on text>
```
Unquoted argument content consists of all text in a continuous block of allowed or escaped characters. Both [Escape Sequences](#escape-sequences) and [Variable References](#variable-references) are evaluated. The resulting value is divided in the same way Lists divide into elements. Each non-empty element is given to the command invocation as an argument. Therefore an unquoted argument may be given to a command invocation as zero or more arguments.

For example:
```CMake
foreach(arg
    NoSpace
    Escaped\ Space
    This;Divides;Into;Five;Arguments
    Escaped\;Semicolon
    )
    message("${arg}")
endforeach()
```

### Escape Sequences
An escape sequence is a `\` followed by one character:
```
escape_sequence   ::= escape_identify | escape_encoded | escape_semicolon
escape_identity   ::= '\' <match '[^A-Za-z0-9;]'>
escape_encoded    ::= '\t' | '\r' | '\n'
escape_semicolon  ::= '\;'
```
A `\` followed by a non-alphanumeric character simply encodes the literal character without interpreting it as syntax. A `\t`, `\r`, or `\n` encodes a tab, carriage return, or newline character, respectively. A `\;` outside of any [Variable References](#variable-references) encodes itself but may be used in an [Unquoted Argument](#unquoted-argument) to encode the `;` without dividing the argument value on it. A `\;` inside [Variable References](#variable-references) encodes the literal `;` character.

### Variable References
### Comments
#### Bracket Comment
#### Line Comment
