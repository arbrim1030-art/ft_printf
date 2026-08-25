*This project has been created as part of the 42 curriculum by asulejma.*

# Description

## ft_printf

`ft_printf` is a project from the 42 curriculum. The goal is to recreate the behavior of the standard C `printf()` function.

The function implemented is:

```c
int ft_printf(const char *format, ...);
```

It must analyze the format string and print the corresponding arguments depending on the conversion specified.

The mandatory conversions are:

* `%c` — prints a character
* `%s` — prints a string
* `%p` — prints a pointer address in hexadecimal
* `%d` — prints a decimal integer
* `%i` — prints an integer
* `%u` — prints an unsigned decimal integer
* `%x` — prints a hexadecimal number in lowercase
* `%X` — prints a hexadecimal number in uppercase
* `%%` — prints a `%`

The function also returns the total number of characters printed.

The project focuses on variadic functions, string manipulation, number conversion, recursion and formatted output.

# Instructions

## Compilation

The project can be compiled using the provided `Makefile`:

```bash
make
```

This creates the `libftprintf.a` static library.

Other available commands are:

```bash
make clean
make fclean
make re
```

* `make clean` removes object files.
* `make fclean` removes object files and the library.
* `make re` recompiles the entire project.

## Usage

To use `ft_printf`, include the project header in your program:

```c
#include "ft_printf.h"
```

Then compile your program together with the library:

```bash
cc main.c -L. -lftprintf -o test
```

Example:

```c
#include "ft_printf.h"

int main(void)
{
    ft_printf("Hello %s! Number: %d\n", "world", 42);
    return (0);
}
```

# Algorithm

## Format String Parsing

The main algorithm processes the format string from left to right.

For every character:

1. If the character is not `%`, it is printed directly.
2. If `%` is encountered, the next character is checked.
3. The conversion character determines which function should handle the corresponding argument.
4. The argument is retrieved using `va_arg()`.
5. The appropriate printing function outputs the argument.
6. The number of printed characters is added to the total.
7. The parser continues until the end of the format string.

For example:

```text
ft_printf("Hello %s, you are %d years old\n", "John", 42);
```

The parser reads:

```text
Hello 
```

and prints it directly.

When `%s` is encountered, it retrieves the next argument as a string and prints it.

When `%d` is encountered, it retrieves the next argument as an integer and converts it to decimal before printing it.

## Conversion Handling

Each conversion is handled separately.

For example:

```text
%c  → character
%s  → string
%d  → signed decimal number
%u  → unsigned decimal number
%x  → hexadecimal lowercase
%X  → hexadecimal uppercase
%p  → pointer address
```

This keeps the main parsing function simple and separates the different printing behaviors.

## Number Conversion

Numbers are converted without using the standard `printf()` function.

For decimal numbers, the implementation separates the digits using division and modulo operations.

For example, the number `123` can be processed as:

```text
123 % 10 = 3
123 / 10 = 12

12 % 10 = 2
12 / 10 = 1

1 % 10 = 1
```

The digits can then be printed in the correct order.

The same principle is used for hexadecimal numbers, using a base of 16 instead of 10.

For hexadecimal output, the following characters are used:

```text
0123456789abcdef
```

or:

```text
0123456789ABCDEF
```

depending on whether `%x` or `%X` is used.

## Variadic Arguments

The project uses the `<stdarg.h>` library to handle a variable number of arguments.

The following macros are used:

* `va_list` — stores information about the arguments.
* `va_start` — initializes the argument list.
* `va_arg` — retrieves the next argument.
* `va_end` — cleans up the argument list.

The format string determines which type should be retrieved from the argument list.

# Data Structure

The main data structure used by the project is `va_list`.

It allows the function to access an unknown number of arguments passed after the format string.

The project does not require a complex data structure such as a linked list or tree. The format string itself acts as the main source of information used to determine how the arguments should be processed.

The conversion functions operate directly on the retrieved values, which keeps the implementation lightweight and avoids unnecessary memory allocation.

# Why This Algorithm and Data Structure?

A sequential parser is well suited to `printf()` because the format string is naturally processed from left to right.

Using a separate function for each conversion makes the code easier to understand, test and maintain. It also avoids having one large function containing all the conversion logic.

Using `va_list` is necessary because the number and types of arguments are determined at runtime by the format string.

This approach also avoids creating unnecessary data structures and allows each argument to be processed as soon as its corresponding conversion is encountered.

# Complexity

Let `n` be the length of the format string.

The format string is processed once from beginning to end, giving a parsing complexity of approximately:

```text
O(n)
```

The complexity of printing an individual number depends on the number of digits it contains.

The additional memory used by the algorithm is minimal and does not depend on the total number of arguments. Apart from the `va_list` and temporary variables, the implementation does not require a large additional data structure.

# Resources

## Documentation

* `man 3 printf` — reference for the standard `printf()` function.
* `man 3 stdarg` — documentation related to variadic functions.
* `man 2 write` — documentation for the `write()` system call.
* 42 `ft_printf` project subject.
* C documentation about integer and hexadecimal number representation.

## AI Usage

AI was used during the development of the project as a learning, debugging and documentation tool.

It was used for:

* Understanding how variadic functions work with `va_list`, `va_start`, `va_arg` and `va_end`.
* Understanding the behavior and requirements of the different `printf` conversions.
* Helping design the format-string parsing algorithm.
* Helping debug conversion functions and identify possible errors.
* Reviewing code structure and improving the separation between the different conversion functions.
* Checking potential edge cases and improving the project's tests.
* Helping write and organize this README.

AI was not used to blindly generate the final project. The code was implemented, tested and reviewed by asulejma to ensure that its behavior and logic were understood.
