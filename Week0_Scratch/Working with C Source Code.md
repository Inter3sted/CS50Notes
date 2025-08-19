# Working with C Source Code

## File Naming

- C source code files must use the `.c` extension (e.g., `hello.c`).

## Compilation

- Compile C code using:
  ```bash
  make hello
  This translates the source code into an executable (machine code).
  ```

Execution
Run the compiled program with:

bash
Copy
Edit
./hello
This executes the machine code and displays its output.

Output
At the end, you will have two files:

The source code: hello.c

The executable: hello

"Hello, World" Example
The snippet:

c
Copy
Edit
printf("hello, world /n");
demonstrates the use of printf from stdio.h to print formatted console output.

Key Points
printf: Standard C function for formatted output.

"hello, world /n": String literal.

"hello, world" is displayed text.

\n is the escape sequence for newline. (The backslash \ introduces escape sequences in C.)

Complete Program
c
Copy
Edit
#include <stdio.h>

int main(void) {
printf("hello, world\n");
return 0;
}
This example shows:

Standard library functions

String literals and escape sequences

Basic program structure

Console output

Libraries in C
A library is a collection of pre-written code you can use.

stdio.h is the standard I/O library, not "studio."

Functions available in stdio.h include:
printf() and scanf() for console I/O

fprintf() and fscanf() for file I/O

fopen() and fclose() for file management

fgets() and puts() for string handling

Without #include <stdio.h>, using these functions would cause compilation errors.

Interactive Example with CS50
c
Copy
Edit
#include <cs50.h>
#include <stdio.h>

int main(void)
{
string answer = get_string("What's your name? ");
printf("hello, %s\n", answer);
}
This program:

Uses cs50.h for the get_string function.

Uses stdio.h for printf.

Demonstrates I/O, variables, data types, functions, and libraries.

Conditionals in C
c
Copy
Edit
if (x < y) {
printf("x is less than y\n");
}

if (x < y) {
printf("x is not less than y\n");
} else {
}

if (x < y) {
printf("x is less than y\n");
} else if (x > y) {
printf("x is greater than y\n");
} else {
printf("x is equal to y\n");
}
Counters in C
c
Copy
Edit
int counter = 0;

counter = counter + 1;
counter += 1;
counter++;
// For decrement:
counter--;
Logical Operators
&& → logical AND

|| → logical OR
