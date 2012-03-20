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

A library is a collection of precompiled object files which can be linked into programs. Libraries are typically stored in special *archive* files with the extension '.a', referred to as *static libraries*. They are created from object files with a separate tool, the GNU archiver **ar**, and used by the linker to resolve references to the functions at compile-time.(dynamic linking at runtime using *shared libraries*)

The standard system libraries are usually found in the directories '/usr/lib' and '/lib'. For example, the C math library is typically stored in the file '/usr/lib/libm.a' on Unix-like systems. The corresponding prototype declarations for the functions in this library are given in the header file '/usr/include/math.h'. The C standard library itself is stored in '/usr/lib/libc.a' and contains functions specified in the ANSI/ISO.C standard.

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

When additional libraries are installed in other directories it is necessary to extend the search paths, in order for the libraries to be found. The compiler options '-I' and '-L' add new directories to the beginning of the include path and library search path respectively.

---

- Shared libraries and static libraries

External libraries are usually provided in two forms: *static libraries* and *shared libraries*. Static libraries are the '.a' files. 

When a program is linked against a static library, the machine code from the object files for any external functions used by the program is copied from the library into the final executable.

Shared libraries are handled with a more advanced form of linking, which makes the executable file smaller. They use the extension '.so', which stands for *shared object*.

An executable file linked against a shared library contains only a small table of the functions it requires, instead of the complete machine code from the object files for the external functions. Before the executable file starts running, the machine code for the external functions is copied into memory from the shared library file on disk by the operating system\-\-\- a process referered to as *dynamic linking*.

Dynamic linking makes executable files smaller and saves disk space, because one copy of a library can be shared between multiple programs. Most operating system also provide a virtual memory mechanism which allows one copy of a shared library in physical memory to be used by all running programs, saving memory as well as disk space.

Furthermore, shared libraries make it possible to update a library without recompiling the programs which use it (provided the interface to the library does not change).

---

- preprocessor

The preprocessor(cpp) expands macros in source files before they are compiled.

It is possible to see the effect of the preprocessor on source file directly, using the '-E' option of gcc. The ability to see the preprocessed source files can be useful for examing the effect of system header files, and finding declarations of system functions.

The preprocessed system header files usually generate a lot of output. This can be redirected to a file, or saved more conveniently using the GCC '-save-temps' option:

	$ gcc -c -save-temps hello.c

After running this command, the preprocessed output will be available in the file 'hello.i'. The '-save-temps' option also saves '.s' assembly files and '.o' object files in addition to preprocessed '.i' files.

---

- Compiling for debugging

Normally, an executable file does not contain any references to the original program source code, such as variable names or line-numbers\-\-the executable file is simply the sequence of machine code instructions produced by the compiler. This is insufficient for debugging, since there is no easy way to find the cause of an error if the program crashes.

GCC provided the '-g' debug option to store addtional debugging information in object files and executables. This debugging information allows errors to be traced back from a specific machine instruction to the corresponding line in the original source file.

The debug option works by storing the names of functions and variables (and all the references to them), with their corresponding source code line-numbers, in a *symbol table* in object files and executables.

In addition to allowing a program to be run under the debugger, another helpful application of the '-g' option is to find the circumstances of a program crash.

When a program exits abnormally the operating system can write out a *core* file, usually named 'core', which contains the in-memory state of the program at the time it crashed. Combined with information from the symbol table produced by '-g', the core file can be used to find the line where the program stopped, and the values of its variables at that point.

Here is a simple program containing an invalid memory access bug, which we will use to produce a core file:

	int a (int *p);

	int
	main (void)
	{
		int *p = 0; /* null pointer*/
		return a (p);
	}
	
	int
	a (int *p)
	{
		int y = *p;
		return y;
	}

The program attempts to dereference a null pointer **p**, which is an invalid operation. On most systems, this will cause a crash.**(Historically, a null pointer has typically corresponded to memory location 0, which is usually restricted to the operating system kernel and not accessible to user program)**.

	$ gcc -Wall -g null.c
	$ ./a.out
	Segmentation fault (core dumped)

