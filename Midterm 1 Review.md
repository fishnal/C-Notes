## Contents
+ [Unix Commands](#unix)
	+ [cp](#unix-cp)
	+ [ls](#unix-ls)
	+ [cd](#unix-cd)
	+ [pwd](#unix-pwd)
	+ [rm](#unix-rm)
	+ [mv](#unix-mv)
	+ [diff](#unix-diff)
	+ [gcc](#unix-gcc)
+ [C Specifics](#clang)
	+ [General Info](#clang-info)
	+ [Data Types](#clang-datatypes)
	+ [Operators](#clang-ops)
	+ [Preprocessor](#clang-preproc)
		+ [#include](#clang-prepoc-include)
		+ [#define](#clang-prepoc-define)
		+ [#ifdef](#clang-prepoc-ifdef)
	+ [Keywords](#clang-kws)
	+ [Expresions](#clang-expr)
	+ [Conditional Expressions](#clang-condtls)
	+ [Loops](#clang-loops)
	+ [Pointers](#clang-ptrs)
		+ [Pointer Arithmetic](#clang-ptrs-arithmetic)
	+ [Dynamic Memory Allocation](#clang-dynamicmem)
		+ [Stack](#clang-dynamicmem-stack)
		+ [Heap](#clang-dynamicmem-heap)
	+ [Arrays](#clang-arrays)
		+ [Multi Dimensional Arrays](#clang-arrays-multidim)
	+ [Strings](#clang-string)
	+ [I/O](#clang-io)
	+ [enum](#clang-enum)
	+ [Structures](#clang-struct)
	+ [Unions](#clang-union)
	+ [Functions](#clang-func)
		+ [Function Pointers](#clang-func-ptr)
	+ [Linkage](#clang-linkage)
	+ [Command Line Arguments](#clang-cla)
	+ [Recursion](#clang-recursion)
+ [Memory Maps](#memmap)
+ [Bitwise Operators](#bitwiseops)
	+ [One's and Two's Complement](#bitwiseops-complements)
+ [Makefiles](#makefile)
+ [Debuggers](#debug)
	+ [gdb](#debug-gdb)
	+ [valgrind](#debug-valgrind)
	+ [splint](#debug-splint)
+ [Electrical Engineering](#ee)
	+ [Basics](#ee-basics)
	+ [Voltage Dividers](#ee-voltdiv)
	+ [Diodes](#ee-diodes)
	+ [Kirchhoff's Laws](#ee-kirchhofflaws)
+ [References](#refs)

## Unix Commands <a id="unix"></a>
+ `cp` <a id="unix-cp"></a> copies files and directories
	+ Usage: `cp [opts] <SRC> <DIR>`
	+ `-r|-R|--recursive` copies directories recursively
+ `ls` <a id="unix-ls"></a> lists directory contents
	+ Usage: `ls [opts] [FILE]` lists info about file
	+ `-a` includes ALL hidden files (including `.` and `..`)
	+ `-A` includes MOST hidden files (excludes `.` and `..`)
	+ `-d|--directory` lists directories but not their contents
	+ `-R|--recursive` lists subdirectories recursively
+ `cd` <a id="unix-cd"></a> changes current working directory
	+ `.` will change to current working directory (no real effect)
	+ `..` will go up one level (if it can)
+ `pwd` <a id="unix-pwd"></a> prints name of current working directory
+ `rm` <a id="unix-rm"></a> removes files or directories
	+ Usage: `rm [opts] <FILE>`
	+ `-r|-R|--recursive` removes directories and their contents recursively
	+ `-d|--dir` removes empty directories
+ `mv` <a id="unix-mv"></a> moves/renames files
	+ Usage: `mv <SRC> <DEST>` renames `SRC` to `DEST`
	+ Usage: `mv <SRC> <DIR>` moves `SRC` to `DIR`
	+ ***how do you move a file up a directory***
+ `diff` <a id="unix-diff"></a> compares files line by line
	+ Usage: `diff [opts] <FILE1/DIR1> <FILE2/DIR1>`
	+ `-bwi` ignores whitespaces, but not empty lines
	+ `-Bwi` ignores whitespaces and empty lines
+ `gcc` <a id="unix-gcc"></a> compiles C/C++ files
	+ Usage: `gcc [opts] <FILES>`
	+ `-c` compiles but does not link (output file ends with `.o`)
	+ `-o <FILE>` places output file in specified file
	+ `-g` compiles with debugging symbols

## C Specifics <a id="clang"></a>
### General Info <a id="clang-info"></a>
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
### Data Types <a id="clang-datatypes"></a>
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
### Operators <a id="clang-ops"></a>
+ `+`, `-`, `*`, `/`, `%`
+ `=`, `+=`, `-=`, `*=`, `/=`, `%=`
+ `++`, `--`
+ `(void *)` cast to another type (e.g. void pointer)
+ `==`, `!=`, `<`, `<=`, `>`, `>=`, `&&`, `||`
+ `()` parentheses for order of operations
+ Assignment operator evaluates to the `rvalue` (see the [end of pointers](#clang-ptrs))
### Preprocessor <a id="clang-preproc"></a>
`#include` and `#define` are both preprocessor directives. Directives end up being a "copy and paste" of whatever they are into wherever they are mentioned.
+ Both of these directives can only span one line
#### #include <a id="clang-preproc-include"></a>
+ Includes a header file and will copy the text of that header file into the current file
+ i.e. `#include <stdio.h>` copies text of `stdio.h` into the file
+ Angled brackets indicate file should be searched in the standard compiler paths
+ Quotes indicates the search should be exapnded to include current source directory (note that it also includes std. compiler paths)
#### #define <a id="clang-preproc-define"></a>
+ Defines object-like or function-like macros
+ By convention, defined symbols are in ALL CAPS
+ Object syntax: `#define <NAME> <value>`
+ Function syntax: `#define <NAME>(<parameters>) <value/body of function>`
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
#### #ifdef <a id="clang-preproc-ifdef"></a>
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
### Keywords <a id="clang-kws"></a>
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
+ `static` localizes a symbol; value of a static variable persists until end of program
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
### Expressions <a id="clang-expr"></a>
There is no boolean data type in C, instead numbers are used. `0` is false, and any other integer is true, including negatives.

### Conditional statements <a id="clang-condtls"></a>
Standard conditional statements:
+ `if`
+ `else if`
+ `else`
+ ternary operators
+ `switch` (same format as Java switch statements)
+ I guess `#ifdef-#endif` are conditional statements as well but for preprocessing?
### Loops <a id="clang-loops"></a>
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
### Pointers <a id="clang-ptrs"></a>
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
#### Pointer Arithmetic <a id="clang-ptrs-arithmetic"></a>
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
+ `rvalue` refers to data value stored some memory address; can't have value assigned to it, which means can only appear on right side of assignment operator (think of right-value)
+ `lvalue` means locator-value and refers to expressions that locate/designate objects; can appear on left or right side of assignment operator (`=`) (think of left-value)
### Dynamic Memory Allocation <a id="clang-dynamicmem"></a>
There are two things we need to understand before we engage in dynamic memory: the *stack* and *heap*.
#### Stack <a id="clang-dynamicmem-stack"></a>
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
#### Heap <a id="clang-dynamicmem-heap"></a>
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
+ `malloc(size_t size) -> void *` allocates `size` bytes and returns pointer to allocated memory; memory is **not** initialized
	+ Passing in `0` returns either `NULL` or unique pointer value that can later be freed
+ `calloc(size_t nmemb, size_t size) -> void *` allocates memory for an array of `nmemb` elements of `size` bytes each and returns pointer to allocated memory; memory is initialized to `0`
	+ Passing in `0` for anything acts like `malloc(0)`
+ `free(void *ptr)` frees memory space pointed to by `ptr`, which must have originated from `malloc`, `calloc`, or `realloc`.
	+ If `ptr` is `NULL`, nothing happens
	+ Be wary of double-freeing
	+ Be wary of dangling pointers (pointers freed but not nulled)
+ `realloc(void *ptr, size_t size)` changes size of memory block pointed to by `ptr` to `size` bytes
	+ `ptr` must have originated from `malloc`, `calloc`, or `realloc`
	+ Extra bytes allocated are **not** initialized
	+ If `ptr` is `NULL`, call is equivalent to `malloc(size)`
	+ If `size` is `0` and `ptr` is non-null, call is equivalent to `free(ptr)`
	+ If the area pointed to by `ptr` was moved, `free(ptr)` is done
	+ If we allocate 20 bytes but read in 10, shrinks to 10
	+ If we allocate 20 bytes but read in 30, grows to 30. Note that it may move memory location if it encounters buffer overrun
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
### Arrays <a id="clang-arrays"></a>
***Arrays start at 0 ([like all arrays should...](http://www.cs.utexas.edu/users/EWD/transcriptions/EWD08xx/EWD831.html))***

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
#### Multi Dimensional Arrays <a id="clang-arrays-multidim"></a>
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
### Strings <a id="clang-string"></a>
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
+ `strlen(const char *)`
	+ Returns length of string (does not include null character)
+ `strcpy(const char *dest, const char *src)` and `strncpy(..., size_t n)`
	+ Copies (at most first `n` bytes) string pointed by `src` to `dest`.
	+ `dest` must be large enough, otherwise buffer overrun!
	+ Null character from `src` is copied if it has it (or is within first `n` bytes)
+ `strcmp(const char *a, const char *b)` and `strncmp(..., size_t n)`
	+ Compares (at most first `n` bytes of) two strings; returns negative if less, positive if greater, or 0 if equal to.
+ `strcat(const char *dest, const char *src)` and `strncat(..., size_t n)`
	+ Appends (at most `n` bytes from) `src` to `dest`, overwriting null terminating byte at end of `dest` and then adds a null terminator.
	+ `dest` must be large enough, otherwise undefined behavior!
		+ \>= `strlen(dest) + strlen(src) + 1` buffer capacity with `strcat`
		+ \>= `strlen(dest) + n + 1` buffer capacity with `strncat`
```c
const char *a = "hello";
const char *b = "hello";
printf("%d\n", a == b); /* prints 1 or true */
a[0] = 'f'; /* not allowed because a is a pointer to a constant char */
*a = 'f'; /* not allowed, same issue */
```
### I/O <a id="clang-io"></a>
Read up on format strings for both [input](http://www.cplusplus.com/reference/cstdio/scanf/) and [output](http://www.cplusplus.com/reference/cstdio/printf/). We don't need to know every single format specifier and flag.

Here are some common input/output functions found in `stdio.h`:
+ `printf(const char *format, ...)` prints a format string to stdout; returns number of characters written
+ `scanf(const char *format, ...)` reads in a format string from stdin; returns number of items in arguments list that were successfully filled/read in
+ `fopen(const char *filename, const char *mode)` opens up a file in a certain mode

  |Mode|Description|
  |:-:|:-|
  |`r`|read|
  |`w`|overwrite (existing contents discarded; creates file if DNE)|
  |`a`|append (creates file if DNE)|
  |`r+`|read/update (opens for update both input and output)
  |`w+`|overwrite/update (existing contents discarded; creates file if DNE)
  |`a+`|append/update (creates file if DNE)

+ `fread(void *ptr, size_t size, size_t count, FILE *stream)` reads `count` elements (each `size` bytes long) from `stream` and stores into `ptr`
+  `fwrite(void *ptr, size_t size, size_t count, FILE *stream)` same as `fread` but writes to `stream` from `ptr`
+ `fgets(char *str, int num, FILE *stream)` reads at most `num` characters (or until `EOF`) into `str` from `stream`; adds null character
+ `fputs(const char *s, FILE *stream)` writes to `s` to `stream` w/o the null character from `s`
+ `fscanf(FILE *stream, const char *format, ...)` like `scanf` but with a file
+ `fprintf(FILE *stream, const char *format, ...)` like `printf` but with a file
+ `fclose(FILE *stream)` closes file stream; returns 0 if successful, `EOF` if failure
+ `fflush(FILE *stream)` forces any buffered data in `stream` to be written
+ `sscanf(const char *str, const char *format, ...)` llike `scanf` but with a string

Buffered I/O can be very efficient because we don't have to constantly make calls to the file system. Instead, we can do it short bursts/intervals. Take copying a sentence for example. With no "buffering", we would read one character, write it, and repeat until we're done. With buffering, we would read in, say, a few words, write them down, and keep on repeating.
### enum <a id="clang-enum"></a>
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
enum1 = D; /* enum1 becomes 1, or A, because D=A=1 */
enum1 = E; /* enum1 becomes 2, or B, because E=B=2 */
enum3 = A; /* enum3 is 0, can't be any constant from the enumeration it was defined from */
enum3 = C; /* enum3 is 2, or H */
enum2 = E; /* enum2 is 2 */

/* using enums as variables */
enum boolean { false, true };
enum boolean works = true;
printf("Works? %d\n", works);
```
### Structures <a id="clang-struct"></a>
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

/* Can do this though to zero everything */
struct Person vishal_short_3 = { 0 };
```
Two ways of accessing the members of a structure:
+ Member operator (`.`)
+ Structure pointer operator (`->`)
```c
printf("%d\n", vishal_good.age); /* 19 */
printf("%s\n", vishal_quick.name); /* Vishal Patel */

struct Pointer *vishal_ptr = &vishal_good;
printf("%d\n", vishal_ptr->age); /* 19 */
printf("%d\n", (*vishal_ptr).age); /* 19 */
printf("%d\n", (&vishal_good)->age); /* 19 */
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
wp_1.str = malloc(sizeof(char) * 11);
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
### Unions <a id="clang-union"></a>
Unions are similar to structures, but have one big difference: the space allocated to both. The size of a structure will be the sum of the size of all it's members. The size of a union, however, will be the largest size of all it's members. Here's an example:
```c
union UnionStructure {
	char name[20];
	int age;
} us;

struct PersonStructure {
	char name[20];
	int age;
} ps;

/* both are essentially same thing logically */
printf("%d\n", sizeof(us)); /* 20 */
printf("%d\n", sizeof(ps)); /* 24 */
```
This happens because unions can access only one of its members at a time; the rest will hold garbage values. This contrasts with structures, which can access all of its members at any given time.
```c
strcpy(us.name, "HELLO");
printf("%s\n", us.name); /* HELLO */
printf("%d\n", us.age); /* garbage */
us.age = 10;
printf("%s\n", us.name); /* garbage characters */
printf("%d\n", us.age); /* 10 */
```
### Functions <a id="clang-func"></a>
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
Function pointers also exist, which are simply just pointers to a function:
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
Function pointers are useful when sorting a list. Take, for example, a function that needs to sort a list of arbitrary objects. These objects could be integers, strings, or even some random struct that no sane person would think of designing. A generalized sorting function (say one that implements the selection sort algorithm) can't predict every kind of object it needs to be able to sort (hence why its *generalized*). It should only need to worry about determining when an object is greater than, less than, or equal to another object. So in order to generalize this sorting function, it takes in a function pointer that acts as the comparator.
### Linkage <a id="clang-linkage"></a>
### Command Line Arguments <a id="clang-cla"></a>
Command line arguments are arguments that you pass in through the command line.
### Recursion <a id="clang-recursion"></a>
## Memory Maps <a id="memmap"></a>
## Bitwise Operators <a id="bitwiseops"></a>
|Operator|Name|Type|Description|Example|
|:-:|:-:|:-:|:-|:-|
|`&`|AND|binary|true if both true|`3&2 = 2`|
|`\|`|OR|binary|true if one true|`3\|2 = 3`|
|`^`|XOR|binary|true if one true and one false|`3^2 = 1`|
|`~`|NOT|unary|true if false|`~3 = 0` (assuming 2-bit number)|
|`>>`|right shift|binary|shifts a number `n` bits to the right (divides and truncates by 2<sup>`n`</sup>)|`9>>3 = 1`|
|`<<`|left shift|binary|shifts a number `n` bits to the left (multiplies by 2<sup>`n`</sup>)|`9<<3 = 72`|

Do not confuse the bitwise operators for the boolean operators!
Hexadecimal can easily be converted to binary:
```
0xfa
-> find binary of 0xf -> 1111
-> find binary of 0xa -> 1010
-> concatenate results -> 1111_1010
```
This intuition applies to any number base that's a power of 2!
```
octal example
0173 -> 123
-> find binary of 01 -> 001
-> find binary of 07 -> 111
-> find binary of 03 -> 011
-> concatenate results -> 001_111_011
```
### One's Two's Complement <a id="bitwiseops-complements"></a>
One's Complement simply involves flipping all the bits, or performing the `NOT` bitwise operation on a number.
+ i.e. 1101 -> 0010

Two's Complement is just taking the One's Complement of a number and adding 1 to the result
+ i.e. 1101 -> 0010 + 1 -> 0010

Two's Complement is significant when it comes to signed numbers.
## Makefiles <a id="makefile"></a>
## Debuggers <a id="debug"></a>
### gdb <a id="debug-gdb"></a>
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
### valgrind <a id="debug-valgrind"></a>
+ Usage: `valgrind [opts] ./a.out`
+ Memory checker
+ Helps us locate problems in our code
+ Can be used to detect invalid memory operations
+ Need to compile with `-g` flag
+ A nice option to use is `--track-origins=yes`. Helps determine root cause of those pesky "Conditional jump or move depends on uninitialised value(s)" messages...'
+ We can do `valgrind ./a.out < public01.in > results` which would act like `./a.out < public01.in > results` but with valgrind
### splint <a id="debug-splint"></a>
+ Usage: `splint file.c`
+ Tool for statically checking C programs
+ Can help us detect uninitialized variables, empty conditional/loop bodies, or even wrong return types.
## Electrical Engineering <a id="ee"></a>
### Basics <a id="ee-basics"></a>
### Voltage Dividers <a id="ee-voltdiv"></a>
### Diodes <a id="ee-diodes"></a>
### Kirchhoff's Laws <a id="ee-kirchhofflaws"></a>
## References
+ Dr. Neil Spring's Lecture Notes
+ [C Syntax - Wikipedia](https://en.wikipedia.org/wiki/C_syntax)
+ [C Programming - Programiz](https://www.programiz.com/c-programming/)
+ [Discussion about array sizes - Reddit](https://www.reddit.com/r/programming/comments/1scchh/the_difference_between_arr_and_arr_how_to_find/cdwjgup/)