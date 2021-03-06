# C Programming Language

1. **C data types**

char, int, float, double

user defined (typedef): structs, unions, enums

2. **What's a pointer? What's its size?**

It's a variable that holds the address of another variable. In this way, the variable that the
pointer variable points to can be modified indirectly. 

The size is system specific. Typically it has the same size as the processor word, e.g. in an ARM32
board, the pointers typically have a 4 bytes size (32 bits).

3. **Null pointer and void pointer**

void pointer are used as general-purpose pointers. They're good in the case that you don't priorly 
know what data type to return from a function, such as in malloc. It's important to cast your
pointer in order for you to be able to use it.

A null pointer is a pointer which points nothing. Some uses of the null pointer are: a) To
initialize a pointer variable when that pointer variable isn't assigned any valid memory address
yet. b) To pass a null pointer to a function argument when we don't want to pass any valid memory
address.

4. **Compilation stages - what happens at each stage?**

TODO: Add diagram of the compilation process

In order to compile hello.c, the following steps need to be done.

- Preprocessor: the preprocessor takes care of commands that begin with #, e.g., #define, #include,
#pragma, etc.

  - Removes comments
  - Expands macros
  - Expands included files

If you included a header file such as #include <stdio.h>, it will look for the stdio.h file and
copy the header file into the source code file.

The preprocessor also generates macro code and replaces symbolic constants defined using #define
with their values.

output: hello.i

- Compiler: compiling is the second step. It takes the output of the preprocessor and generates
assembly language, an intermediate human readable language, specific to the target processor.

output: hello.s

- Assembler: assembly is the third step of compilation. The assembler will convert the assembly
code into pure binary code or machine code (zeros and ones). This code is also known as object
code.

output: hello.o

- Linker: Linking is the final step of compilation. The linker merges all the object code from
multiple modules into a single one. If we are using a function from libraries, linker will link
our code with that library function code.

In static linking, the linker makes a copy of all used library functions to the executable file.
In dynamic linking, the code is not copied, it is donde by just placing the name of the library
in the binary file.

5. **Difference between a switch and an if statement**

6. **Common problems - segmentation fault, memory leaks**

7. **C reserved words - const (in parameters), extern, private, volatile, static (functions, variables), etc. for both variables and functions.**

8. **Pointers to functions**

9. **Libraries**

Libraries are collections of precompiled functions that have been written to be reusable. Typically,
they consists of sets of related functions to perform a common task. 

The C compiler (or more exactly, the linker) needs to be told which libraries to search, because
by default it searches only the standard C library.

A library filename always starts with lib. Then follow the part indicating what library this is (like c for the C library,
or m for the mathematical library). The last part starts with a dot (.), and specifies the type of library:

  - .a for static libraries
  - .so for shared libraries

The simplest form of library is just a collection of object files kept together in a ready-to-use form.
When a program needs to use a function stored in the library, it includes a header file that
declares the function. The compiler and linker take care of combining the program code and the library
into a single executable program. You must use the -l option to indicate which libraries other than
the standard C runtime library are required.

  - **Static libraries**, also known as archives, conventionally have names that end with .a. Example,
    /usr/lib/libc.a for the standard C library. You can create your own static libraries using ar (for archive) program 
    and compiling functions separately with gcc -c. [Example of a static library](). For futher information how to
    create a shared library check Introduction to Programming Linux book p.11. One disadvantage of static libraries is that
    when you run many applications at the same time and they all use functions from the same library, you may end up with many
    copies of the same functions in memory and indeed many copies in the program files themselves. This can consume a large
    amount of valuable memory and disk space. Shared libraries can overcome this disadvantage.

  - **Shared Libraries** have the so suffix, e.g., the shared version of the standard math library is /lib/libm.so. When a
    program uses a shared library, it is linked in such a way that it doesn't contain function code itself, but references to
    shared code that will be made available at run time. When the resulting program is loaded into memory to be executed, the
    function references are resolved and calls are made to the shared library, which will be loaded into memory if needed. In
    this way, the system can arrange for a single copy of a shared library to be used by many applications at once and stored
    just once on the disk. An additional benefit is that the shared library can be updated independently of the applications
    that rely on it.

    In Linux, the program (dynamic loader) that takes care of loading shared libraries and resolving client program functions
    references is called ld.so.

    You can verify which libraries have been linked in this or any other program by using the
    readelf command:

    ``` $ readelf -a myprog | grep "Shared library" ```

    Shared libraries need a runtime linker, which you can expose using this:

    ``` $ readelf -a myprog | grep "program interpreter" ```

10. **The C Library**

The C library is not a single library file. It is composed of four main parts that together implement the POSIX API:

  - libc: The main C library that contains the well-known POSIX functions such as printf, open, close, read, write, and so on.
  - libm: Contains math functions such as cos, exp, and log
  - libpthread: Contains all the POSIX thread functions with names beginning with pthread_
  - librt: Has the real-time extensions to POSIX, including shared memory and asynchronous I/O

The first one, libc, is always linked, in but the others have to be explicitly linked with the -l option.

11. **main() arguments**

When a Linux program written in C runs, it starts at the function main. For these programs, main is declared as

```int main(int argc, char *argv[])```

where argc is a count of the program arguments and argv is an array of character strings representing the arguments themselves.

12. **Macros vs constants**

Macros are handled by the pre-processor - the pre-processor does text replacement in your source
file, replacing all occurances of 'A' with the literal 8.

Constants are handled by the compiler. They have the added benefit of type safety.

For the actual compiled code, with any modern compiler, there should be zero performance difference
between the two.