Whenever the error message 'core dumped' is displayed, the operating system should produce a file called 'core' in the current directory.This core file contains a complete copy of the pages of memory used by the program at the time it was terminated. Incidentally, the term *segmentation fault* refers to the fact that the program tried to access a restricted memory "segment" outside the area of memory which had been allocated to it.

Some systems are configured not to write core files by default, since the files can be large and rapidly fill up the available disk space. In the *GNU Bash shell* the command **ulimit -c** controls the maximum size of core files. If the size limit is zero, no core files are produced. The current size limit can be shown by command below:

	$ ulimit -c
	0

If the result is zero, as shown above, then it can be increased with the following command to allow core files of any size to be written:

	$ ulimit -c unlimited

Note that this setting only applies to the current shell. To set the limit for future sessions the command should be placed in an appropriate login file, such as '.bash_profile' for the GNU Bash shell.

Core files can be loaded into the GNU Debugger gdb with the following command:

	$ gdb EXECUTABLE-FILE CORE-FILE

---

- Compiling with optimization

In order to control compilation-time and compiler memory usage, and the trade-offs between speed and space for the resulting executable, GCC provides a range of general optimization levels, numbered from 0-3, as well as individual options for specific types of optimization.

An optimization level is chosen with the command line option '-O*LEVEL*', where *LEVEL* is a number from 0 to 3. The effects of the different optimization levels are described below:

**'-O0' or no '-O' option (default)**: At this optimization level GCC does not perform any optimization and compiles the source code in the most straightforward way possible. Each command in the source code is converted directly to the corresponding instructions in the executable file, without rearrangement. This is the best option to use when debugging a program.

The option '-O0' is equivalent to not specifying a '-O' option.

**'-O1' or '-O'**: This level turns on the most common forms of optimization that do not require any speed-space tradeoffs. With this option the resulting executables should be smaller and faster than with '-O0'.

The more expensive optimizations, such as instruction scheduling, are not used at this level.

Compiling with the option '-O1' can often take less time than compiling with '-O0', due to the reduced amounts of data that need to be processed after simple optimizations.

**'-O2'**: This option turns on further optimizations, in addition to those used by '-O1'. These additional optimizations include instruction scheduling. Only optimizations that do not require any speed-space tradeoffs are used so the executable should not increase in size. The compiler will take longer to compile programs and require more memory than '-O1'.**This option is generally the best choice for deployment of a program**,because it provides maximum optimization without increasing the executable size. It is the default optimization level for releases of GNU packages.

**-O3**: This option turns on more expensive optimizations, such as function inlining, in addition to all the optimizations of the lower levels '-O2' and '-O1'. The '-O3' optimization level may increase the speed of the resulting executable, but can also increase its size. Under some circumstances where these optimizations are not favorable, this option might actually make a program slower.

**-funroll-loops**: This option turns on loop-unrolling, and is independent of the other optimization options. It will increase the size of an executable. Whether or not this option produces a beneficial result has to be examined on a case-by-case basis.

**-Os**: This option selects optimizations which reduce the size of an executable. The aim of this option is to produce the smallest possible executable, for systems constrained by memory or disk space. In some cases a smaller executable will also run faster, due to better cache usage.

It is important to remember that the benefit of optimization at the highest levels must be weighed against the cost. **The cost of optimization includes greater complexity in debugging**, and increased time and memory requirements during compilation. For most purposes it is satisfactory to use '-O0' for debugging, and '-O2' for development and deployment.

- Optimization and compiler warnings

When optimization is turned on, GCC can produce additional warnings that do not appear when compiling without optimization.

As part of the optimization process, the compiler examines the use of all variables and their initial values\-\-\-this is referred to as *data-flow* analysis. It forms the basis for other optimization strategies, such as instruction scheduling. A side-effect of data-flow analysis is that the compiler can detect the use of uninitialized variables.

The '-Wuninitialized' option (which is included in '-Wall') warns about variables that are read without being initialized. It only works when the program is compiled with optimization to enable data-flow analysis.

---

- Verbose compilation

The '-v' option can also be used to display detailed information about the exact sequence of commands used to compile and link a program.

The output produced by '-v' can be useful whenever there is a problem with the compilation process itself. It displays the full directory paths used to search for header files and libraries, the predefined preprocessor symbols, and the object files and libraries used for linking.

---

- Compiler-related tools

