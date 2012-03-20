## GNU工具

### GCC

- History

The original author of the GNU C Compiler (GCC) is Richard Stallman, the founder of the GNU Project. The GNU project was started in 1984 to create a complete Unix-like operating system as free software, in order to promote freedom and cooperation among computer users and programmers. Every Unix-like operating system needs a C compiler, and as there were no free compilers in existence at that time, the GNU Project had to develop one from scratch.

The first release of GCC was made in 1987. This was a significant breakthrough, being the first portable ANSI C optimizing compiler released as free software.

A major revision of the compiler came with the 2.0 series in 1992, which added the ability to compile C++. In 1997, an experimental branch of the compiler (EGCS) was created, to improve optimization and C++ support. Following this work, EGCS was adopted as the new main-line of GCC development, and these features became widely available in the 3.0 release of GCC in 2001.

---

- Link order of object files

On Unix-like systems, the traditional behavior of compilers and linkers is to search for external functions from left to right in the object files specified on the command line. This means that the object file which contains the definition of a function should appear after any files which call that function.

Most current compilers and linkers will search all object files, regardless of order, but since not all compiler do this it is best to follow the convention of ordering object files from left to right.

This is worth keeping in mind if you ever encounter unexpected problems with undefined references, and all the necessary object files appear to be present on the command line.


---

- Linking with external libraries

A library is a collection of precompiled object files which can be linked into programs. Libraries are typically stored in special *archive* files with the extension \'.a\', referred to as *static libraries*. They are created from object files with a separate tool, the GNU archiver **ar**, and used by the linker to resolve references to the functions at compile-time.(dynamic linking at runtime using *shared libraries*)

The standard system libraries are usually found in the directories \'/usr/lib\' and \'/lib\'. For example, the C math library is typically stored in the file \'/usr/lib/libm.a\' on Unix-like systems. The corresponding prototype declarations for the functions in this library are given in the header file \'/usr/include/math.h\'. The C standard library itself is stored in \'/usr/lib/libc.a\' and contains functions specified in the ANSI/ISO.C standard.

---

- Setting search paths

By default, gcc searches the following directories for header files:

	/usr/local/include/

	/usr/include/

and the following directories for libraries:

	/usr/local/lib/

	/usr/lib/

The list of directories for header files is often referred to as the *include path*, and the list of directories for libraries as the *library search path* or *link path*

The directories on these paths are searched in order, from first to last in the two lists above.

When additional libraries are installed in other directories it is necessary to extend the search paths, in order for the libraries to be found. The compiler options \'-I\' and \'-L\' add new directories to the beginning of the include path and library search path respectively.

- Important options

**-Wall**

**-c**: creating object file from source file

**-o**: creating executable from object file(s)

**-l*Name***: In general, the compiler option \'-l*NAME*\' will attempt to link object files with a library file \'lib*NAME*.a\' in the standard library directories.
