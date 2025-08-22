````markdown
# notes_meow.c

> A beginner-friendly, fully commented C program that prints "meow" a number of times. It includes explanations of function prototypes, loops, user input with CS50, and why `do...while` is used for input validation.

---

## How to Compile & Run

**Prerequisite:** Assumes the [CS50 Library](https://cs50.readthedocs.io/library/c/) for C is installed.

**Compile:**
```bash
clang -std=c11 -Wall -Wextra -Werror notes_meow.c -lcs50 -o notes_meow
````

**Run:**

```bash
./notes_meow
```

-----

## Key Concepts 💡

  * **Function Prototypes**: Declarations that tell the compiler about a function's name, return type, and parameters *before* it's used. This allows you to define functions after `main()` or in other files.

      * **Example:** `void meow(int n);`
      * **Meaning:** "There is a function named `meow` that takes one `int` argument and returns nothing (`void`)."

  * **`main()`**: The program's required entry point—execution always begins here.

  * **Loops**:

      * **`for` loop**: `for (int i = 0; i < n; i++) { ... }` — Best for iterating a known number of times.
      * **`while` loop**: `while (condition) { ... }` — Repeats as long as a condition is true; may not run at all.
      * **`do...while` loop**: `do { ... } while (condition);` — Always runs the body **at least once**, then checks the condition to see if it should repeat.

  * **CS50 `get_int()`**: A helpful function from `<cs50.h>` that prompts the user with a string and safely returns an integer.

-----

## Input Validation with `do...while`

We use a `do...while` loop to get user input because we must **prompt the user at least once** *before* we can check if their input is valid.

  * A `do...while` loop runs the code in the `do { ... }` block first, and *then* checks the `while (condition)`. This is perfect for input validation.
  * A regular `while (condition) { ... }` loop checks the condition first. This is a problem if the variable hasn't been given a value yet (it holds a "garbage" value), leading to unpredictable behavior.

### ⚠️ Memory Safety: The Danger of Uninitialized Variables

Reading a variable before you've assigned it a value is **undefined behavior** in C. The program might crash, or it might *seem* to work sometimes and fail unexpectedly at others.

**Bad Example (Undefined Behavior):**

```c
int n; // n is uninitialized (holds a random "garbage" value)

// This checks the garbage value of n BEFORE we get input from the user!
while (n < 1)
{
    n = get_int("Number: ");
}
```

**Good Example (Correct & Safe):**

```c
int n; // n is uninitialized, but that's okay here.

do
{
    // We get input and assign a value to n FIRST.
    n = get_int("Number: ");
}
// THEN we check the condition. The value of n is now defined and predictable.
while (n < 1);
```

-----

## Execution Flow 🐾

```text
      +--------------------+
      |   Program starts   |
      +---------+----------+
                |
                v
      +--------------------+
      |       main()       |
      +---------+----------+
                |
                v
+-------------------------------+
| do...while: prompt "Number:"  |
| store result in int n         |
+---------------+---------------+
                |
      Is n < 1 ?+---- yes -----> loop back and prompt again
                |
               no
                v
         call meow(n)
                |
                v
          +--------------+
          | meow(int n): |
          |  loop i=0..n |
          |  print       |
          +--------------+
                |
                v
         return to main()
                |
                v
          +--------------+
          |     end      |
          +--------------+
```

-----

## Final Code: `notes_meow.c`

```c
#include <cs50.h>   // Allows get_int()
#include <stdio.h>  // Allows printf()

/*
 * FUNCTION PROTOTYPE:
 * Tell the compiler that a function named `meow` exists below,
 * takes an int parameter `n`, and returns nothing (void).
 */
void meow(int n);

/* MAIN: Program entry point */
int main(void)
{
    // We need a positive integer from the user.
    int n;

    // do...while guarantees we prompt at least once.
    // We keep asking until n >= 1.
    do
    {
        n = get_int("Number: "); // Example input: 3
    }
    while (n < 1); // If user typed 0 or negative, ask again.

    // Now print "meow" exactly n times.
    meow(n);

    return 0; // Return 0 signals success.
}

/*
 * FUNCTION DEFINITION:
 * Prints "meow\n" exactly n times using a for loop.
 */
void meow(int n)
{
    // Start i at 0; continue while i < n; increment i each loop.
    for (int i = 0; i < n; i++)
    {
        printf("meow\n");
    }
}
```

-----

## Appendix: Alternate Implementations

These examples are for learning and show different ways to achieve the same result (e.g., printing "meow" 3 times).

#### Method 1: Manual `printf`

Repetitive and not scalable, but it works for a fixed number.

```c
printf("meow\n");
printf("meow\n");
printf("meow\n");
```

#### Method 2: `while` loop (count down)

```c
int i = 3;
while (i > 0)
{
    printf("meow\n");
    i--;
}
```

#### Method 3: `for` loop (classic)

This is the most common and idiomatic way to loop a specific number of times in C.

```c
for (int i = 0; i < 3; i++)
{
    printf("meow\n");
}
```

#### Earlier Version A: Function with no parameters

The `main` function is responsible for looping and calls a simple `meow` function multiple times.

```c
// prototype
void meow(void);

int main(void)
{
    for (int i = 0; i < 3; i++)
    {
        meow();
    }
    return 0;
}

void meow(void)
{
    printf("meow\n");
}
```

#### Earlier Version B: Function with a parameter

`main` passes the desired count to the function, making the `meow` function more reusable and powerful. This is the design used in the final program.

```c
// prototype
void meow(int n);

int main(void)
{
    meow(3);
    return 0;
}

void meow(int n)
{
    for (int i = 0; i < n; i++)
    {
        printf("meow\n");
    }
}
```

```
# Data Types and Variables
### `int`
- Stores integers.
- Always takes **4 bytes (32 bits)**.
- Range: `-2^31` to `2^31 - 1`.
### `unsigned int`
- Qualifier that disallows negative values.
- Doubles the positive range compared to `int`.
- Occasionally used in CS50.
### `char`
- Stores **single characters**.
- Always **1 byte (8 bits)**.
- Range: `-128` to `127`.
- Uses **ASCII mapping** to represent characters like `A`, `B`, `C`.

### `float`
- Stores **floating-point values (real numbers)**.
- **4 bytes (32 bits)**.
- Limited precision.

### `double`
- Stores **floating-point values** with **double precision**.
- **8 bytes (64 bits)**.
- Allows more precise real numbers than `float`.

### `void`
- Not a data type, but a **type specifier**.
- Functions can have:
  - `void` return type → return nothing.
  - `void` parameter list → takes no parameters.
- Think of it as "nothing."

### `bool`
- Stores **Boolean values**: `true` or `false`.
- Requires `#include <cs50.h>`.

### `string`
- Stores words, sentences, and text.
- Requires `#include <cs50.h>`.

---

## Creating Variables

    - To create, specify data type of variables and give a name
    int number;
    char letter;
    - if wish to create multiple of same type, specify name once, then list as many as you want
    int height, width;
    float sqrt2, sqrt3, pi;
    - In general, good practice to declare variables when needed.
Using variables
    - After variable **declared**, it's no longer necessary to specify variables type. (In fact, doing that leads to unintended consequences)
    int number; // declaration
    number = 17; // assignment
    char letter; // declaration
    letter = 'H'; // assignment
    - if declaring and setting value of a variable (sometimes called **initializing**), you can consolidate to one step.
    int number = 17; // initialization
    char letter = 'H'; // initialization

## Arithmetic Operators
    - In C we add (+), subtract (-), multiply (*) and divide (/) numbers.
    int x = y + 1;
    x = x * 5;
    - We also have modulus operator, (%) which gives us remainder when number on left of operator is divided by number on right.
    int m = 13 % 4; // m is now 1
    - C provides shorthand way to apply arithmetic operator.
    x = x * 5;
    x *= 5;
    - This works with all five basic arithmetic operators. C provides further shorthand for increments or decrements of a variable by 1:
    x++;
    x--;
## Boolean Expressions
    - Boolean expressions are for comparing values in C
    - All expressions in C are either True or False
    - Result of Boolean expression in other progreamming constructs such as deciding which branch in a **conditional** to take, or determining whether a **loop** should continue to run.
    - Sometimes when using Bool expressions we use variables of type bool, but we don't have to.
    - In C, every nonzero value is true, and zero is false.
    Two main types of Bool expressions: **logical operators** an **relational operators**.
- Logical operators
    - Logical AND (&&) is true if and only if both operands are true, otherwise false
    x true y true (x&&y) true, x true y false (x&&y) false
- Logical Operators
    - Logical OR (||) is true if and only if at least one operand is true, otherwise false. x true y true (x||y) true, x true y false (x||y) true, x false y false (x||y) false.
    - Logical NOT (!) inverts the value of its operand. x true !x false, x false !x true.
- Relational operators
    - These behave as you would expect them to, and appear syntactically similar to how you may recall them from elementary arithmetic.
        - Less than (x < y)
        - Less than or equal to (x <= y)
        - Greater than (x > y)
        - Greater than or equal to (x >= y)
    - C also tests two variables for equality or inequality
        - Equality (x == y)
        - Inequality (x != y)
    - It's a common mistake to use the assignment operator (=) when you intend to use the equality operator (==).
# Conditionals
    - Conditional expressions allow your programs to make decisions and take different forks in the road, depending on the values of variables or user input.
    - C provides a few different ways to implement conditional expressions (also known as branches) in your programs, some of which likely look familiar from Scratch.
if (boolean-expression) { 

} 
 - If the boolean-expression evaluates to true, all lines of code between the curly braces will execute in order from top-to-bottom.
 - If the boolean-expression evaluates to false, those lines of code will not execute.
if (boolean-expression) { 

} 
else { 

}
- If the boolean-expression evaluates to true, all lines of code between the first set of curly braces will execute in order from top-to-bottom.
- If the boolean-expression evaluates to false, all lines of code between the second set of curly braces will execute in order from top-to-bottom.

if (boolean-expr1) 
{ 
    // first branch 
} 
else if (boolean-expr2) 
{ 
    // second branch 
} else if (boolean-expr3) 
{ 
    // third branch 
} 
else 
{ 
    // fourth branch
 }
 - In C, it is possible to create an if–else if-else chain.
     - In Scratch, this required nesting blocks.
 - As you would expect, each branch is mutually exclusive. 
if (boolean-expr1) 
{ 
    // first branch   
} 
if (boolean-expr2) 
{ 
    // second branch 
} 
if (boolean-expr3) 
{ 
    // third branch 
} 
else 
{ 
    // fourth branch
}
- It is also possible to create a chain of non-mutually exclusive branches. 
- In this example, only the third and fourth branches are mutually exclusive. The else binds to the nearest if only.

int x = GetInt();
switch(x)
{
     case 1: printf(“One!\n”); 
     break; 
     case 2: printf(“Two!\n”); 
     break; 
     case 3: printf(“Three!\n”); 
     break; 
     default: printf(“Sorry!\n”);
}
- C’s switch() statement is a conditional statement that permits enumeration of discrete cases, instead of relying on Boolean expressions.
- It’s important to break; between each case, or you will “fall through” each case (unless that is desired behavior).

int x = GetInt(); 
switch(x) 
{ 
    case 5: printf(“Five, ”); 
    case 4: printf(“Four, ”); 
    case 3: printf(“Three, ”); 
    case 2: printf(“Two, ”); 
    case 1: printf(“One, ”); 
    default: printf(“Blastoff!\n”); 
}
- C’s switch() statement is a conditional statement that permits enumeration of discrete cases, instead of relying on Boolean expressions.
- It’s important to break; between each case, or you will “fall through” each case (unless that is desired behavior)

int x; 
if (expr) 
{ 
    x = 5;  
    } 
else 
{ 
    x = 6; 
}

and

int x = (expr) ? 5 : 6;
- These two snippets of code act identically.
- The ternary operator (?:) is mostly a cute trick, but is useful for writing trivially short conditional branches. Be familiar with it, but know that you won’t need to write it if you don’t want to.

if (and if-else, and if-else if-…-else)
 - Use Boolean expressions to make decisions.
Switch
    - Use discrete cases to make decisions.
?:
    - Use to replace a very simple if-else to make your code look fancy.
# Loops
    - Loops allow your programs to execute lines of code repeatedly, saving you from needing to copy and paste or otherwise repeat lines of code. 
    - C provides a few different ways to implement loops in your programs, some of which likely look familiar from Scratch.
    while (true)
    {

    }
        - This is what we call an infinite loop. The lines of code between the curly braces will execute repeatedly from top to bottom, until and unless we break out of it (as with a break; statement) or otherwise kill our program.
    while (boolean-expr)
    {

    }
    - If the boolean-expr evaluates to true, all lines of code between the curly braces will execute repeatedly, in order from top-to-bottom, until boolean-expr evaluates to false.
    - Somewhat confusingly, the behavior of the Scratch block is reversed, but it is the closest analog.
    do
    {

    }
    while (boolean-expr);
    - This loop will execute all lines of code between the curly braces once, and then, if the boolean-expr evaluates to true, will go back and repeat that process until boolean-expr evaluates to false.
    for (int i = 0; i < 10; i++)
    {

    }
    - Syntactically unattractive, but for loops are used to repeat the body of a loop a specified number of times, in this example 10.
    - The process undertaken in a for loop is:
        - The counter variable(s) (here, i) is set
        - The Boolean expression is checked.
            - If it evaluates to true, the body of the loop executes.
            - If it evaluates to false, the body of the loop does not execute.
        - The counter variable is incremented, and then the Boolean expression is checked again, etc.
    for (start; expr; increment) 
    { 

    }
    - Syntactically unattractive, but for loops are used to repeat the body of a loop a specified number of times, in this example 10.
    - The process undertaken in a for loop is:
            - The statement(s) in start are executed 
            - The expr is checked.
            - If it evaluates to true, the body of the loop executes.
            - If it evaluates to false, the body of the loop does not execute.
        - The statement(s) in increment are executed, and then the expr is checked again, etc.
while
    - Use when you want a loop to repeat an unknown number of times, and possibly not at all. 
do-while 
    - Use when you want a loop to repeat an unknown number of times, but at least once. 
for
    - Use when you want a loop to repeat a discrete number of times, though you may not know the number at the moment the program is completed.