**ar**: creating a library with the GNU archiver.

**gprof**: The GNU profiler *gprof* is a useful tool for measuring the performance of a program\-\-\-it records the number of calls to each function and the amount of time spent there, on a per-function basis.Functions which consume a large fraction of the run-time can be identified easily from the output of *gprof*. Efforts to speed up a program should concentrate first on those functions which dominate the total run-time.

To use profiling, the program must be compiled and linked with the '-pg' profiling option:

	$ gcc -Wall -c -pg collatz.c
	$ gcc -Wall -pg collatz.o

This creates an instrumented executable which contains additional instructions that record the time spent in each function.

If the program consists of more than on source file then the '-pg' option should be used when compiling each source file, and used again when linking the object files to create the final executable. Forgetting to link with the option '-pg' is a common error, which prevents profiling from recording any useful information.

While running the instrumented executable, profiling data is silently writtten to a file 'gmon.out' in the current directory. It can be analyzed with *gprof* by giving the name of the executable as an argument:

	$ gprof a.out

**gcov**: The GNU coverage testing tool *gcov* analyses the number of times each line of a program is executed during a run. This makes it possible to find areas of the code which are not used, or which are not exercised in testing. When combined with profiling information from *gprof* the information from coverage testing allows efforts to speed up a program to be concentrated on specific lines of the source code.

To enable coverage testing the program must be compiled with the following options:

	$ gcc -Wall -fprofile-arcs -ftest-coverage cov.c

This creates an instrumented executable which contains additional instructions that record the number of times each line of the program is executed. The option *-ftest-coverage* adds instructions for counting the number of times individual lines are executed, while '-fprofile-arcs' incorporates instrumentation code for each branch of the program. Branch instrumentation records how frequently different paths are taken through 'if' statements and other conditionals. The executable must then be run to create the coverage data:

	$ ./a.out

The data from the run is written to several files with the extensions '.bb' '.bbg' and '.da' respectively in the current directory. This data can be analyzed using the *gcov* command and the name of a source file:

	$ gcov cov.c

The gcov command produces an annotated(带注释的)version of the original source file, with the file extension '.gcov', containing counts of the number of times each line was executed. Lines which were not executed are marked with hashes '######'. The command "grep '######' *.gcov" can be used to find parts of a program which have not been used.

---

- How the compiler works

The sequence of commands executed by a single invocation of GCC consists of the following stages:

	preprocessing (to expand macros)

	compilation (from source code to assembly language)

	assembly (from assembly language to machine code)

	linking (to create the final executable)

The purpose of the assembler is to convert assembly language into machine code and generate an object file. When there are calls to external functions in the assembly source file, the assembler leaves the addresses of the external functions undefined, to be filled in later by the linker.

The final stage of compilation is the linking of object files to create an executable. In practice, an executable requires many external functions from system and C run-time (crt) libraries. Consequently, the actual link commands used internally by GCC are complicated.

---

- Examing compiled files

**Identifying files**: The **file** command looks at the contents of an object file or executable and determines some of its characteristics, such as whether it was compiled with dynamic or static linking.

**Examing the symbol table**: The **nm** command can display the symbol table contained by executables and object files.**The most common use of the nm command is to check whether a library contains the definition of a specific function**, by looking for a 'T' entry in the second column against the function name in the output of nm command.

**Finding dynamically linked libraries**: When a program has been compiled using shared libraries it needs to load those libraries dynamically at run-time in order to call external functions. The command **ldd** examines an executable and displays a list of the shared libraries that it needs.

The **ldd** command can also be used to examine shared libraries themselves, in order to follow a chain of shared library dependencies.

---

- Important options

**-Wall**

**-c**: creating object file from source file

**-o**: creating executable from object file(s)

**-l*Name***: In general, the compiler option '-l*NAME*' will attempt to link object files with a library file 'lib*NAME*.a' in the standard library directories.

**-E**: causes gcc to run the preprocessor, display the expanded output, and then exit without compiling the resulting source code.

**-S**: instructs *gcc* to convert the [preprocessed] C source code to assembly language without creating an object file.

**-save-temps**

**-g**: the debug option to store additional debugging information in object files and executables.

---

**以上内容为《An introduction to GCC》阅读笔记**
