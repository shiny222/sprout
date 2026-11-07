# Sprout Programming Language
## Prototype Documentation — Version 0.1 (Draft)

**Status:** Proposed language design for review. Sprout does not have an interpreter yet.  
**Goal:** An easy-to-read, Python-like language with C/C++-style `{}` code blocks.  
**Proposed keywords:** `let`, `fn`, `if`, `else`, `while`, `for`, `in`, `return`, `break`, `continue`, `and`, `or`, `not`, `true`, `false`, `null`.

> This is a proposed design, not a finalized specification. You can change keywords or syntax before implementing the interpreter.

---

## 1. A complete example

This example shows variables, functions, parameters, return values, conditions, `and`, `or`, lists, and loops. Comments beginning with `#` explain what each part does.

```text
# Define a function named can_enter.
# age, has_ticket, and is_staff are parameters.
fn can_enter(age, has_ticket, is_staff) {
    # A person can enter if they are 18 or older AND
    # they either have a ticket OR are a staff member.
    return age >= 18 and (has_ticket or is_staff)
}

# Create three variables. 'let' is used only when creating a variable.
let name = "Alex"
let age = 20
let has_ticket = true

# Call the function and save its returned boolean value.
let allowed = can_enter(age, has_ticket, false)

# Run one of the two blocks depending on the boolean condition.
if allowed {
    print("Welcome,", name)
} else {
    print("Access denied")
}

# A list stores multiple values in order.
let scores = [10, 20, 30]

# The loop runs once for each value in the list.
for score in scores {
    print("Score:", score)
}
```

**Expected output:**

```text
Welcome, Alex
Score: 10
Score: 20
Score: 30
```

## 2. General syntax rules

1. A program runs **top to bottom**, starting at the first statement. A `main()` function is not required.
2. **Curly braces `{}`** mark the start and end of blocks (functions, loops, and conditions).
3. **Indentation is for readability**, not a requirement of the language.
4. **No semicolons**: a newline normally ends a statement. Version 0.1 keeps one statement per line.
5. **Parentheses are not required** around `if` and `while` conditions, but can be used to group expressions.
6. **Variables have dynamic types**: you do not write `int` or `string` when declaring them.
7. **Names are case-sensitive**: `age` and `Age` are different names.

```text
# Both blocks are valid, even though their indentation differs.
if true {
    print("Hello")
}

if true {
print("Hello again")
}
```

## 3. Comments

Comments are notes for people reading the program. The interpreter ignores them.

```text
# This is a single-line comment.
let x = 5  # A comment may also follow code.

/*
   This is a block comment.
   It can span multiple lines.
*/
```

**Version 0.1 rule:** Block comments do not nest.

## 4. Variables and data types

Use `let` to **declare** a variable, and `=` to **change** an existing variable.

```text
let age = 25             # Integer: whole number
let price = 12.50        # Float: number with a decimal part
let name = "Sam"         # String: text in double quotes
let online = true        # Boolean: true or false
let nothing = null       # Null: absence of a value
let scores = [8, 9, 10]  # List: ordered collection of values

age = 26                 # Reassign an existing variable
name = "Taylor"          # A variable can hold a new value
```

**Supported value types for version 0.1:** `integer`, `float`, `string`, `boolean`, `null`, and `list`.

**Rules:**
- Using a variable before declaring it is an error.
- `let age = 25` creates a name; `age = 26` updates it.
- Assigning to a name that has not been declared is an error.
- The interpreter determines the value's type at runtime.
- List indexes start at `0`.

## 5. Operators

### Arithmetic

```text
let a = 10
let b = 3

print(a + b)  # 13: addition
print(a - b)  # 7: subtraction
print(a * b)  # 30: multiplication
print(a / b)  # 3.333...: division (produces a float)
print(a % b)  # 1: remainder
print(-a)     # -10: unary minus
```

### Comparison

Comparisons produce the boolean value `true` or `false`.

```text
print(5 == 5)  # true: equal
print(5 != 3)  # true: not equal
print(5 > 3)   # true: greater than
print(5 < 3)   # false: less than
print(5 >= 5)  # true: greater than or equal
print(5 <= 4)  # false: less than or equal
```

### Logical operators: `and`, `or`, `not`

**These are language keywords, not functions.** They combine boolean conditions.

