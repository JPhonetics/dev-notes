---
Created: 2026-10-19
Modified:
---

# Syntax and Structure

Python syntax defines the rules for writing valid Python code, while structure determines how statements are organized and grouped together. Python emphasizes readability by using indentation, whitespace, and straightforward syntax to make code easier to understand and maintain.

Unlike languages such as JavaScript and C++, Python does not require semicolons to end statements or curly braces to define code blocks. Instead, Python generally uses newlines to separate statements and indentation to establish their structure.

When a Python script executes, it does not automatically display the result of every operation. For example, assignments and calculations can execute without producing terminal output. Use `print()` when you want to display a value. Defining a function also does not execute its body; the statements inside run when that function is called. We'll explore these behaviors throughout the examples below.

## Overview

1. [Identifiers and Naming Conventions](#identifiers-and-naming-conventions)
2. [Keywords](#keywords)
3. [Statements and Expressions](#statements-and-expressions)
4. [Lines and Whitespace](#lines-and-whitespace)
5. [Indentation and Code Blocks](#indentation-and-code-blocks)
6. [Line Continuation](#line-continuation)
7. [Comments and Docstrings](#comments-and-docstrings)
8. [Common Syntax Errors](#common-syntax-errors)

## Identifiers and Naming Conventions

Identifiers are names used to identify and reference elements in Python, such as variables, functions, classes, and modules. They allow developers to assign meaningful names to different parts of a program, making code easier to read, understand, and maintain.

Instead of repeating values throughout a program, we can reference them through identifiers. This makes it easier to read the code and update values in one place. In the following example, `username` and `age` identify two variables.

```python
username = "Alice"
age = 25

print(username, age)
```

**Output:**

```text
Alice 25
```

### Identifier Rules

Python enforces specific rules when naming identifiers. Violating these rules results in a syntax error.

- Must begin with a letter or underscore (`_`), not a number. Python also supports many Unicode letters.
- May contain letters, numbers, and underscores.
- Cannot contain spaces or special characters such as `@`, `!`, or `-`.
- Are case-sensitive, meaning `username` and `userName` are different identifiers.
- Cannot use reserved Python keywords such as `if`, `class`, or `return`.

**Valid identifiers:**

```python
username = "Alice"
user_age = 25
_private_value = 100
user2 = "Bob"
```

**Invalid identifiers (examples of code that will raise errors; do not execute):**

```python
2username = "Alice"
user-name = "Alice"
user name = "Alice"
class = "Python"
```

Here, `2username` begins with a number, `user-name` uses a hyphen, `user name` contains a space, and `class` is reserved.

### Case Sensitivity

Python identifiers are case-sensitive, meaning uppercase and lowercase letters are treated as different characters. For example, `username`, `userName`, and `USERNAME` are three separate names:

```python
username = "Alice"
userName = "Bob"
USERNAME = "Charlie"

print(username)
print(userName)
print(USERNAME)
```

**Output:**

```text
Alice
Bob
Charlie
```

### Naming Conventions

In addition to the naming rules enforced by Python, developers follow naming conventions to keep code consistent and readable.

> [!IMPORTANT]
> Naming rules and naming conventions are different. **Naming rules** are enforced by Python and must be followed for code to execute successfully. **Naming conventions** are recommended practices that improve consistency and readability but are not generally enforced by the language.

Python's PEP 8 style guide defines commonly accepted conventions for naming variables, functions, classes, constants, and modules.

| Element | Convention | Example |
|---|---|---|
| Variables | `snake_case` | `user_name` |
| Functions | `snake_case` | `calculate_total()` |
| Classes | `PascalCase` | `UserAccount` |
| Constants | `UPPER_SNAKE_CASE` | `MAX_RETRIES` |
| Modules | `lowercase` or `snake_case` | `file_utils.py` |

**snake_case**

Uses lowercase letters with underscores (`_`) separating words. Commonly used for variables and functions.

```python
user_name = "Alice"
account_balance = 1500

def calculate_total():
    return 100 + 200
```

**PascalCase**

Capitalizes the first letter of each word without using underscores. Commonly used for class names.

```python
class UserAccount:
    pass
```

**UPPER_SNAKE_CASE**

Uses uppercase letters with underscores separating words. Commonly used for **constants**, which are values intended to remain unchanged throughout a program. However, Python does not enforce constants, meaning a value assigned to `MAX_RETRIES` can still be reassigned. Developers follow this naming convention to communicate that the value should not be modified.

```python
MAX_RETRIES = 5
DEFAULT_TIMEOUT = 30
```

[🔼 Back to Overview](#overview)

## Keywords

Keywords are reserved words that have special meanings within Python's syntax. They are used to define the structure and behavior of programs, including conditional statements, loops, functions, classes, and other language features.

Because keywords are reserved by Python, they cannot be used as ordinary identifiers. Python also supports **soft keywords**, which only act as keywords in specific contexts. For example, `match` and `case` are used in structural pattern matching but can still be used as identifiers in other contexts.

For example, `if` is a keyword used to create conditional statements. Attempting to use it as a variable name results in a syntax error.

**Invalid (do not execute):**

```python
if = 25
```

**Valid:**

```python
age = 25

if age >= 18:
    print("Adult")
```

**Output:**

```text
Adult
```

The following table groups several common keywords by purpose. There is no need to memorize all of them now; you will encounter them throughout Python Basics.

| Category | Keywords |
|---|---|
| Conditional statements | `if`, `elif`, `else` |
| Loops | `for`, `while`, `break`, `continue` |
| Functions | `def`, `return`, `lambda` |
| Classes | `class` |
| Exception handling | `try`, `except`, `finally`, `raise` |
| Logical and membership operations | `and`, `or`, `not`, `in`, `is` |
| Imports | `import`, `from`, `as` |
| Other | `True`, `False`, `None`, `pass`, `with`, `yield`, `async`, `await` |

Python provides a standard library module named `keyword` that can display the reserved keywords supported by the current Python interpreter. The `keyword.kwlist` attribute contains these keywords, while `keyword.softkwlist` contains soft keywords.

```python
import keyword

print(keyword.kwlist)
print(keyword.softkwlist)
```

[🔼 Back to Overview](#overview)

## Statements and Expressions

Python programs use statements and expressions to describe operations. Although closely related, they serve different purposes.

- **Statement:** A complete instruction that Python executes, such as assigning a value to a variable, defining a function, or executing a loop.
- **Expression:** Code that evaluates to a value, such as a number, variable, calculation, comparison, or function call.

Statements can contain expressions. An expression can also stand alone as a complete instruction; Python calls this an **expression statement**. For example, a call such as `print("Hello")` is a function-call expression, and when written by itself as a line of code, it is also an expression statement.

### Statements

A statement is a complete instruction that tells Python to perform an operation. Python has several types of statements, including assignments, conditionals, loops, imports, and function definitions.

**Assignment statements** assign values to variables. In `age = 25`, `age` is the identifier, `=` performs assignment, and `25` is an integer literal. The full line `age = 25` is the assignment statement.

For example:

```python
age = 25
name = "Alice"
```

### Expressions

An expression is code that evaluates to a value. For example, `10 + 5` evaluates to `15`. An expression can also use a variable: if `age = 25`, then `age + 5` evaluates to `30`.

In a `.py` script, these calculated values are not displayed automatically. Use `print()` to display them.

**Arithmetic expression:**

```python
10 + 5
```

**Expression using a variable:**

```python
age = 25

age + 5
```

**Displaying the result:**

```python
age = 25

print(age + 5)
```

**Output:**

```text
30
```

### How Statements and Expressions Work Together

Statements frequently contain expressions. The expression provides a value that the statement uses to perform an operation. For `new_age = age + 5`, Python evaluates `age + 5` first, then assigns the result to `new_age`.

For example:

```python
age = 25
new_age = age + 5

print(new_age)
```

**Output:**

```text
30
```

### Expression Statements

An expression can also appear on its own as a complete statement. Python calls this an **expression statement**. This doesn't mean every statement is an expression: `age = 25` is an assignment statement, whereas a standalone function call such as `print("Hello, Python!")` is an expression statement.

A call to `print()` displays text and evaluates to `None`. If its return value is assigned to a variable, the whole line is an assignment statement instead. For example:

```python
print("Hello, Python!")
```

**The same function call within an assignment:**

```python
message = print("Hello, Python!")
```

In both examples, `Hello, Python!` is displayed. In the second, `message` receives `None`, not the printed text.

[🔼 Back to Overview](#overview)

## Lines and Whitespace

Python generally uses newlines to separate statements, so semicolons are not needed for ordinary code. Blank lines can separate logical sections and improve readability, but blank lines between complete statements normally do not change execution.

The following examples produce the same output despite differences in blank lines.

```python
name = "Alice"
age = 25
print(name, age)
```

```python
name = "Alice"

age = 25


print(name, age)
```

Output:

```text
Alice 25
```

[🔼 Back to Overview](#overview)

## Indentation and Code Blocks

Python uses indentation to define code blocks, which are groups of statements that belong together. Unlike languages such as JavaScript, which use curly braces `{}` to define code blocks, Python relies on whitespace to establish their structure.

Statements within the same block must use consistent indentation. A block generally begins after a statement ending with a colon (`:`), such as a conditional statement, loop, function, or class definition.

> [!IMPORTANT]
> Python convention is to use **four spaces to indicate indentation** for code inside a block. In **VS Code**, pressing **Tab** typically inserts four spaces automatically when configured to use spaces. Consistent indentation is important because it determines which lines of code belong together. Incorrect indentation can cause errors or change how your program executes.

In the following example, we define a function named `code_block()` containing two `print()` statements. Both statements are indented, indicating that they belong to the function's code block. When we call the function, both statements execute.

```python
def code_block():
    print("Line 1")
    print("Line 2")

code_block()
```

**Output:**

```text
Line 1
Line 2
```

Now, let's remove the indentation from the second `print()` statement. This moves it outside the function's code block, changing when it executes.

```python
def code_block():
    print("Line 1")
print("Line 2")

code_block()
```

**Output:**

```text
Line 2
Line 1
```

Notice how the output changes. Defining `code_block()` does not immediately execute the statements inside it. Instead, `print("Line 2")` executes first because it is outside the function. When `code_block()` is called, the indented `print("Line 1")` statement executes. Changing indentation changes which statements belong to the function and therefore when they execute.

[🔼 Back to Overview](#overview)

## Line Continuation

Python generally uses newlines to separate statements. However, long statements can be split across multiple lines to improve readability and make code easier to maintain. Parentheses `()` can be used to group expressions across multiple lines, while a backslash (`\`) can explicitly continue a statement onto the next line.

Python also supports line continuation within square brackets `[]` and curly braces `{}`. However, these enclosures serve different purposes and are not interchangeable.

> [!TIP]
> When a line becomes too long or difficult to read, consider splitting it across multiple lines. Python's PEP 8 style guide recommends limiting most code lines to 79 characters, although individual projects may adopt different limits.

### Arithmetic Expressions

The following arithmetic expression is written on a single line:

```python
total = 100 + 200 + 300 + 400 + 500 + 600 + 700 + 800
```

The same expression can be formatted across multiple lines using parentheses `()` or a backslash (`\`). Both approaches produce the same result, but parentheses are generally preferred because they make code easier to read and maintain.

**Using parentheses:**

```python
total = (
    100 + 200 + 300 + 400
    + 500 + 600 + 700 + 800
)

print(total)
```

**Using a backslash:**

```python
total = 100 + 200 + 300 + 400 + \
        500 + 600 + 700 + 800

print(total)
```

**Output (both examples):**

```text
3600
```

### Strings

Long strings can also be split across multiple lines to improve readability. Inside parentheses, adjacent string literals are automatically combined without inserting a newline. With a backslash placed inside a quoted string at the end of a physical line, the backslash escapes that newline. These are different mechanisms, but both examples below produce the same string. Be careful to include any space you want between words.

For example, the following string is written on a single line:

```python
message = "Hello, this is a very long message that continues onto the next line."
```

The same string can be written across multiple lines using parentheses and adjacent string literals or a backslash.

**Using parentheses:**

```python
message = (
    "Hello, this is a very long message that "
    "continues onto the next line."
)

print(message)
```

**Using a backslash:**

```python
message = "Hello, this is a very long message that \
continues onto the next line."

print(message)
```

**Output (both examples):**

```text
Hello, this is a very long message that continues onto the next line.
```

### Functions and Conditional Statements

Line continuation is also useful when working with functions that accept multiple parameters or conditional statements containing several conditions. Parentheses already surround function parameters, so they can span multiple lines without adding backslashes. For longer `if` conditions, parentheses let each check appear on its own line. Both formats below behave identically; the driving checks are only illustrative, not a complete legal determination.

The following function checks a simplified set of driving requirements using a single-line parameter list and conditional expression:

```python
def can_drive(age, has_license, has_insurance, is_registered):
    if age >= 16 and has_license and has_insurance and is_registered:
        return "Meets driving requirements"
    return "Cannot drive legally"

print(can_drive(18, True, True, True))
```

The same function can be formatted across multiple lines, separating its parameters and conditions to make the code easier to read and modify.

```python
def can_drive(
    age,
    has_license,
    has_insurance,
    is_registered,
):
    if (
        age >= 16
        and has_license
        and has_insurance
        and is_registered
    ):
        return "Meets driving requirements"
    return "Cannot drive legally"

print(can_drive(18, True, True, True))
```

**Output (both examples):**

```text
Meets driving requirements
```

Both versions behave identically. Notice that parentheses already surround the function parameters, so no additional continuation character is needed.

[🔼 Back to Overview](#overview)

## Comments and Docstrings

Comments and docstrings help developers understand the purpose, behavior, and design of Python code. As programs grow in complexity, documentation becomes increasingly valuable for maintaining, troubleshooting, and modifying code.

Well-written documentation helps developers understand why certain decisions were made, how code is intended to work, and how different parts of a program should be used. This is especially useful when revisiting code months or years later or collaborating with other developers.

### Comments

Comments begin with a hash symbol (`#`) and are ignored by Python during execution. They are useful for explaining logic, documenting decisions, and providing additional context about specific sections of code.

Comments should generally explain *why* something is done rather than simply repeating what the code already communicates. A comment can appear on its own line or after a statement (an *inline comment*); neither changes the intended program behavior.

```python
# Calculate the total price after applying a discount
price = 100
discount = 0.20

total = price * (1 - discount)  # Apply the discount

print(total)
```

**Output:**

```text
80.0
```

### Multiline Comments

Python does not have a dedicated syntax for multiline comments. Instead, multiple comment lines can be written by placing a hash symbol (`#`) at the beginning of each line. 

> [!TIP]
> The PEP 8 style guide recommends using `#` for regular comments, including comments spanning multiple lines. Triple-quoted strings are intended for multiline text and docstrings, not as a replacement for comments.

```python
# Calculate the total price after applying a discount.
# The discount is represented as a decimal percentage.
# For example, 0.20 represents a 20% discount.

price = 100
discount = 0.20

total = price * (1 - discount)
```

You may also encounter triple-quoted strings (`"""` or `'''`) used to simulate multiline comments. Although this resembles a multiline comment, triple quotes actually create a string literal. Python does not treat it as a comment, even when the string is not assigned to a variable or used elsewhere.

```python
"""
Calculate the total price after applying a discount.
The discount is represented as a decimal percentage.
For example, 0.20 represents a 20% discount.
"""
```

### Docstrings

Docstrings (documentation strings) describe the purpose and behavior of Python modules, functions, classes, and methods. Unlike regular comments, docstrings are stored as documentation associated with these objects and can be accessed programmatically.

Docstrings are written as string literals placed at the beginning of the object being documented. They commonly use triple double quotes (`"""`), although triple single quotes (`'''`) are also supported.

> [!TIP]
> Python's PEP 257 docstring conventions recommend using triple double quotes (`"""`) for consistency, even when a docstring fits on a single line.

For example, the following function includes a single-line docstring describing what it does.

```python
def greet(name):
    """Return a greeting for the specified name."""
    return f"Hello, {name}!"

print(greet("Alice"))
```

**Output:**

```text
Hello, Alice!
```

Docstrings can also span multiple lines to provide more detailed documentation, such as describing parameters and return values. The two forms below document the same behavior; the multiline version adds details for other developers.

```python
def greet(name):
    """
    Generate a personalized greeting.

    Args:
        name (str): The name of the person to greet.

    Returns:
        str: A greeting containing the person's name.
    """
    return f"Hello, {name}!"
```

### Accessing Docstrings

Unlike comments, docstrings can be retrieved using Python's built-in `help()` function, which displays documentation about functions, classes, modules, and other Python objects. This lets you inspect documentation without opening the original source file. A docstring is also accessible directly through the object's `__doc__` attribute.

> [!IMPORTANT]
> Triple-quoted strings are not automatically docstrings. A string becomes a docstring when it appears as the first statement in a module, function, class, or method. Triple-quoted strings elsewhere remain ordinary string literals.

For example, using the `greet()` function:

```python
def greet(name):
    """Return a greeting for the specified name."""
    return f"Hello, {name}!"

help(greet)
```

**Output (simplified):**

```text
Help on function greet in module __main__:

greet(name)
    Return a greeting for the specified name.
```

Docstrings can also be accessed directly using the `__doc__` attribute.

```python
print(greet.__doc__)
```

**Output:**

```text
Return a greeting for the specified name.
```

[🔼 Back to Overview](#overview)

## Common Syntax Errors

A syntax error occurs when Python encounters code that violates its grammatical rules and cannot compile it into an executable code object.

Common errors include:

- `SyntaxError` — Python encounters invalid syntax.
- `IndentationError` — Code is incorrectly indented.
- `TabError` — Tabs and spaces are used inconsistently in indentation.

> [!IMPORTANT]
> Syntax errors differ from runtime errors. Syntax errors prevent code from being compiled successfully, while runtime errors occur during execution. Runtime errors can be handled using Python's exception-handling mechanisms.

**Missing colon**

```python
# Incorrect
if True
    print("Hello")

# Correct
if True:
    print("Hello")
```

**Incorrect indentation**

```python
# Incorrect
if True:
print("Hello")

# Correct
if True:
    print("Hello")
```

**Unclosed parentheses**

```python
# Incorrect
print("Hello"

# Correct
print("Hello")
```

**Invalid identifiers**

```python
# Incorrect
2name = "Alice"

# Correct
name2 = "Alice"
```

[🔼 Back to Overview](#overview)

---
 
[🔼 Back to Python Basics](README.md)