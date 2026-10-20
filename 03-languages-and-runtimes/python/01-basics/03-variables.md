---
Created: 2026-10-19
Modified:
---

# Variables

Variables are fundamental concepts in Python that allow developers to assign meaningful names to values, making code easier to read, reuse, and maintain.

In Python, **everything is an object**, including numbers, strings, functions, and collections. Variables are names that reference these objects, allowing us to access and work with their values throughout a program. Unlike containers that physically hold values, variables reference objects and can be reassigned to reference different objects.

Python does not require variables to be declared with a specific data type. Instead, objects have types, and variables can reference objects of different types throughout a program.

The ability to reuse variables also relates to an important software development principle called **DRY (Don't Repeat Yourself)**, which encourages reducing unnecessary duplication in code.

## Overview

1. [Variables and Assignment](#variables-and-assignment)
2. [Reassigning Variables](#reassigning-variables)
3. [Constants](#constants)
4. [Multiple Assignment and Unpacking](#multiple-assignment-and-unpacking)

## Variables and Assignment

A variable is a name used to reference an object in Python. Variables allow developers to work with values without repeatedly writing those values directly into their code.

By convention, variable names use lowercase `snake_case`, with underscores separating words. Variable names should also be descriptive enough to communicate what they represent.

Variables are created by assigning a value to an identifier using the assignment operator (`=`).

> [!NOTE]
> The assignment operator (`=`) is different from the equality comparison operator (`==`). Assignment associates a value with a variable, while equality comparison checks whether two values are equal.

The basic syntax for assigning a variable is:

```python
variable_name = value
```

Unlike some programming languages, such as JavaScript, which uses keywords like `let` and `const` to declare variables, Python does not require a separate variable declaration. Instead, variables are created directly through assignment using the `=` operator.

For example, we can create variables to represent a person's name, age, and account balance. Each variable references an object whose value can be accessed later by using its identifier.

- `username` references the string `"Alice"`.
- `age` references the integer `25`.
- `account_balance` references the floating-point number `1500.50`.

```python
username = "Alice"
age = 25
account_balance = 1500.50

print(username)
print(age)
print(account_balance)
```

**Output:**

```text
Alice
25
1500.5
```

Variables can also be used within expressions and passed as arguments to functions. Instead of repeatedly writing the same values, we can reuse variable names in calculations and other operations.

For example, we can calculate the total price of a purchase by multiplying `price` by `quantity` and assigning the result to `total`.

```python
price = 100
quantity = 3

total = price * quantity

print(total)
```

**Output:**

```text
300
```

Python evaluates the expression `price * quantity` using the values referenced by those variables, then assigns the result to `total`.

[🔼 Back to Overview](#overview)

## Reassigning Variables

Variables can be reassigned throughout a program. **Reassignment** means associating an existing variable name with another object using the assignment operator (`=`).

For example, we might update an account balance after making a deposit.

```python
balance = 100

print(balance)

balance = 150

print(balance)
```

**Output:**

```text
100
150
```

Initially, `balance` references the integer `100`. After reassignment, it references `150`.

Python executes these statements in order, so the second `print()` displays the updated value.

We can also use the current value of a variable when calculating its new value. A variable can appear on both sides of the assignment operator (`=`) because Python evaluates the expression on the right before assigning the result to the variable on the left.

```python
balance = 100

balance = balance + 50

print(balance)
```

**Output:**

```text
150
```

Python evaluates `100 + 50` and assigns the resulting value, `150`, back to `balance`.

### Reassigning Different Data Types

Python is a **dynamically typed language**, meaning a variable does not have a fixed data type. Instead, the objects it references have types.

A variable can initially reference an integer and later be reassigned to a string. The `type()` function identifies the data type of the object referenced by a variable.

> [!IMPORTANT]
> **Reassignment** changes which object a variable references, while **mutation** changes an existing object's contents without replacing the object.
>
> For example, assigning a new list to a variable is reassignment, while adding an element to an existing list using `.append()` is mutation.

For example:

```python
value = 25

print(value)
print(type(value))

value = "Twenty-five"

print(value)
print(type(value))
```

**Output:**

```text
25
<class 'int'>
Twenty-five
<class 'str'>
```

The variable `value` first references an integer object and then a string object.

[🔼 Back to Overview](#overview)

## Constants

A **constant** is a variable intended to reference the same value throughout a program. Unlike some programming languages, Python does not enforce constants, meaning these variables can still be reassigned.

By convention, constants use `UPPER_SNAKE_CASE`, with underscores separating words. This naming convention communicates that the variable should not be reassigned, even though Python allows it.

Constants are commonly used for configuration settings, fixed limits, and values reused throughout a program. For example, you might define a constant representing the maximum number of retry attempts allowed before an operation fails.

For example:

```python
MAX_RETRIES = 5
DEFAULT_TIMEOUT = 30
SALES_TAX_RATE = 0.08

print(MAX_RETRIES)
print(DEFAULT_TIMEOUT)
print(SALES_TAX_RATE)
```

**Output:**

```text
5
30
0.08
```

Using a named constant makes the purpose of a value easier to understand and allows developers to update it in one location rather than changing the same value throughout a program.

For example, we can define `SALES_TAX_RATE` once and reuse it to calculate the tax and total price of a purchase.

```python
SALES_TAX_RATE = 0.08

price = 100
tax = price * SALES_TAX_RATE
total = price + tax

print(total)
```

**Output:**

```text
108.0
```

[🔼 Back to Overview](#overview)

## Multiple Assignment and Unpacking

Python allows developers to assign values to multiple variables within a single statement. This can reduce repetitive code and make related assignments more concise.

Python also supports **unpacking**, which distributes elements from an iterable, such as a tuple or list, into separate variables.

### Multiple Assignment

Multiple variables can be assigned values simultaneously by separating the identifiers and values with commas.

For example, instead of writing:

```python
first_name = "Alice"
age = 25
is_active = True
```

We can write:

```python
first_name, age, is_active = "Alice", 25, True

print(first_name)
print(age)
print(is_active)
```

**Output:**

```text
Alice
25
True
```

Python assigns each value to the corresponding variable based on its position.

The number of variables must match the number of values being unpacked unless extended unpacking syntax is used.

### Assigning the Same Value to Multiple Variables

Python also supports **chained assignment**, which allows multiple variables to reference the same object.

For example:

```python
a = b = c = 0

print(a)
print(b)
print(c)
```

**Output:**

```text
0
0
0
```

All three variables initially reference the same integer object.

> [!CAUTION]
> Be careful when using chained assignment with mutable objects such as lists or dictionaries. Multiple variables may reference the same object, meaning a modification through one variable can affect what the others reference.

In this example, both `a` and `b` reference the same list object, initially containing `[1, 2, 3]`. When we add `4` through `a`, the change is also visible through `b` because both variables reference the same object.
   - The `.append()` method adds a single element to the end of an existing list. Because lists are mutable, `.append()` modifies the original list rather than creating a new one.

```python
a = b = [1, 2, 3]

a.append(4)

print(a)
print(b)
```

**Output:**

```text
[1, 2, 3, 4]
[1, 2, 3, 4]
```

### Swapping Variables

Python supports swapping the values referenced by two or more variables without creating a temporary variable. Python evaluates the expressions on the right before assigning their values to the variables on the left.

This allows multiple values to be swapped or rearranged in a single statement.

For example:

```python
a = 10
b = 20

a, b = b, a

print(a)
print(b)
```

**Output:**

```text
20
10
```

Another example with more variables:

```python
a = 0
b = 10
c = 20
d = 30
e = 40

a, b, c, d, e = e, c, d, a, b

print(a)
print(b)
print(c)
print(d)
print(e)
```

**Output:**

```text
40
20
30
0
10
```

### Unpacking Collections

Unpacking allows values from an iterable to be assigned to individual variables.

An **iterable** is an object whose elements can be accessed one at a time, such as a list, tuple, or string. When unpacking, Python assigns elements to variables based on their positions.

> [!IMPORTANT]
> When unpacking without an asterisk (`*`), the number of variables must match the number of elements being unpacked. Otherwise, Python raises a `ValueError`.

For example, a tuple containing three values can be unpacked into three separate variables.

```python
person = ("Alice", 25, "California")

name, age, state = person

print(name)
print(age)
print(state)
```

**Output:**

```text
Alice
25
California
```

Each variable receives the value at the corresponding position in the tuple.

Unpacking also works with lists and other iterable objects.

For example:

```python
colors = ["Red", "Green", "Blue"]

first, second, third = colors

print(first)
print(second)
print(third)
```

**Output:**

```text
Red
Green
Blue
```

### Extended Unpacking

Python supports **extended unpacking** using an asterisk (`*`) to collect multiple elements into a list. The asterisk can appear at the beginning, middle, or end of the assignment targets and collects the elements not assigned to the other variables.

Only one assignment target can use an asterisk (`*`) in a single unpacking assignment.

Extended unpacking is especially useful when working with sequences containing a variable number of elements.

**Example 1: Collecting the Middle Elements**

In this example, we unpack a list of five numbers into three variables:

- `first` receives the first element.
- `middle` receives a list containing the elements between the first and last.
- `last` receives the final element.

```python
numbers = [10, 20, 30, 40, 50]

first, *middle, last = numbers

print(first)
print(middle)
print(last)
```

**Output:**

```text
10
[20, 30, 40]
50
```

**Example 2: Collecting All Elements Except the Last**

In this example, the asterisk appears before `beginning`, allowing it to collect all elements except the final one, which is assigned to `last`.

- `beginning` receives a list containing the first four elements.
- `last` receives the final element.

```python
numbers = [10, 20, 30, 40, 50]

*beginning, last = numbers

print(beginning)
print(last)
```

**Output:**

```text
[10, 20, 30, 40]
50
```

If we move the asterisk (`*`) to `last` instead (`beginning, *last = numbers`), the results would be reversed: `beginning` would receive the first element (`10`), while `last` would receive a list containing the remaining elements (`[20, 30, 40, 50]`).

[🔼 Back to Overview](#overview)

---

[🔼 Back to Python Basics](README.md)