```text
let age = 20
let has_ticket = false
let is_staff = true

# AND: true only when BOTH sides are true.
if age >= 18 and has_ticket {
    print("Adult with ticket")
}

# OR: true when AT LEAST ONE side is true.
if has_ticket or is_staff {
    print("Entry credential found")
}

# NOT: changes true to false, or false to true.
if not has_ticket {
    print("No ticket")
}

# Parentheses make mixed conditions easy to understand.
if age >= 18 and (has_ticket or is_staff) {
    print("Entry permitted")
}
```

**Logical rules:**
- In version 0.1, `and`, `or`, and `not` operate on **boolean values**.
- `and` uses short-circuit evaluation: if its left side is `false`, its right side is not evaluated.
- `or` uses short-circuit evaluation: if its left side is `true`, its right side is not evaluated.
- `not` reverses a boolean value.
- `if` and `while` conditions must evaluate to booleans in version 0.1.

| A | B | `A and B` | `A or B` |
|---|---|---|---|
| false | false | false | false |
| false | true | false | true |
| true | false | false | true |
| true | true | true | true |

### Operator precedence (highest to lowest)

1. Parentheses `(...)`, function calls `f(...)`, and indexing `items[i]`
2. Unary minus `-x`
3. Multiplication, division, remainder `*`, `/`, `%`
4. Addition and subtraction `+`, `-`
5. Comparisons `<`, `<=`, `>`, `>=`, `==`, `!=`
6. Logical NOT `not`
7. Logical AND `and`
8. Logical OR `or`

For example, `not age < 18 and has_ticket` means `(not (age < 18)) and has_ticket`. Use parentheses whenever a condition might be unclear.

## 6. Conditions

The interpreter evaluates conditions in order. Only the first matching block runs.

```text
let score = 75

if score >= 90 {
    print("Excellent")
} else if score >= 60 {
    print("Passed")
} else {
    print("Failed")
}
```

`else if` and `else` are optional. An `else` block has no condition.

## 7. Loops

### While loop

A `while` loop repeats while its condition is `true`.

```text
let counter = 0

while counter < 3 {
    print(counter)           # Print the current counter value.
    counter = counter + 1    # Change it so the loop eventually ends.
}

# Output: 0, then 1, then 2 (each on its own line).
```

### For loop with `range`

`range(start, end)` generates integers starting at `start`, **excluding** `end`.

```text
for i in range(0, 5) {
    print(i)  # Prints 0, 1, 2, 3, 4 on separate lines.
}
```

### For loop with a list

```text
let colors = ["red", "green", "blue"]

for color in colors {
    print(color)  # 'color' holds the next item on each iteration.
}
```

### `break` and `continue`

```text
for i in range(0, 6) {
    if i == 2 {
        continue  # Skip the rest of this iteration.
    }

    if i == 5 {
        break     # Exit the loop completely.
    }

    print(i)      # Prints 0, 1, 3, 4.
}
```

`break` and `continue` are only valid inside loops.

## 8. Functions, parameters, and return values

Use `fn` to declare a function. **Parameters** are the names inside the function declaration; **arguments** are the values supplied when calling it.

```text
# Define a function with two parameters: a and b.
fn add(a, b) {
    return a + b  # Stop the function and send a value back.
}

# Call add with two arguments: 10 and 20.
let answer = add(10, 20)
print(answer)  # 30

# Functions may also have zero parameters.
fn say_hello() {
    print("Hello")
    # No return statement: the function returns null.
}

say_hello()
```

**Function rules for version 0.1:**
- Arguments are passed by value for numbers, strings, booleans, and null; lists are shared references.
- A function call must supply the number of arguments its parameters require.
- `return` immediately exits the current function.
- If a function reaches its closing `}` without `return`, it returns `null`.
- `return` outside a function is an error.
- Function declarations are supported at the top level; nested function declarations can wait until a later version.

## 9. Lists and indexing

```text
let numbers = [10, 20, 30]  # Create a list of three integers.

print(numbers[0])            # 10: first item (index 0).
print(numbers[2])            # 30: third item (index 2).

numbers[1] = 99              # Replace the item at index 1.
print(numbers)               # [10, 99, 30]
print(len(numbers))          # 3: number of items.
```

