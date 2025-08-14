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
