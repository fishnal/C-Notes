# CMSC 216H Midterm #2 Review

+ [Unix Commands](#unix)
	+ [`cp`](#unix-cp)
	+ [`ls`](#unix-ls)
	+ [`cd`](#unix-cd)
	+ [`pwd`](#unix-pwd)
	+ [`rm`](#unix-rm)
	+ [`mv`](#unix-mv)
	+ [`diff`](#unix-diff)
	+ [`touch`](#unix-touch)
	+ [`gcc`](#unix-gcc)
	+ [`rmdir`](#unix-rmdir)
	+ [`tar`](#unix-tar)
	+ [`grep`](#unix-grep)
	+ [`chmod`](#unix-chmod)
+ [C Specifics](#clang)
	+ [General Info](#clang-info)
	+ [Data Types](#clang-datatypes)
	+ [Operators](#clang-ops)
	+ [Preprocessor](#clang-preproc)
		+ [`#include`](#clang-preproc-include)
		+ [`#define`](#clang-preproc-define)
		+ [`#ifdef`](#clang-preproc-ifdef)
	+ [Keywords](#clang-kws)
	+ [Expresions](#clang-expr)
	+ [Conditional Expressions](#clang-condtls)
	+ [Loops](#clang-loops)
	+ [Pointers](#clang-ptrs)
		+ [Pointer Arithmetic](#clang-ptrs-arithmetic)
		+ [`void *`](#clang-ptrs-void)
	+ [Dynamic Memory Allocation](#clang-dynamicmem)
		+ [Stack](#clang-dynamicmem-stack)
		+ [Heap](#clang-dynamicmem-heap)
		+ [`malloc`](#clang-dynamicmem-malloc)
		+ [`calloc`](#clang-dynamicmem-calloc)
		+ [`realloc`](#clang-dynamicmem-realloc)
		+ [`free`](#clang-dynamicmem-free)
	+ [Arrays](#clang-arrays)
		+ [Multi Dimensional Arrays](#clang-arrays-multidim)
	+ [Strings](#clang-string)
		+ [`strlen`](#clang-string-strlen)
		+ [`strcpy`](#clang-string-strcpy)
		+ [`strcmp`](#clang-string-strcmp)
		+ [`strcat`](#clang-string-strcat)
		+ [`memcpy`](#clang-string-memcpy)
		+ [`memmove`](#clang-string-memmove)
	+ [I/O](#clang-io)
	+ [Enumerations](#clang-enum)
	+ [Structures](#clang-struct)
	+ [Unions](#clang-union)
	+ [Functions](#clang-func)
		+ [Function Pointers](#clang-func-ptr)
	+ [Standard I/O](#clang-stdio)
	+ [`exit`](#clang-exit)
	+ [`err`](#clang-err)
	+ [`perror`](#clang-perror)
	+ [Linkage](#clang-linkage)
	+ [Command Line Arguments](#clang-cla)
	+ [Recursion](#clang-recursion)
+ [Linked Lists](#linked-lists)
+ [AVR Assembly](#avr)
	+ [Registers](#avr-registers)
	+ [Pointer Registers](#avr-ptr-registers)
	+ [Functions](#avr-funcs)
	+ [SRAM](#avr-sram)
	+ [Recursion](#avr-recursion)
	+ [Instructions](#avr-instructions)
+ [Memory Maps](#memmap)
+ [Big Endian vs. Little Endian](#endian)
+ [Bitwise Operators](#bitwiseops)
	+ [One's and Two's Complement](#bitwiseops-complements)
+ [Debuggers](#debug)
	+ [`gdb`](#debug-gdb)
	+ [`valgrind`](#debug-valgrind)
	+ [`splint`](#debug-splint)
+ [Electrical Engineering](#ee)
	+ [Resistors](#ee-resistors)
	+ [Voltage Dividers](#ee-voltdiv)
	+ [Kirchhoff's Laws](#ee-kirchhofflaws)
+ [References](#refs)

___

## <a id="unix"></a> Unix Commands

+ <a id="unix-cp"></a> `cp` copies files and directories
	+ Usage: `cp [opts] <SRC> <DIR>`
	+ `-r|-R|--recursive` copies directories recursively
+ <a id="unix-ls"></a> `ls` lists directory contents
	+ Usage: `ls [opts] [FILE]` lists info about file
	+ `-a` includes ALL hidden files (including `.` and `..`)
	+ `-A` includes MOST hidden files (excludes `.` and `..`)
	+ `-d|--directory` lists directories but not their contents
	+ `-R|--recursive` lists subdirectories recursively
+ <a id="unix-cd"></a> `cd` changes current working directory
	+ `.` will change to current working directory (no real effect)
	+ `..` will go up one level (if it can)
+ <a id="unix-pwd"></a> `pwd` prints name of current working directory
+ <a id="unix-rm"></a> `rm` removes files or directories
	+ Usage: `rm [opts] <FILE>`
	+ `-r|-R|--recursive` removes directories and their contents recursively
	+ `-d|--dir` removes empty directories
+ <a id="unix-mv"></a> `mv` moves/renames files
	+ Usage: `mv <SRC> <DEST>` renames `SRC` to `DEST`
	+ Usage: `mv <SRC> <DIR>` moves `SRC` to `DIR`
	+ ***how do you move a file up a directory***
+ <a id="unix-diff"></a> `diff` compares files line by line
	+ Usage: `diff [opts] <FILE1/DIR1> <FILE2/DIR1>`
	+ `-bwi` ignores whitespaces, but not empty lines
	+ `-Bwi` ignores whitespaces and empty lines
+ <a id="unix-touch"></a> `touch` changes the timestamp on a file
	+ Usage: `touch [opts] <FILE>`
	+ If used on existing file, file's timestamp changed to now
	+ If used on non-existing file, creates that file
	+ `-c|--no-create` does not create a file
+ <a id="unix-gcc"></a> `gcc` compiles C/C++ files
	+ Usage: `gcc [opts] <FILES>`
	+ `-c` compiles but does not link (output file ends with `.o`)
	+ `-o <FILE>` places output file in specified file
	+ `-g` compiles with debugging symbols
+ <a id="unix-rmdir"></a> `rmdir` removes empty directories
	+ Usage: `rmdir [opts] <dir>`
	+ `-p|--parents` removes directory and it's ancestors (if they're also empty)
+ <a id="unix-tar"></a> `tar` stores and extracts files from a tape archive
	+ Usage: `tar [opts]`
	+ `-c` creates a new archive
	+ `-z` indicates the gzip compression method
	+ `-f` indicates the archive file
	+ `-x` extracts files from an archive
	+ Creating a compressed `.tar.gz` file: `tar -czf dest.tar.gz srcdir`
	+ Extracting a compressed `.tar.gz` file: `tar -xzf src.tar.gz`
+ <a id="#unix-grep"></a> `grep` searches a file for a pattern
	+ Usage: `grep [opts] <pattern> <file>`
	+ Supports regular expressions for patterns
+ <a id="unix-chmod"></a> `chmod` changes the file mode bits (basically it's permissions)
	+ Usage: `chmod <permission> <file>`
	+ Permission is provided in as a 9-bit octal number or in `rwx` format
		+ `chmod 777 file` provides all permissions to everyone because 7 in octal is 111 in binary
		+ `chmod r---w---x file` provides read to owner, write to group, and x to others
	+ First 3 bits modify the owner's permission
	+ Second 3 bits modify group's permission
	+ Third 3 bits modify other's permission
	+ Each bit in the 3-bit sequence represents read, write, and execute permissions respectively

## <a id="clang"></a> C Specifics

### <a id="clang-info"></a> General Info

Things special about C:

+ Memory management, no garbage collection
+ Programmer trusted to check errors; no exception protocol
+ Programmer trusted to check input is valid and consistent
+ Compiled to machine code
+ Not built around object-oriented features (though can be simulated)

C does share some things with Java (well technically the other way around, but same difference):

+ Imperative
+ Compiled
+ Explicitly typed

Assume we use the following compilation flags for any of the below examples:

+ `-ansi` ANSI standard C programs (more portable)
+ `-Wall` includes a bunch of common warnings
+ `-Wshadow` warns whenever a local variable shadows another local variable
+ `-Wwrite-strings` give string constants the type `const char[]` so when copying address of that into a non-`const char *` pointer will warn you
+ `-pedantic-errors` errors for bad ideas
+ `-O0` don't optimize
+ `-g` include debugging symbols
+ `-fstack-protector-all` buffer overflow protection for all functions

This isn't used in most of our compilations, but it's important to know:

+ `-c` compile only

### <a id="clang-datatypes"></a> Data Types

+ The following can be signed or unsigned:
	+ `char` exactly 1 byte (8 bits)
	+ `short`
	+ `int` always at least as big as a `short`
	+ `long` always at least as big as an `int`
	+ `long long`
+ Those are usually signed when not specified (but not always like in ARM chips). Their sizes vary based on the machine.
+ Integer constants can be defined in decimal (`123`), octal with a leading zero (`012`), and hex (`0xff`).
+ A `char` constant, like `'a'`, represents the value of that character with the `int` type.
+ Unsigned types are *never* negative:
	+ `unsigned char`
	+ `unsigned short`
	+ `unsigned int`
		+ 8-bit signed `int` ranged from `-128` to `-127`
		+ 8-bit unsigned `int` ranged from `0` to `255`
+ Refer to `stdint.h` for integer data types
+ Floating types, when integers just don't cut it:
	+ `float`
	+ `double` default for symbolic constants
	+ Remember these have finite precision...
+ Be wary of implicit type conversions: arithmetic operators require operands to be of same type!
	+ As such, there's a heiarchy of types:
		+ Floating-precision over integer-precision
		+ Wide over small
		+ Unsigned over signed
	```c
	float f = 5.02;
	int i = 100;

	printf("%d\n", (int) f); /* 5 */
	printf("%f\n", (int) f); /* 0, compiler warns */
	printf("%f\n", i * f); /* roughly 502.0000 */
	/* takes the bytes in f and computes them as an integer, rather
	   than a float (in other words, it's not going to truncate
	   the float and them use that truncation as the integer to
	   print */
	printf("%d\n", f);
	```

### <a id="clang-ops"></a> Operators

+ `+`, `-`, `*`, `/`, `%`
+ `=`, `+=`, `-=`, `*=`, `/=`, `%=`
+ `++`, `--`
+ `(void *)` cast to another type (e.g. void pointer)
+ `==`, `!=`, `<`, `<=`, `>`, `>=`, `&&`, `||`
+ `()` parentheses for order of operations
+ Assignment operator evaluates to the `rvalue` (see the [end of pointers](#clang-ptrs))

### <a id="clang-preproc"></a> Preprocessor

`#include` and `#define` are both preprocessor directives. Directives end up being a "copy and paste" of whatever they are into wherever they are mentioned.

+ Both of these directives can only span one line

#### <a id="clang-preproc-include"></a> #include

+ Includes a header file and will copy the text of that header file into the current file
+ i.e. `#include <stdio.h>` copies text of `stdio.h` into the file
+ Angled brackets indicate file should be searched in the standard compiler paths

+ Quotes indicates the search should be exapnded to include current source directory (note that it also includes std. compiler paths)

#### <a id="clang-preproc-define"></a> #define

+ Defines object-like or function-like macros
+ By convention, defined symbols are in ALL CAPS
+ Object syntax: `#define <NAME> <token>`
+ Function syntax: `#define <NAME>(<parameters>) <token/body of function>`
	+ Note that there must be no space between the name and parameters
	+ Wrap parameters in parentheses before doing anything with them (preprocessor will directly copy and paste)
		```c
		#define SQUARE(a) a*a

		int main() {
		  int x = 2;
		  SQUARE(x); /* 4 */
		  SQUARE(x+1); /* 5 */
		}
		```
		We would think that the second would give us 9, after all (2+1) squared is 9. But the preprocessor replaces `SQUARE(x+1)` as it was given, which gives us `x+1*x+1`. We can fix this by wrapping the parameter `a` in the `SQUARE` macro when we're multiplying it against itself, like so:
		```c
		#define SQUARE(a) (a)*(a)
		```
	+ Functions are easier to write with multiple lines, but macros must be on line, so what do we do? We can use `\` at the end of the line and we can continue writing onto our next line!
		```c
		#include <stdio.h>
		#define MULTIPLE() printf("HELLO\n"); \
			printf("HELLO AGAIN\n");

		int main() {
		  MULTIPLE();
		  /* prints HELLO and HELLO AGAIN on two separate lines */
		}
		```
	+ Always wrap your function macros in do-while loops if they contain multiple statements (not necessarily multiple lines)
		```c
		#include <stdio.h>
		#define FOO(a) printf("%d\n", a); printf("%d\n", a);

		int main() {
		  if (0) FOO(5);
		}
		```
		We would think that nothing should print, but actually `5` is printed only once because preprocessor replaces this as
		```c
		if (0) printf("%d\n", 5); printf("%d\n", 5);
		```
		Which we can logically rewrite as
		```c
		if (0) {
		  printf("%d\n", 5);
		}
		printf("%d\n", 5);
		```
		That's not what we want! We can fix this by doing
		```c
		#define FOO(a) do { \
			printf("%d\n", a); \
			printf("%d\n", a); \
		} while(0);
		```
		This will then replace
		```c
		if (0) FOO(5);
		```
		with the following
		```c
		if (0) do { printf("%d\n", a); printf("%d\n", a); } while(0);
		```

#### <a id="clang-preproc-ifdef"></a> #ifdef

+ Enables compilation to happen conditionally
+ "If this symbol is defined, then compile the following"

```c
#define NAME "Vishal"

int main() {
  #ifdef NAME
    printf("NAME is %s\n", NAME);
    /* we can actually use concatenation here, but only because \
       they're symbolic constants*/
    printf("NAME is " NAME "\n");
    printf("CON" "CAT\n");
  #endif

    #ifdef NAME_TWO
    /* don't worry, this won't be read by the compiler because \
       the symbol NAME_TWO is not defined, so no seg fault */
    int *ptr = NULL;
    printf("%d\n", *ptr);
  #endif
}
```

### <a id="clang-kws"></a> Keywords

Only listing the notable ones in C that we use in class:

+ `const` makes an identifier constant
	```c
	const int a = 20;
	a = 30; /* error, a declared as constant */
	```
+ `enum` used for declaring an enumerated type ([more](#clang-enum))
	```c
	enum { WIN, LOSE, TIE } status;
	```
+ `extern` globalizes a symbol; declares that a variable/function has external linkage outside of the file it's declared in (default)
	```c
	/* file.c */
	void foo() { ... } /* by default an external function */
	int bar; /* by default an external variable */
	extern void extern_foo() { ... } /* function explicitly made external */
	extern int extern_bar; /* variable explicitly made external */
	```
+ `sizeof` evaluates size of a data (variable or constant) in bytes
	```c
	sizeof(int); /* returns at least 2 */
	sizeof(char); /* always 1 */
	sizeof(int *); /* however many bytes a pointer is on your machine */
	```
+ `static` localizes a symbol
	+ A global static variable is one that can only be referenced in the same file
	+ A global static function is one that can only be referenced by other functions in the same file
	+ A local static variable is a static variable inside a function. It's value persists until the end of the program.
	```c
	/* file.c */

	/* static symbol */
	static void static_foo() {
	  printf("I can't be called outside of this file");
	}

	void foo() {
	  /* static variable */
	  static int a = 0;
	  printf("%d\n", a);
	  a += 5;
	}

	foo(); /* prints 0, a is now 5 */
	foo(); /* prints 5, a is now 10 */
	```
	+ There are some small caveats with local static variables:
	```c
	void foo() {
	  static int a = 10;
	  static int b;
	  b = 5;

	  printf("%d\n", a++);
	  printf("%d\n", b++);
	}

	foo(); /* prints 10 5 */
	foo(); /* prints 11 5 */
	```
	This happens because `static int b;` gives `b` a garbage value, and then the next line `b = 5;` reassigns `b` to be 5. When `foo` is called the first time, `b` is a garbage value, set to 5, printed, and then incremented by 1 (so `b` is 6). Calling `foo` the second time, `b` is currently 6, but it gets reassigned to 5, printed, and incremented by 1 (so `b` is basically forever 6).
+ `struct` used for declaring a structure which can hold variables of different types under one name ([more](#clang-struct))
+ `typedef` used to explicitly associate a type with an identifier
	```c
	typedef float kg;
	kg kg1, kg2; /* bear and tiger are of type float */
	float f1, f2; /* can still use the original identifier */

	typedef long long ll;
	assert(sizeof(ll) == sizeof(long long)); /* true */
	```
+ `union` used for grouping different types of variable under one name ([more](#clang-union))
+ `void` indicates function doesn't have return value

### <a id="clang-expr"></a> Expressions

There is no boolean data type in C, instead numbers are used. `0` is false, and any other integer is true, including negatives.

### <a id="clang-condtls"></a> Conditional statements

Standard conditional statements:

+ `if`
+ `else if`
+ `else`
+ ternary operators
+ `switch` (same format as Java switch statements)
+ `#ifdef-#endif` are conditional statements as well but for preprocessing

### <a id="clang-loops"></a> Loops

Loops in C are typical loops you would see elsewhere. You have 3 main ones, `while`, `for`, and `do-while`. All of them have the same general syntax as described in many other languages. There are a few caveats in C though.

+ **CANNOT** declare a variable inside a for-loop. In fact, you can only declare variables before executing code statements.
	```c
	int x = 20;
	printf("%d\n", x);
	for (int i = 0; i < 3; i++) { ... }
	```
	This **will not** work. Instead, do
	```c
	int x = 20, i;
	printf(...);
	for (i = 0; i < 3; i++) { ... }
	```
+ Say for example you're iterating over a `char` array (effectively string). To get the length, you'd use `strlen(char *)` (from `string.h`), and you can get a simple `for-loop` setup like so.
	```c
	char *str = "hello";
	int i;
	for (i = 0; i < strlen(str); i++) { ... }
	```
	The question is, how many times is `strlen(str)` called? Likely 3 times. What if the string were declared as `char const *str = "hello"`? It could be only 1 time if the compiler was smart enough to figure out `str` was a constant string.

### <a id="clang-ptrs"></a> Pointers

In declarations, the asterisk (`*`) specifies a pointer type. Pointer values associate two pieces of info: a *memory address* and a *data type*.
A good trick in understanding the declaration of pointers is *quite literally* reading the declaration from right to left.

```c
int *ptr; /* pointer to integer */
const int *ptr2; /* pointer to integer constant (better read as constant integer, same idea here though); means you can't change value */
int *const ptr3; /* const pointer to integer; means you can't change address */
int **ptr4; /* pointer to pointer to integer */
```

We can *dereference* a pointer by using the asterisk (`*`) again. For example, if we wanted to dereference `ptr`, we simply do `*ptr` (don't try this yet, keep on reading).

In the above example, ALL the pointers have not been initialized. Attempting to dereference any of them is not exactly a good idea, and results in undefined behavior.

So how do we fix that? Simple, just assign a memory address location to that pointer. This can be done in a variety of ways: use `malloc`, `calloc`, or get the address of another appropriate variable (known as *referencing*). We can reference an address by using the "address-of" unary operator (`&`), as shown below:

```c
int x = 10, y = 50;
int *ptr = &x; /* points to address of x */
const int *ptr2 = &x; /* points to address of x, but cannot change value that ptr2 points to */
int *const ptr3 = &x; /* points to address of x, but now we cannot change the address that ptr3 points to */
int **ptr4;

/* some extra stuff */
*ptr = 20;

ptr4 = &ptr3;
*ptr4 = &y;
```

Ok so those last few lines in the example above are some important things to take note of.

`*ptr = 20` will change the value of `x` to 20, which also effectively changes `*ptr2` and `*ptr3` to `20` because they both point to the address of `x`. We can have the compiler stop us when do we such a thing if we really don't want `*ptr2` to change since we made `ptr2` point to a constant integer. We can simply make `x` a `const int`. The compiler will stop us saying "Hey, you could possibly change the value of `x`, a constant integer, by reassigning `*ptr`!" (this error stems from the `-pedantic-errors` flag)

`ptr4 = &ptr3; *ptr4 = &y;` looks more daunting but it's actually somewhat similar to the above scenario. In this scenario, the compiler will stop us and spit out an error. This is because `ptr4 = &ptr3` discards the idea that `ptr3` is a constant pointer. If we this went through execution, we could now change what `ptr3` points to, which is not what we want to happen at all. What we could do to change this is declare `ptr4` as a pointer to a constant pointer to an integer, or `int *const *ptr4` in C code. So now we can do `ptr4 = &ptr3`, but not `*ptr4 = &y` because that'll be changing the pointer that `ptr4` points to, which is intended to be constant.

Simiarly, if `x` were a `const int`, then `int *ptr = &x` and `int *const ptr3 = &x` are not allowed (compiler will throw an error about those specifically). That's because both `ptr` and `ptr3` never made a promise to keep the value of the integer they're pointing constant. DO NOT confuse `ptr3` as a pointer to a constant integer, that is `ptr2`. `ptr3` has a constant pointer, but the value of the integer it's pointing to can be changed via `*ptr3`.

#### <a id="clang-ptrs-arithmetic"></a> Pointer Arithmetic

There's also *pointer arithmetic*, which explains how adding numbers with memory addresses works. For example, say we have `int *ptr` that points to address `0x04`. Assuming the size of an `int` is 4 bytes, performing `*(ptr + 1)` will get the integer value at the memory address `0x08`. But didn't we just add 1 to `ptr`, so why isn't the referenced address `0x05`? Because the `+1` indicates to move forward `1*sizeof(int)` bytes in memory. So we advanced `0x04` 4 bytes foward to get `0x08`. The generalized way of viewing this is that `<data type> *ptr` will advance `n * sizeof(<data type>)` bytes ahead in memory when doing pointer arithmetic (`n` is some arbitrary amount). A shorter way of looking at this is jumping _memory blocks_ rather than the memory addresses themselves.

```c
/* assume sizeof(char) returns 1 */
char *ptr = ...; /* say it points to 0x04 */
char c = *(ptr + 1); /* will get char value at 0x04 + 1 -> 0x05 */

int *ptr2 = ...; /* say it points to 0x10 */
int i = *(ptr2 + 2); /* gets int value at 0x14 assuming int is 4 bytes */

/* this is the difference in the number of blocks,
   not the address themselves, so prints 2 */
printf("%li\n", (ptr2 + 2) - ptr);
```

Also note that `ptr++` is allowed as it is equivalent to `ptr = ptr + 1`. Also be wary of `*ptr++`, that **does not** increment that value that `ptr` points to; it dereferences `ptr`, and then increments the value of `ptr`. It's equivalent to calling `*(ptr++)`.


Important to understand `rvalue` and `lvalue` when dealing with "address-of" operator (`&`) and asterisk modifier (`*`):

+ `rvalue` refers to data value; can't have value assigned to it, which means can only appear on right side of assignment operator (think of right-value)
+ `lvalue` refers to a place/location where we can store a value; appears on left side of assignment operator (think of left-value).

#### <a id="clang-ptrs-void"></a> void *

A void pointer (`void *`) is a sort of "generic" pointer type. It can be converted to any other type without an explicit cast. But, you can't do dereference it or do pointer arithmetic with it; first cast it to a specific type then have your fun with it.

```c
void foo(void * void_ptr) {
  /* print memory address */
  printf("%p\n", void_ptr);
}

int *ptr = malloc(sizeof(int));
int **ptr2 = malloc(sizeof(int) * sizeof(int));

foo(ptr);
foo(ptr2); /* (int **) is a pointer to an (int *), so this is valid! */
```

Just a heads up, void pointers are **not** valid for [function pointers](#clang-func-ptr)!

### <a id="clang-dynamicmem"></a> Dynamic Memory Allocation

There are two things we need to understand before we engage in dynamic memory: the *stack* and *heap*.

#### <a id="clang-dynamicmem-stack"></a> Stack

+ The stack is a literal stack data structure (push and pop)
+ Automatic variables: like when you enter a scope/function variables are given some space, when you leave a scope space is reclaimed (simple and easy)
+ Function parameters and return values
+ When you leave scope, nothing that was allocated on the stack survives (why would it?)
+ Fixed in size because compiler needs to know the stack frame. This lets it setup the base pointer and stack pointer.
	+ Base pointer points to top of current scope's stack
	+ Stack pointer points to address at bottom of stack (grows downward)
		+ Any addresses below stack pointer are invalid
+ Small objects, think like couple megabytes.
+ We have no control over stack in the sense that we can't insert/remove some space in the middle of the stack (why would that make *any* sense?)

#### <a id="clang-dynamicmem-heap"></a> Heap

+ Think of it as a bunch of papers on the ground, with none of them overlapping (I guess they can be placed perfectly next to each other, not the point though).
+ When program wants something from heap, looks at a certain location/memory address and gets it.
+ Explicit memory allocations
+ BIG objects, think possibly 500 megabytes!
+ Sized as you need it

Dynamic memory allocation refers to manual memory management in C. So what are the pros and cons of not having a garbage collector?

#### Pros

+ Fast, predictable performance
+ Don't need to chase pointers around
+ Don't need to check which pointers can't be accessed

#### Cons

+ Can forget about, or lose, allocated memory
+ Mem leaks in kernel are bad because no swap for kernel
+ Double-freeing memory: freeing memory that was already freed
+ Dangling pointers: accessing deallocated/freed memory
+ Overrun: overrunning into different allocated regions can cause overwrites!

We can manually manage memory using 4 methods from `<stdlib.h>`:

+ <a id="clang-dynamicmem-malloc"></a> `malloc(size_t size) -> void *` allocates `size` bytes and returns pointer to allocated memory; memory is **not** initialized
	+ Passing in `0` returns either `NULL` or unique pointer value that can later be freed
+ <a id="clang-dynamicmem-calloc"></a> `calloc(size_t nmemb, size_t size) -> void *` allocates memory for an array of `nmemb` elements of `size` bytes each and returns pointer to allocated memory; memory is initialized to `0`
	+ Passing in `0` for anything acts like `malloc(0)`
+ <a id="clang-dynamicmem-realloc"></a> `realloc(void *ptr, size_t size) -> void *` changes size of memory block pointed to by `ptr` to `size` bytes
	+ `ptr` must have originated from `malloc`, `calloc`, or `realloc`
	+ Extra bytes allocated are **not** initialized
	+ If `ptr` is `NULL`, call is equivalent to `malloc(size)`
	+ If `size` is `0` and `ptr` is non-null, call is equivalent to `free(ptr)`
	+ If the area pointed to by `ptr` was moved, `free(ptr)` is done
	+ If we allocate 20 bytes but read in 10, shrinks to 10
	+ If we allocate 20 bytes but read in 30, grows to 30. Note that it may move memory location if it encounters buffer overrun
+ <a id="clang-dynamicmem-free"></a> `free(void *ptr)` frees memory space pointed to by `ptr`, which must have originated from `malloc`, `calloc`, or `realloc`.
	+ If `ptr` is `NULL`, nothing happens
	+ Be wary of double-freeing
	+ Be wary of dangling pointers (pointers freed but not nulled)

#### The DON'TS

+ Double free memory: null pointer after freeing it, that way if you free again nothing should happen
+ Freeing memory on stack: only free memory that was manually allocated
+ Freeing address in middle of block
	```c
	char *p = malloc(sizeof(char) * 5);
	p++;
	free(p);
	```
+ Dereferencing NULL, uninitialized, or dangling pointer
	```c
	char *p; /* unintialized pointer */
	printf("%s\n", c); /* prints garbage */
	```
	```c
	char *p = NULL; /* NULL pointer */
	printf("%s\n", c); /* seg fault */
	```
	```c
	char *p;
	{
		char c = 'f';
		p = &c; /* p is dangling pointer after leaving this scope */
	}
	printf("%s\n", p); /* prints garbage */

	p = malloc(sizeof(char));
	free(p);
	/* p is dangling pointer after freeing and not nulling */
	printf("%s\n", p); /* prints garbage */
	```

### <a id="clang-arrays"></a> Arrays

**_Arrays start at 0 (<a href="http://www.cs.utexas.edu/users/EWD/transcriptions/EWD08xx/EWD831.html" target="_blank">like all arrays should...</a>)_**

Arrays in C can be made in two ways: fixed arrays/initializers and pointers.

Fixed arrays are fixed in size, hence the name. `int arr[10]` will create an array of 10 integers. In C, however, the size of a fixed array must be determined during compile time. This means that you cannot use a variable size; only macro definitions and literals.

```c
#define SIZE 10

int x = 10;
int arr[x]; /* not allowed */

const int y = 10;
int arr2[y]; /* still not allowed b/c value y is a variable (even though it's a constant) */

int arr3[SIZE]; /* allowed, though the values are garbage */
int arr4[SIZE + 10]; /* allowed; values are garbage */
int arr5[SIZE + x]; /* not allowed */
int arr6[SIZE + y]; /* not allowed */
```

We can also use array initializers, though they have some caveats:

```c
/* arr[0] = 1, arr[1] = 2, so on... */
int arr[5] = { 1, 2, 3, 4, 5 };

/* all elements in arr2 init to 0 */
int arr2[5] = { 0 };

/* arr3[0] = 1, all other elements init to 0 */
int arr3[5] = { 1 };

/* error, excess elements in array initializer */
int arr4[5] = { 1, 2, 3, 4, 5, 6 };

/* error, empty initializer */
int arr5[5] = {};
```

Arrays can also be made through pointers:

```c
int arr[5];
int *ptr_arr = arr;
```

Why can we do this? Because whenever we refer to `arr` it *decays* to a pointer to the first element of `arr`. This results in an `int *`, which we see in `int *ptr_arr = arr`. When decayed to an `int *`, we no longer know what the size of the array is. Sure, we can do `sizeof(arr)` to get the total number of bytes in `arr`, which would be `5*sizeof(int)`, and then the length of `arr` by doing `sizeof(arr)/sizeof(int)`. What about `sizeof(ptr_arr)` then? That returns `sizeof(int *)`, or effectively the number of bytes a pointer takes up on your machine. There's no way to get the length of the array `ptr_arr` at runtime; we have to have it stored somewhere for later use.

But we can take advantage of this! We can access the elements in the array using pointer arithmetic OR array subscripts: `arr[i] == *(arr + i)`. These are the same, but it is conventional to stick to the notation in which the array was declared:


```c
int arr[5]; /* use arr[i] */
int *ptr = arr; /* use *(arr + i) */
```

Now let's spice it up by referencing an array's address.

```c
int arr[5];
int (*ptr_to_arr)[5] = &arr;
```

When we do `&arr`, we are referring to a pointer with the type `int (*)[5]`, or a pointer to an array of 5 integers. This is *not* the same as `int *` because decaying has not occurred. Here are some interesting things about this:

```c
sizeof(ptr_to_arr); /* returns size of a pointer, nothing new */
sizeof(*ptr_to_arr); /* returns size of int[5], or 5*sizeof(int) */

/* say ptr_to_arr's address is 0x10
   from our understanding of pointer arithmetic, ptr_to_arr + 1
   will advance 1 * sizeof(*ptr_to_arr) bytes -> sizeof(int[5])
   which is also 5*sizeof(int) bytes.
   taking sizeof(int) to be 4, ptr_to_arr + 1 will be the address
   0x24
 */
printf("%x\n", ptr_to_arr + 1); /* prints 0x24 */
```

Another important note is that we can't reassign an array. If we could, that could mean the array ends up having more elements than we initially allocated it to have:

```c
int arr[3];
int *ptr_arr = malloc(sizeof(int) * 5);
arr = ptr_arr;
/* if this was ok, then arr would have 5 elements and have a buffer overrun */
```

#### <a id="clang-arrays-multidim"></a> Multi Dimensional Arrays

Multi dimensional arrays in C are just a one-dimensional array whose elements are arrays (much like in Java) and are stored in [row-major](https://en.wikipedia.org/wiki/Row-major_order) order.

```c
int matrix[5][5]; /* familiar 2d array declaration*/
int **matrix2 = malloc((sizeof(int) * 5) * 5); /* pointer style */
int r, c;

for (r = 0; r < 5; r++) {
  for (c = 0; c < 5; c++) {
    /* familiar array subscripting */
    printf("matrix[%d][%d]=%d\n", r, c, matrix[r][c]);
    /* pointer arithmetic */
    printf("*(*(matrix2+%d)+%d)=%d\n", r, c, *(*(matrix2+r)+c));
  }
}
```

Again, follow proper convention when accessing the elements.

### <a id="clang-string"></a> Strings

+ A string in C is really a `char` array.
	+ `char[constant size]`
	+ `char *`
+ `char` is always 1 byte long.
+ String constants, or literals, are surrounded in double quotes (`"`). Declared as `const char *`
+ Know the escape sequences
+ There is no concatenation operator, you have to manipulate strings yourself! Consider using format strings!
+ All string literals have at least one byte. The last byte of every string literal is known as the null terminator/character (a `char` with a value of `0x0`). Because of this, the literal `"hello"` is actually 6 bytes long, not 5.
	+ `sizeof("hello")` returns 6
+ If you have want a `char *` to be at most 10 characters long, what you really mean is at most 10 characters long *plus* the null character. So what you need to do is allocate space for 11 characters (the last character should be the null character).
+ If string is not null terminated, then C keeps on printing characters until it finds one (so that gets pretty weird).
	+ We can null terminate strings ourselves, so do not be afraid to take advantage of it!
+ <a id="clang-string-strlen"></a> `strlen(const char *)`
	+ Returns length of string (does not include null character)
+ <a id="clang-string-strcpy"></a> `strcpy(char *dest, const char *src)` and `strncpy(..., size_t n)`
	+ Copies (at most first `n` bytes) string pointed by `src` to `dest`.
	+ `dest` must be large enough, otherwise buffer overrun!
	+ Null character from `src` is copied if it has it (or is within first `n` bytes)
+ <a id="clang-string-strcmp"></a> `strcmp(const char *a, const char *b)` and `strncmp(..., size_t n)`
	+ Compares (at most first `n` bytes of) two strings; returns negative if less, positive if greater, or 0 if equal to.
+ <a id="clang-string-strcat"></a> `strcat(char *dest, const char *src)` and `strncat(..., size_t n)`
	+ Appends (at most `n` bytes from) `src` to `dest`, overwriting null terminating byte at end of `dest` and then adds a null terminator.
	+ `dest` must be large enough, otherwise undefined behavior!
		+ \>= `strlen(dest) + strlen(src) + 1` buffer capacity with `strcat`
		+ \>= `strlen(dest) + n + 1` buffer capacity with `strncat`
+ <a id="clang-string-memcpy"></a> `memcpy(void *dest, const void *src, size_t n)`
	+ Copies `n` bytes from `src` into `dest`
	+ Their memory areas shouldn't overrlap
+ <a id="clang-string-memmove"></a> `memmove(void *dest, const void *src, size_t n)`
	+ Same as `memcpy`, but memory areas *can* overlap

```c
const char *a = "hello";
const char *b = "hello";
printf("%d\n", a == b); /* prints 1 or true */
a[0] = 'f'; /* not allowed because a is a pointer to a constant char */
*a = 'f'; /* not allowed, same issue */
```

### <a id="clang-io"></a> I/O

Read up on format strings for both <a href="http://www.cplusplus.com/reference/cstdio/scanf/" target="_blank">input</a> and <a href="http://www.cplusplus.com/reference/cstdio/printf/" target="_blank">output</a>. We don't need to know every single format specifier and flag.

Here are some common input/output functions found in `stdio.h`:

+ `printf(const char *format, ...)` prints a format string to stdout; returns number of characters written
+ `scanf(const char *format, ...)` reads in a format string from stdin; returns number of items in arguments list that were successfully filled/read in
	+ If `scanf` encounters an invalid input (say it was expecting a number but got a character/letter), then it stops reading.
	```c
	int a = 200;
	char c = 'f';
	scanf("%d %c", &a, &c); /* say we input "a b" (w/o quotes) */
	printf("%d %c\n", a, c); /* prints "200 f" */
	```
+ `fopen(const char *filename, const char *mode)` opens up a file in a certain mode

|Mode|Description|
|:-:|:-|
|`r`|read|
|`w`|overwrite (existing contents discarded; creates file if DNE)|
|`a`|append (creates file if DNE)|
|`r+`|read/update (opens for update both input and output)
|`w+`|overwrite/update (existing contents discarded; creates file if DNE)
|`a+`|append/update (creates file if DNE)

+ `fdopen(int fd, const char *mode)` same as `fopen` but takes in a file descriptor instead of a path
+ `fread(void *ptr, size_t size, size_t count, FILE *stream)` reads `count` elements (each `size` bytes long) from `stream` and stores into `ptr`
+ `fwrite(const void *ptr, size_t size, size_t count, FILE *stream)` same as `fread` but writes to `stream` from `ptr`
+ `fgets(char *str, int num, FILE *stream)` reads at most `num` characters (or until `EOF`) into `str` from `stream`; adds null character
+ `fputs(const char *s, FILE *stream)` writes `s` to `stream` w/o the null character from `s`
+ `fscanf(FILE *stream, const char *format, ...)` like `scanf` but with a file
+ `fprintf(FILE *stream, const char *format, ...)` like `printf` but with a file
+ `fclose(FILE *stream)` closes file stream; returns 0 if successful, `EOF` ("end-of-file") if failure
+ `fflush(FILE *stream)` forces any buffered data in `stream` to be written
+ `sscanf(const char *str, const char *format, ...)` like `scanf` but with a string

Buffered I/O can be very efficient because we don't have to constantly make calls to the file system. Instead, we can do it in short bursts/intervals. Take copying a sentence for example. With no "buffering", we would read one character, write it, and repeat until we're done. With buffering, we would read in, say, a few words, write them down, and keep on repeating.

### <a id="clang-enum"></a> Enumerations

+ Represents values across a series of named constants
+ Each constant is of type `int`

```c
/* A=0, B=1, C=2 */
enum { A, B, C } enum1; /* enum1 will be a variable of this enumeration */
/* D=1, E=2, F=3 */
enum letters { D = 1, E, F }; /* enumerated type of "letters" */
/* G=1, H=2, I=5 */
enum { G = 1, H, I = 5} enum3;
enum letters enum2; /* enum2 is of enumerated type "letters" */

enum1 = A; /* enum1 is effectively 1 */
enum1 = D; /* enum1 becomes 1, or B, because D=B=1 */
enum1 = E; /* enum1 becomes 2, or C, because E=C=2 */
enum3 = A; /* enum3 is 0, can't be any constant from the enumeration it was defined from */
enum3 = C; /* enum3 is 2, or H */
enum2 = E; /* enum2 is 2, or E */

/* using enums as variables */
enum boolean { false, true };
enum boolean works = true;
printf("Works? %d\n", works);
```

### <a id="clang-struct"></a> Structures

A structure holds a collection of variables of differing types under one name. Say you want to store information about a person: their name and age. We can use a `char [81]` (name of 80 chars + null character) and `int` respectively, like so:

```c
struct Person {
  char name[81];
  int age;
} my_person; /* my_person is a struct variable of type Person */

/* vishal is also a struct variable of type Person */
struct Person vishal; /* all of vishal's fields will have garbage values */
```

It's important to make sure the members of a structure have been properly initialized before using them, especially pointers!

```c
/* Continuing from above example */
/* Can initialize members by using dot (.) operator */
strcpy(vishal.name, "Vishal Patel");
vishal.age = 19;

/* Assume for the below code that we can declare variables after code statements (even though our compiler flags don't let us) */

/* Can't initialize the members individually by using the dot (.) operator
   and the name of the member because ANSI forbids this
 */
struct Person vishal_bad = { .name = "Vishal Patel", .age = 19 };

/* Can initialize members in order by giving appropriately-typed values.
   This is more concise, but isn't flexible because it relies on the
   ordering of the definition of the struct. What if I swapped the lines
   of age and name?
 */
struct Person vishal_quick = { "Vishal Patel", 19 };

/* Can initialize a few members, the rest will be zero-ed. */
struct Person vishal_short = { "Vishal Patel" }; /* age is 0 */

/* Can't do this, empty initializer */
struct Person vishal_short_2 = { };

/* Can do this to zero everything */
struct Person vishal_short_3 = { 0 };
```

Two ways of accessing the members of a structure:

+ Member operator (`.`)
+ Structure pointer operator (`->`)

```c
struct Person vishal_good = { "Vishal Patel", 19 };
struct Person *vishal_ptr = &vishal;

printf("%d\n", vishal_good.age); /* 19 */
printf("%s\n", vishal_quick.name); /* Vishal Patel */

printf("%d\n", vishal_ptr->age); /* 19 */
printf("%d\n", (*vishal_ptr).age); /* 19 */
printf("%d\n", (&vishal_good)->age); /* 19 */

vishal_good->age = 21; /* setting my age to 21 */

printf("%d\n", vishal_ptr->age); /* 21 */
printf("%d\n", vishal_good.age); /* 21 */
```

We can use `typedef` to name the structure as a whole (think of this as abbreviating a really long structure name):

```c
typedef struct Very_Long_Name {
  int a;
  int b;
  int c;
} VLN;

VLN var_1 = { 0 }, var_2 = { 0, 0, 500 };

printf("%d\n", var_1.a); /* 0 */
printf("%d\n", var_2.c); /* 500 */
```

Structures can be nested within structures:

```c
struct Class {
  struct Teacher {
    int age;
  } teacher;

  int size;
};

struct Class class = { { 30 }, 20 };
struct Class class_2 = { 0 };
printf("%d\n", class.teacher.age); /* 30 */
printf("%d\n", class.size); /* 20 */
printf("%d\n", class_2.teacher.age); /* 0 */
printf("%d\n", class_2.size); /* 0 */
```

Another example of nested structures:

```c
struct Teacher {
  int age;
};

struct Class {
  struct Teacher teacher;
  int size;
};

struct Class class = { 0 };
struct Teacher teacher = { 50 };
struct Class class_2 = { 0 };

/* Note that we can't declare class_2 as
      class_2 = { teacher, 10 }
   because teacher can't be computed at load time
 */

class.teacher.age = 30;
class.size = 15;
class_2.teacher = teacher;
class_2.size = 10;

printf("%d\n", class.teacher.age); /* 30 */
printf("%d\n", class.size); /* 15 */
printf("%d\n", class_2.teacher.age); /* 50 */
printf("%d\n", class_2.size); /* 10 */
```

Structures can also container pointers!

```c
typedef struct WithPointer {
  int *ptr;
  char *str;
} WP;

WP wp_1;

/* ok b/c we're not dereferencing */
printf("%p\n", (void *) wp_1.ptr); /* prints some unpredictable address */
/* don't... */
printf("%d\n", *wp_1.ptr);

/* now let's initialize */
wp_1.ptr = malloc(sizeof(int));
wp_1.str = calloc(sizeof(char) * 11);
*wp_1.ptr = 20;
*wp_1.str = 'H';
*(wp_1.str + 1) = 'E';

printf("%d\n", *wp_1.ptr); /* 20 */
printf("%s\n", wp_1.str); /* HE */
```

We can also take advantage of dynamic memory allocation when dealing with structures! As shown above, we allocated some space to the pointer members of `WithPointer`. We can take this one step further and allocate space to a struct pointer itself!

```c
struct WP *wp_ptr, *wp_list;
int list_size = 0;

wp_ptr = (WP*) malloc(sizeof(WP));

scanf("%d", &list_size);
wp_list = (WP*) malloc(list_size * sizeof(WP));
```

Assigning one structure to another is a shallow copy. Say we assign `a` to `b`, both of the same struct type. If we change anything in `b`, it is not reflected in `a`, and vice-versa:

```c
struct A {
  int i;
  int *ptr;
}

struct A a = { 0 }, b = { 0 };
int x = 100, y = 200;

a.i = 10;
b.i = 20;
a.ptr = &x;
b.ptr = &y;

a = b;

/* a.i == b.i is true */
/* a.ptr == b.ptr is true */
/* &a == &b is false */
/* &a.i == &b.i is false */
/* &a.ptr == &b.ptr is false */

b.i = 30; /* a.i is still 20 */
b.ptr = &x; /* b.ptr points to x, but a.ptr still points to y */
```

### <a id="clang-union"></a> Unions

Unions are similar to structures, but have one big difference: how much space is allocated to a union. The size of a structure will be the sum of the size of all it's members. The size of a union, however, will be the largest size of all it's members. Here's an example:

```c
union Union {
  char name[20];
  int age;
  int age2;
} u;

struct Structure {
  char name[20];
  int age;
  int age2;
} p;

/* both are essentially same thing logically */
printf("%d\n", sizeof(u)); /* 20 */
printf("%d\n", sizeof(p)); /* 28 */
```

This happens because unions are only meant to access one of its members at a time, whereas structures can access any of its members at any time. Hence why the size of the union here is 20 bytes, or the largest size among all its members (the size of `name`):

```c
/* u.name becomes HELLO */
strcpy(u.name, "HELLO");
```

This doesn't mean we can't do `u.age`. But if we do that, it'll be a bit weird, predictable, but weird. Say an `int` is 4 bytes long. This means that `u.age` and `u.age2` are interpreted as the first four bytes of `HELLO`, or essentially they're simply `HELL`. In this scenario, they end up representing the decimal integer `1280066888`, or `4c4c4548` in hex.

So in a general sense, unions hold data that is meant to be used by only one field. All other fields share those same bytes in that data. So when you access those other fields, C interprets those bytes for whatever the data types for those fields are. Just keep in mind, C is not changing those bytes. It's simply just saying "I'm meant to hold `x` amount of bytes, and I'll interpret them however my fields want me to."

### <a id="clang-func"></a> Functions

Functions in C are compiled linearly. That means you can't call a function if you haven't defined it before that line.

```c
int main() {
  foo(1); /* not allowed! compiler doesn't know what foo is! */
}

void foo(int a) { ... }
```

A workaround this is placing the defintion of `foo` before `main`

```c
void foo(int a) { ... }

int main() {
  foo(1); /* all good! */
}
```

Another alternative is to use *function prototypes*. These tell the compiler "Hey, I'm going to define this function later on, so don't panic if I make any calls or references to this function!"

```c
void foo(int a);

int main() {
  foo(); /* this is ok! */
}

void foo(int b) { ... }
```

With prototypes, the compiler will spit out errors if you make a call to the function but it hasn't been defined yet.

A general structure for a function prototype is as follows

```c
return_type function_name(type1 arg1, type2 arg2, ...);
```

+ `typeN` is the type of the argument.
+ `argN` is the name of the argument; this is totally optional in the prototype and does not have to match with your definition (as shown above)

Remember that `static` and `extern` are really important when it comes to functions and variables, take advantage of them!

Parameters for a function are ***always*** passed by value, never by reference. If you want to modify the reference of the argument, use pointers:

```c
void foo(int a) {
  a += 2;
}

void bar(int *a) {
  *a += 2;
}

int main() {
  int x = 10;
  foo(x); /* x is still 10 */
  bar(x); /* x is now 12 */

  return 0;
}
```

There's a small caveat when it comes to function parameters:

```c
void foo() { ... }
foo(); /* ok! */
foo(20); /* ok! */
```

and

```c
void foo(void) { ... }
foo(); /* ok! */
foo(20); /* not so ok... */
```

are different functions.
The former can take an arbitrary amount of arguments, while the latter will take 0 arguments.

#### <a id="#clang-func-ptr"></a> Function Pointers

Function pointers are simply just pointers to a function:

```c
int add(int a, int b) {
  return a + b;
}

/* not a prototype */
int (*func_ptr)(int, int);
func_ptr = &add;
/* we could've also done int (*func_ptr)(int, int) = &add; */

assert((*func_ptr)(2, 3) == add(2, 3)); /* true */
```

Generalized, a function pointer declaration would look like this:

```c
return_type (*name)(type1, type2, ...);
```

Function pointers allow us to pass in functions as parameters.

```c
int use_func_ptr(int (*func_ptr)(int, int), int a, int b) {
  return (*func_ptr)(a, b);
}

int add(int a, int b) { return a + b; }

int (*ptr)(int, int) = &add;

printf("%d\n", use_func_ptr(ptr, 2, 3)); /* 5 */
```

We can also use `typedef` with function pointers to shorten some of those declarations.

```c
typedef int (*two_int_func)(int, int);

int use_func_ptr(two_int_func func, int a, int b) {
  return (*func)(a, b);
}

int add(int a, int b) { return a + b; }

two_int_func add_func = &add;

printf("%d\n", use_func_ptr(add_func, 2, 3)); /* 5 */
```

As noted when discussing `void *`, function pointers cannot be cast to a `void *`:

```c
void foo() {
  printf("I'm foo!\n");
}

/* Don't get confused, pointing to a void function is perfectly fine.
   Casting any function pointer to void *, however, isn't. */
void (*foo_func_ptr)() = &foo;

printf("%p\n", (void *) foo_func_ptr); /* NOT ALLOWED */
```

Function pointers are useful when sorting a list. Take, for example, a function that needs to sort a list of arbitrary objects. These objects could be integers, strings, or even some random struct that no sane person would think of designing. A generalized sorting function (say one that implements the selection sort algorithm) can't predict every kind of object it needs to be able to sort (hence why its *generalized*). It should only need to worry about determining when an object is greater than, less than, or equal to another object. So in order to generalize this sorting function, it takes in a function pointer that acts as the comparator.

### <a id="clang-stdio"></a> Standard I/O

We have three standard I/O streams:

+ `stdin` is standard input stream
+ `stdout` is standard output stream
+ `stderr` is standard error stream

All of the streams are of the type `FILE *`, so they can be used in various other functions.

Buffering is an important thing to note for these standard I/O streams:

+ `stdout` is line-buffered *when pointed to a terminal*. This means some lines may not appear until `fflush` or `exit` is called.
+ `stderr` is not buffered, so flushing won't really have an effect here

### <a id="clang-exit"></a> `exit`

`exit(int status)` (from `stdlib.h`) causes the process to terminate, however it performs some calls before it does (like flushing buffered streams). Eventually, it calls `_exit(int)`

`_exit(int status)` (from `unistd.h`) on the other hand skips all that stuff and goes straight to terminating the process.

### <a id="clang-err"></a> `err`

`err(int eval, const char *fmt, ...)` (from `err.h`) displays a formatted error message on `stderr`, and then exits with a provided value.

### <a id="clang-perror"></a> `perror`

`perror(const char *s)` (from `stdio.h`) prints a message to `stderr`. So this is similar to `err(int)`, but it doesn't exit.

### <a id="clang-linkage"></a> Linkage

Recall:

+ A symbol is a named variable of function.
+ The use of `extern` and `static` scoping.

Linking takes symbols that are referencing in one object file/library, and resolves them with a symbol defined in another object file/library. `static` and `extern` matters when you combine multiple `.c` files.

Linkers, the programs responsible for linking, take machine code files as input and produces an executable object code

+ Machine code, or object, files usually end in `.o`
+ Exectable object code end in `a.out` by default from `gcc` on Linux (on Windows it's `a.exe`), but the name of the executable file can be changed by using the `-o` flag.

For functions and global variables, `static` will indicate that the symbol should not be exported to other files. `extern` is the opposite in that someone can find that symbol elsewhere (if it were declared) or the symbol will be exported elsewhere (if it's being defined).

### <a id="clang-cla"></a> Command Line Arguments

Command line arguments (CLAs) are arguments that you pass in through the command line. These are specified in the `main` function that we typically write. However, we don't need to define `main` to take CLAs if we don't care about them. This is simplified to `int main() { ... }`.

If we wanted to incorporate CLAs into our program, we add two parameters to our `main` function:

```c
int main(int argc, char *argv[]) { ... }
```

+ `argc` is the number of arguments that were passed in via the command line. `argc` will always be greater than or equal to 1 (explained in `argv`).
+ `argv` is the value of the argument that were passed in via the command line. `argv` will always contain `a.out` (or whatever your executable file was called) as it's first element, and then the rest of your arguments follow. Hence why `argc >= 1`.
	+ This can allow us to figure out from which file the program was executed from
	+ `argv` can also be a `char **`

### <a id="clang-recursion"></a> Recursion

There isn't anything unique or special to recursion in C. But there are some properties of C that you can take advantage of:

+ Local `static` variables can be useful in recursive functions as it can remove the need for a helper function:
	```c
	/* poor implementation of a recursive factorial function */
	int fac(int i) {
	  static int prod = 1;

	  if (i > 1) {
        prod *= i;
	  }

	  fac(i - 1);

	  return prod;
	}

	printf("%d\n", fac(4)); /* 24 */
	```
+ C supports tail recursion. That means if your recursive call is at the end of a function, C won't allocate a new stack frame for the call; it'll just reuse the current one. This makes it more memory efficient:
	```c
	void tail_recurse(int i) {
      if (i > 0) {
        printf("%d\n", i);
        tail_recurse(i - 1);
      }
	}

	tail_recurse(5); /* prints 5 4 3 2 1 */

	/* You can use gdb (look at the debuggers for more info) to verify
	   that the same stack frame is used for each recursive call made for
	   tail_recurse(int) */
	```

## <a id="linked-lists"></a> Linked Lists

Linked lists can get a bit trickier in C. Because they're variable in size, we have to rely on pointers and memory allocation. Creating nodes isn't terribly difficult so long as you properly allocate memory and change the fields in the node struct.

Removing a single node requires a bit more effort on your side. You need to have a previous and current node pair. The current node is the one you want to remove, where the previous node is the node that links to the current node. Before removing current, make sure you set previous' next link to current's next, and then remove current. When removing current, you need to:

+ Null the node's next link
+ Free all the data associated with the node
+ Free the node itself

As for doubly linked lists, make sure you update the links properly for the next and previous links.

Clearing a list is where it can get annoying. Recursively clearing the list isn't a bad idea, but it's how we get rid of the nodes is the complication. We need to make sure we free them as we remove them, but you also can't free nodes that you're going to need after you bubble up your recursion. Here's the trick:

+ Recurse towards the end of the list
+ Null the node's next link
+ Free all the data associated with the node
+ Free the node itself
+ Bubble back up

That's with singly linked lists though. Doubly linked lists require a bit more work. We can start with either side of the list since it's doubly linked, but let's stick with recursing to the end:

+ Null both the node's next and previous links
+ Free the data associated with the node
+ Free the node itself
+ Bubble back up

Make sure you null any "head" or "tail" variables you have in your linked list struct.

## <a id="avr"></a> AVR Assembly

### <a id="avr-registers"></a> Registers

+ A register is one byte long (8 bits)
+ *words* are two registers next to each other (i.e. `r1:0` or `r25:24`)
+ There are 32 registers in AVR assembly, from `r0` to `r31`
+ `r1` is known as the zero register (because `r0` wouldn't make any sense to be the zero register)

### <a id="avr-ptr-registers"></a> Pointer Registers

+ Special register pairs (so special that they have their own short names)
	+ `r27:r26 -> X`
	+ `r29:r28 -> Y`
	+ `r31:r30 -> Z`
	+ Can reference higher byte of the pair by using `H` and lower using `L`
		+ i.e. `ldi YH, hi(256)` (stores hi 8 bits of 256 into higher byte of `Y`, or effectively into `r29`)
+ `ld` read access for pointer registers, `st` write access for pointer registers
+ `X` simply reads/writes from the address `X`, pointer doesn't change
	+ `ld r1, x` or `st x, r1`
+ `X+` reads/writes from/to address `X` then increments pointer forward by one
	+ `ld r1, x+` or `st x+, r1`
+ `-X` first decrement pointer by one then read/write from/to new address
	+ `ld r1, -x` or `st -x, r1`
+ Note: you can use `Y` or `Z` in place of `X`

### <a id="avr-funcs"></a> Functions

+ Calling functions via `call` creates a new stack frame
	+ Do NOT confuse this with jumping to labels.
+ When calling and writing functions, be wary of callee-save and caller-save registers:
	+ Caller-save `r18-r27;r30-r31`: saved by whoever called this function, so this function can freely change these
	+ Callee-save `r2-r17; r28-r29`: saved by the function that's called, so the caller should expect these to have to the same values after the function returns
+ Be wary of `r0` and `r1`:
	+ `r0` is a garbage like register, so anything can be in there. Don't expect any function to be considerate of what you put in there. With actual reasoning, when multiplying two registers, the product is stored in `r1:0`. This is because the product of two 8-bit numbers can be 16-bits. These registers are meant to be messed around with (well not so much `r1`, but more on that later), so it's safer for the program to store the product in here.
	+ `r1` is the zero register. So if this gets modified in any way whatsoever, you need to make sure you clear this register before leaving your subroutine. Everyone's assuming that `r1` will be 0 when they work with it. So if you multiply, remember to clear `r1` after you're done with the product.
+ Parameters are passed via `r25` to `r8` (left to right)

	```c
	uint8_t function(uint8_t i, uint8_t j);
	/* i would be in `r25:r24` (w/8-bit value in `r24`) */
	/* j would be in `r23:r22` (w/8-bit value in `r22`) */
	```

	+ If we have many parameters, then they're pushed onto the stack

+ Return values are stored in registers `r25:18` depending on how big the return value is
	+ 8-bit: `r24`
	+ 16-bit: `r24-r25`
	+ 32-bit: `r22-r25`
	+ 64-bit: `r18-r25`

### <a id="avr-sram"></a> SRAM

+ Static RAM (hence **S**RAM)
+ Useful for pushing variables to stack
	+ Contents of a register (caller-save registers in particular)
	+ Return address prior or after a subroutine
+ Useful for recursion

### <a id="avr-recursion"></a> Recursion

+ Before calling a recursive function, save any registers you will need to use (especially caller-save ones)  after the recursion finishes.
+ Make sure you write your base cases properly
	+ There's no less than or equal to, there's only less than
	+ No greater than, only greater than or equal to

### <a id="avr-instructions"></a> Instructions

Some common instructions used in my class (instructions are not case-sensitive). The descriptions aren't provided, but you can learn more about them <a href="https://www.microchip.com/webdoc/avrassembler/avrassembler.wb_instruction_list.html" target="_blank">here.</a>

+ `rd` refers to destination register
+ `rr` refers to a source register
+ `k` is a constant
+ `C` is the carry flag/bit

|Instruction|Syntax|Operation|Registers|Notes|
|:-:|:-|:-|:-|:-|
|`INC`|`inc rd`|`rd += 1`|ALL||
|`DEC`|`dec rd`|`rd -= 1`|ALL||
|`CLR`|`clr rd`|`rd = 0`|ALL||
|`JMP`|`jmp label`||||
|`CALL`|`call subroutine`||||
|`RET`|`ret`||||
|`PUSH`|`push rr`||ALL||
|`POP`|`pop rd`||ALL||
|`ADD`|`add rd, rr`|`rd += rr`|ALL||
|`ADC`|`adc rd, rr`|`rd += rr + C`|ALL||
|`ADIW`|`adiw rd, k`|`rd += k`|24,26,28,30|even registers|
|`SUB`|`sub rd, rr`|`rd -= rr`|ALL||
|`SBC`|`sbc rd, rr`|`rd -= rr - C`|ALL||
|`SUBI`|`subi rd, k`|`rd -= k`|16-31||
|`SBCI`|`sbci rd, k`|`rd -= k - C`|16-31||
|`SBIW`|`sbiw rd, k`|`rd -= k`|24,26,28,30|even registers|
|`MUL`|`mul rd, rr`|`r1:0 = rd * rr`|ALL|unsigned|
|`MULS`|`muls rd, rr`|`r1:0 = rd * rr`|16-31|signed|
|`LSL`|`lsl rd`|`rd <<= 1`|ALL|unsigned, no rotation|
|`LSR`|`lsr rd`|`rd >>= 1`|ALL|unsigned, no rotation|
|`ROL`|`rol rd`|`rd <<= 1`|ALL|unsigned, rotation|
|`ROR`|`ror rd`|`rd >>= 1`|ALL|unsigned, rotation|
|`ARS`|`asr rd`|`rd >>= 1`|ALL|signed, no rotation|
|`CP`|`cp rd, rr`|`rd - rr`|ALL||
|`CPC`|`cpc rd, rr`|`rd - rr - C`|ALL||
|`CPI`|`cpi rd, k`|`rd - k`|ALL||
|`BREQ`|`breq label`||||
|`BRNE`|`brne label`||||
|`BRGE`|`brge label`|||signed|
|`BRSH`|`brsh label`|||unsigned
|`BRLT`|`brlt label`|||signed|
|`BRLO`|`brlo label`|||unsigned|
|`MOV`|`mov rd, rr`|`rd = rr`|ALL||
|`MOVW`|`movw rd, rr`|`rd+1:rd = rr+1:rr`|ALL|even registers|
|`LDI`|`ldi rd, k`|`rd = k`|16-31||
|`LD`|`ld rd, -(XYZ)+`|`rd = --*(XYZ)++`|X,Y,Z|pre-dec, none, post-inc|
|`ST`|`st -(XYZ)+, rr`|`--*(XYZ)++ = rr`|X,Y,Z|pre-dec, none, post-inc|

## <a id="memmap"></a> Memory Maps

Don't really know how to explain memory maps, so uh, just refer to <a href="http://www.cs.umd.edu/class/spring2018/cmsc216/exams/exam1/MemoryMapExample.pdf" target="_blank">this example.</a>

Tips:

+ Be wary of pointers and what they point to (to be honest would just triple check them).
+ Parameters are passed in to functions by value, not reference.
+ If you have an uninitialized value, indicate that it's garbage.
+ Start drawing your memory map from the very first executed statement and update values/add variables as you step through the code.
+ Indicate when you have `NULL` values either with `NULL` or the ground symbol.
+ Take up all the space you need; the more space you have for your memory map, the more cleaner your map can look.

## <a id="endian"></a> Big Endian vs. Little Endian

The difference is what pattern the bytes are stored in memory. **Big Endian (BE)** means that the **most significant byte (MSB)** is stored first. **Little Endian (LE)** is the opposite: when the **least significant byte (LSB)** is stored first. Say we want to represent `1234` in hex, which is `0x04D2` (we're storing this in 2 bytes). In BE, we would store `0xD2` and then `0x04`, resulting in `0x04D2` (just like we would read it in real life). In LE, we'd do the opposite. That's because the MSB in `1234` is `0x04`, and the LSB is `0xD2`, if we were to read the memory in order we'd get `0xD204`.

## <a id="bitwiseops"></a> Bitwise Operators

|Operator|Name|Type|Description|Example|
|:-:|:-:|:-:|:-|:-|
|&|AND|binary|true if both true|`3 AND 2 = 2`|
|&#124;|OR|binary|true if one true|`3 OR 2 = 3`|
|^|XOR|binary|true if one true and one false|`3 XOR 2 = 1`|
|~|NOT|unary|true if false|`NOT 3 = 0` (assuming 2-bit number)|
|>>|right shift|binary|shifts a number `n` bits to the right (divides and truncates by 2<sup>`n`</sup>)|`9 RSHIFT 3 = 1`|
|<<|left shift|binary|shifts a number `n` bits to the left (multiplies by 2<sup>`n`</sup>)|`9 LSHIFT 3 = 72`|

**Note:** Do not confuse the bitwise operators for the boolean operators!

Hexadecimal can easily be converted to binary:

```text
0xfa
-> find binary of 0xf -> 1111
-> find binary of 0xa -> 1010
-> concatenate results -> 1111_1010
```

This intuition applies to any number base that's a power of 2!

```text
octal example
0173 -> 123
-> find binary of 01 -> 001
-> find binary of 07 -> 111
-> find binary of 03 -> 011
-> concatenate results -> 001_111_011
```

### <a id="bitwiseops-complements"></a> One's Two's Complement

One's Complement simply involves flipping all the bits, or performing the `NOT` bitwise operation on a number.

+ i.e. 1101 -> 0010

Two's Complement is just taking the One's Complement of a number and adding 1 to the result

+ i.e. 1101 -> 0010 + 1 -> 0010

Two's Complement is significant when it comes to signed numbers. A question you may be asked is

```text
What is the two's complement of -5 of a 4-bit number?
```

The approach is to

+ Find the binary representation of `5` for a 4-bit number, which is `0101`.
+ Flip it -> `1010`.
+ Add 1 -> `1011`.
+ And you're done!

This approach applies to basically any negative integer of an `n`-bit number.

## <a id="debug"></a> Debuggers

### <a id="debug-gdb"></a> gdb

+ Usage: `gdb a.out`
+ C debugger
+ Remember to compile with `-g` flag
+ Common commands:
	+ `q` exits
	+ `l [line number]` lists code (around the specified line number)
	+ `r` runs program
		+ Can redirect input: `r < public01.in`
	+ `b <line number | function name>` sets breakpoint at a line number or on a function call
		+ gdb is useless if you do not set a breakpoint...
	+ `clear [line number]` clears breakpoint at line number in current stack frame
	+ `p <expression>` prints an expression (very useful with memory addresses)
	+ `n <number of steps>` executes (the next `number of steps`) steps/statements/calls
	+ `s` steps into a function/call
	+ `c` continues to next breakpoint or termination (normal or abnormal)
	+ `where` see the stack frame
	+ `frame <frame number>` changes stack frame
	+ `backtrace [count]` prints backtrace of all stack frames, or innermost `count` frames
		+ If `count` is negative, prints outermost `count` frames
	+ `up [count]` selects and prints the stack frame that called the current stack frame
		+ `count` indicates how many frames up to go
+ Stuck in an infinite loop? `Ctrl+C` to send a `SIGINT` to the program. gdb will trap the signal and stop execution

### <a id="debug-valgrind"></a> valgrind

+ Usage: `valgrind [opts] ./a.out`
+ Memory checker
+ Helps us locate problems in our code
+ Can be used to detect invalid memory operations
+ Need to compile with `-g` flag
+ A nice option to use is `--track-origins=yes`. Helps determine root cause of those pesky "Conditional jump or move depends on uninitialised value(s)" messages...'
+ `--leak-check=full` is also a great option, gives us more detail about memory leaks
+ We can do `valgrind ./a.out < public01.in > results` which would act like `./a.out < public01.in > results` but with valgrind

### <a id="debug-splint"></a> splint

+ Usage: `splint file.c`
+ Tool for statically checking C programs
+ Can help us detect uninitialized variables, empty conditional/loop bodies, or even wrong return types.

## <a id="ee"></a> Electrical Engineering

### <a id="ee-resistors"></a> Resistors

+ Resistors
	+ Resists/limits flow of electrons through a circuit
	+ Measured in **ohms**
	+ Series: sum of resistors
		+ R<sub>total</sub> = R<sub>1</sub> + R<sub>2</sub> + ... + R<sub>n</sub>
	+ Parallel: inverse of sum of inverses
		+ R<sub>total</sub> = (R<sub>1</sub><sup>-1</sup> + R<sub>2</sub><sup>-1</sup> + ... + R<sub>n</sub><sup>-1</sup>)<sup>-1</sup>
	+ Ohm's Law: `V = IR`
		+ `V` voltage (volts)
		+ `I` current (amperes)
	+ Resistors dissipate power in the form of heat: `P = IV`
		+ `P` power (watts)
		+ `I` current

### <a id="ee-voltdiv"></a> Voltage Dividers

+ A type of resistor circuit that turns big voltages into smaller ones
+ Uses two resistors in series:

	![Diagram of a voltage divider](../../misc/ee_volt_div.png)
+ V<sub>out</sub> = V<sub>in</sub> * R<sub>2</sub> / (R<sub>1</sub> + R<sub>2</sub>)
+ V<sub>out</sub> is the smaller voltage

### <a id="ee-kirchhofflaws"></a> Kirchhoff's Laws

+ Kirchhof's Current Law (KCL): sum of currents flowing into a circuit element is equal to the sum of currents flowing out of that element
+ Kirchhof's Voltage Law (KVL): the sum of the voltage difference across all circuit elements (including source) is 0

## <a id="refs"></a> References

+ Dr. Neil Spring's Lecture Notes
+ Dr. Nelson Padua-Perez's Lecture Slides
+ [C Syntax - Wikipedia](https://en.wikipedia.org/wiki/C_syntax)
+ [C Programming - Programiz](https://www.programiz.com/c-programming/)
+ [Discussion about array sizes - Reddit](https://www.reddit.com/r/programming/comments/1scchh/the_difference_between_arr_and_arr_how_to_find/cdwjgup/)
+ [Resistors - Sparkfun](https://learn.sparkfun.com/tutorials/resistors)
+ [Voltage Dividers - Sparkfun](https://learn.sparkfun.com/tutorials/voltage-dividers)