Indexes must be integers between `0` and `len(list) - 1`. Accessing an invalid index is a runtime error in version 0.1.

## 10. Built-in functions

The first interpreter can provide these functions automatically:

| Function | Meaning | Example |
|---|---|---|
| `print(value, ...)` | Print values separated by spaces, then a newline | `print("Hi", 5)` |
| `input()` | Read one line of text from the user | `let name = input()` |
| `len(value)` | Length of a string or list | `len([1, 2])` → `2` |
| `range(start, end)` | Integer sequence, end excluded | `range(0, 3)` |
| `str(value)` | Convert a value to a string | `str(12)` → `"12"` |
| `int(value)` | Convert a compatible value to an integer | `int("42")` → `42` |
| `float(value)` | Convert a compatible value to a float | `float("2.5")` → `2.5` |
| `type(value)` | Return the name of a value's type | `type(true)` → `"boolean"` |

```text
print("What is your name?")
let name = input()       # input() always returns a string.
print("Hello", name)

print("How old are you?")
let age = int(input())   # Convert the text into an integer.
print("Next year:", age + 1)
```

A conversion such as `int("hello")` produces a runtime error. `range` is used for iteration; converting it to a list is not required in version 0.1.

## 11. Scope (where variables exist)

Every `{}` block creates a **local scope**. A variable declared with `let` belongs to that scope, and code inside the block can read variables in outer scopes.

```text
let x = 10           # Declared in the global scope.

if true {
    let y = 20       # 'y' exists only inside this block.
    x = 15           # Update the existing outer variable.
    print(x + y)     # 35
}

print(x)             # 15
# print(y)           # ERROR: 'y' is not defined here.
```

Function parameters and variables declared inside functions are local to the function call. Version 0.1 does not include closures or nested functions.

## 12. Errors

The interpreter should stop with a readable error message when a program cannot execute correctly.

```text
let result = 10 / 0     # Runtime error: division by zero.
# print(missing)        # Runtime error: unknown variable.
# let bad = [1, 2][9]  # Runtime error: list index out of range.
# if 123 { }            # Runtime error: condition must be boolean.
```

Example error format (proposed):

```text
RuntimeError at line 1: division by zero
```

Error messages should include the line number and a short explanation.

## 13. A small complete program: guessing game

This program shows how the features work together in a practical example.

```text
# Define a function that checks whether a guess is correct.
fn check_guess(guess, secret) {
    if guess == secret {
        return true             # Correct guess: tell caller to stop.
    } else if guess > secret {
        print("Too high")      # Guess is bigger than secret.
    } else {
        print("Too low")       # Guess is smaller than secret.
    }

    return false                # Wrong guess: keep playing.
}

let secret = 7                  # The number the player must guess.
let attempts = 0               # Count how many guesses are made.
let won = false                 # Track whether the game is finished.

print("Guess a number from 1 to 10")

while not won {                 # Repeat until 'won' becomes true.
    let guess = int(input())    # Read a line of text and convert it to int.
    attempts = attempts + 1    # Count the new attempt.
    won = check_guess(guess, secret)  # Call the helper function.
}

print("You won in", attempts, "attempts!")
```

## 14. Features deliberately NOT in version 0.1

To make the first interpreter achievable, these can be considered for later releases:

- Classes and objects
- Imports and modules
- Dictionaries/maps
- Exception handling (`try`/`catch`)
- Anonymous functions and closures
- Static typing and type annotations
- Advanced list operations and standard libraries

## 15. Design decisions to confirm before implementation

The current prototype proposes:

| Decision | Proposed choice |
|---|---|
| Language name | `Sprout` (temporary) |
| Variable declaration | `let name = value` |
| Function declaration | `fn name(parameters) { ... }` |
| Block delimiters | `{` and `}` |
| Statement endings | Newlines; no `;` |
| Logical operators | `and`, `or`, `not` |
| Conditional branches | `if`, `else if`, `else` |
| Loops | `while`, `for ... in ...` |
| Comments | `# ...` and `/* ... */` |
| Types | Dynamic |
| Condition values | Strict booleans (no implicit truthiness) |
| Block scoping | `let` is block-local |

**After approval:** define the tokens and grammar, build the lexer, parse into an Abstract Syntax Tree (AST), and execute it with an interpreter.
