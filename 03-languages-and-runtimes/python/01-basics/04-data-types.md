---
Created: 2026-10-20
Modified:
---

# Data Types

Data types describe what kind of value an object represents and which operations that object supports. Python provides built-in types for working with numbers, text, Boolean values, collections, binary data, and the absence of a value.

In Python, **objects have types, while variable names reference objects**. A variable can reference an integer at one point and a string later without declaring a new type. Understanding data types helps explain why some operations work with certain values but not others.

This guide introduces the built-in data types, their common characteristics, ways to inspect an object's type, and basic type conversion. Individual types and their operations are explored in more detail in later Python Basics guides.

## Overview

1. [Understanding Indexing](#understanding-indexing)
2. [Understanding Data Types](#understanding-data-types)
3. [Built-in Data Types](#built-in-data-types)
4. [Data Type Characteristics](#data-type-characteristics)
5. [Checking Data Types](#checking-data-types)
6. [Dynamic and Strong Typing](#dynamic-and-strong-typing)
7. [Type Conversion](#type-conversion)

## Understanding Indexing

**Indexing** allows developers to access individual elements within a sequence, such as a list or string, using their positions. Not all Python data types support indexing. Lists, tuples, and strings support positional indexing, while integers and sets do not.

An **element** is an individual item within a sequence or collection. For example, a list can contain multiple numbers, with each number representing an element. In a string, each individual character, including spaces, occupies a position in the sequence.

> [!NOTE]
> **Objects and elements describe different aspects of a value.** An object has a type and identity, while an element describes an object's role within a sequence or collection. A single collection can contain elements of different data types.
>
> For example, `[10, "Hello", True]` is a list **object** containing three **elements**: an integer (`10`), a string (`"Hello"`), and a Boolean (`True`).

Python uses **zero-based indexing**, meaning the first element has an index of `0`, the second has an index of `1`, and so on.

**Negative indexing** allows developers to access elements by counting backward from the end of a sequence. An index of `-1` accesses the last element, `-2` accesses the second-to-last element, and so on. This is useful when accessing elements without knowing the sequence's length.

Square brackets (`[]`) are used to specify the index immediately after the variable name.

- **`variable[0]`** — Accesses the first element.
- **`variable[1]`** — Accesses the second element.
- **`variable[2]`** — Accesses the third element.
- **`variable[-1]`** — Accesses the last element.
- **`variable[-2]`** — Accesses the second-to-last element.
- **`variable[-3]`** — Accesses the third-to-last element.

### Indexing a List

Lists are collections that allow elements to be accessed by their positions. Each element is separated by a `,`.

For example:

```python
# indexes   0   1   2   3   4
# negative -5  -4  -3  -2  -1
numbers = [40, 20, 10, 60, 30]

print(numbers[0])
print(numbers[2])
print(numbers[4])
print(numbers[-1])
```

**Output:**

```text
40
10
30
30
```

In this example, `numbers[0]` accesses the first element (`40`), `numbers[2]` accesses the third element (`10`), `numbers[4]` accesses the fifth element (`30`), and `numbers[-1]` accesses the last element (`30`) which is also the fifth element.

### Indexing a String

Strings are sequences of characters, with each character occupying an index position beginning at `0`. Spaces also count as characters and occupy their own index positions.

For example:

```python
# indexes  012345678
# neg     -987654321
message = "Hi there!"

print(message[0])
print(message[2])
print(message[5])
print(message[-3])
```

**Output:**

```text
H
 
e
r
```

Each index accesses an individual character within the string, including spaces. `message[0]` accesses the first character (`H`), `message[2]` accesses the third character (a space), `message[5]` accesses the sixth character (`e`), and `message[-3]` accesses the third-to-last character (`r`).

Notice that `message[2]` returns a space. Because `print()` displays that space without surrounding characters, the second line appears blank even though a character was printed.

### Accessing an Invalid Index

Attempting to access an index outside the available range raises an `IndexError`.

For example, a list containing three elements has indexes `0`, `1`, and `2`. Attempting to access index `3` raises an error because no element exists at that position.

**Invalid (raises an error):**

```python
numbers = [10, 20, 30]

print(numbers[3])
```

**Output (error):**

```text
IndexError: list index out of range
```

[🔼 Back to Overview](#overview)

## Understanding Data Types

A **data type** determines how Python represents an object and what operations are available for it. For example, integers support arithmetic, strings represent text, and lists can hold multiple elements.

The most important distinction is that a **variable name is not assigned a permanent type**. Instead, a variable references an object, and that object has a type. The assignment operator (`=`) associates the name with the object; it does not declare the variable's type.

Python provides a built-in function called `type()` that identifies an object's data type. When used with `print()`, it displays the object's value or type in the terminal.

In the example below, Python recognizes each assigned object according to its type:

- `username` references a string (`str`).
- `age` references an integer (`int`).
- `height` references a floating-point number (`float`).
- `is_active` references a Boolean (`bool`).
- `favorite_numbers` references a list (`list`).

```python
username = "Alice"
age = 25
height = 5.8
is_active = True
favorite_numbers = [5, 10, 15]

print(username, type(username))
print(age, type(age))
print(height, type(height))
print(is_active, type(is_active))
print(favorite_numbers, type(favorite_numbers))
```

**Output:**

```text
Alice <class 'str'>
25 <class 'int'>
5.8 <class 'float'>
True <class 'bool'>
[5, 10, 15] <class 'list'>
```

Each `print()` call displays the value referenced by the variable alongside its data type, as returned by `type()`.

[🔼 Back to Overview](#overview)

## Built-in Data Types

Python includes several built-in types organized into categories. The following table is a reference for recognizing common values; the examples show how those values can appear directly in Python code.

| Category | Data Type | Description | Example |
|---|---|---|---|
| Numeric | `int` | Integer (whole number) | `25` |
| Numeric | `float` | Floating-point number | `25.5` |
| Numeric | `complex` | Number with real and imaginary components | `3 + 4j` |
| Text | `str` | Sequence of Unicode characters | `"Alice"` |
| Boolean | `bool` | Logical value: `True` or `False` | `True` |
| Sequence | `list` | Ordered, mutable collection | `[1, 2, 3]` |
| Sequence | `tuple` | Ordered, immutable collection | `(1, 2, 3)` |
| Sequence | `range` | Immutable sequence of integers | `range(5)` |
| Mapping | `dict` | Collection of key-value pairs | `{"age": 25}` |
| Set | `set` | Unordered collection of unique elements | `{1, 2, 3}` |
| Set | `frozenset` | Immutable set of unique elements | `frozenset({1, 2})` |
| Binary | `bytes` | Immutable sequence of bytes | `b"Hello"` |
| Binary | `bytearray` | Mutable sequence of bytes | `bytearray(b"Hello")` |
| Binary | `memoryview` | View of data stored in a binary buffer | `memoryview(b"Hi")` |
| None | `NoneType` | Type of `None`, representing the absence of a value | `None` |

### Recognizing Data Types by Their Syntax

Some Python data types can be recognized by how their values are written. Different types use quotation marks, brackets, parentheses, or braces to represent their contents.

| Data Type | Syntax | Example |
|---|---|---|
| String (`str`) | Single (`' '`) or double (`" "`) quotation marks | `"Hello"` |
| List (`list`) | Square brackets (`[]`) | `[10, 20, 30]` |
| Tuple (`tuple`) | Comma-separated elements, typically enclosed in parentheses (`()`) | `(10, 20, 30)` |
| Dictionary (`dict`) | Curly braces (`{}`) containing key-value pairs separated by colons (`:`) | `{"name": "Alice", "age": 25}` |
| Set (`set`) | Curly braces (`{}`) containing unique elements | `{10, 20, 30}` |
| Boolean (`bool`) | Keywords `True` or `False` | `True` |
| None (`NoneType`) | Keyword `None` | `None` |

> [!IMPORTANT]
> The symbols used to represent values help identify their data types, but some symbols have multiple purposes.
>
> - Parentheses can group expressions or surround tuples. A single-element tuple requires a trailing comma, such as `(10,)`.
> - Curly braces can represent dictionaries or sets. An empty `{}` creates a dictionary, while an empty set is created using `set()`.
> - Square brackets can create lists or access individual elements through indexing, depending on how they are used.

### Understanding the Differences Between Data Types

A few distinctions are worth remembering:

- **Integers and floats:** `25` is an `int`, while `25.0` is a `float`, even though they represent numerically equal values.
- **Strings:** Quotation marks distinguish text such as `"25"` from the integer `25`.
- **Lists and tuples:** Both can contain multiple elements, but lists can be modified in place, while tuples cannot have their elements replaced.
- **Dictionaries:** Store values associated with keys and preserve insertion order.
- **Sets:** Contain unique elements and do not guarantee a particular iteration order.
- **`None`:** Is a value with its own type (`NoneType`), not the same thing as `0`, `False`, or an empty string.

> [!NOTE]
> **Truthy** and **falsy** are not separate data types. They describe how Python evaluates values in Boolean contexts, such as conditional statements.

Although two values may look similar, Python can recognize them as different data types. The built-in `type()` function allows us to identify the type of each object.

For example:

```python
age = 25
age_as_text = "25"
price = 25.0

print(age, type(age))
print(age_as_text, type(age_as_text))
print(price, type(price))
```

**Output:**

```text
25 <class 'int'>
25 <class 'str'>
25.0 <class 'float'>
```

Although `25`, `"25"`, and `25.0` represent similar-looking values, Python identifies them as different data types.

- `25` is an integer (`int`).
- `"25"` is a string (`str`) because it is enclosed in quotation marks.
- `25.0` is a floating-point number (`float`).

Notice that `print()` displays strings without their surrounding quotation marks. This means the displayed output alone may not reveal an object's data type. Using `type()` allows us to identify the type explicitly.

[🔼 Back to Overview](#overview)

## Data Type Characteristics

Python data types have different characteristics that determine how their objects behave and which operations they support.

Some of the most important characteristics include:

- **Mutable:** An object's contents can be modified after creation without replacing the object. Examples include lists, dictionaries, and sets.
- **Immutable:** An object's contents cannot be modified after creation. Examples include strings, integers, and tuples. Variables referencing immutable objects can still be reassigned.
- **Iterable:** An object whose elements can be accessed one at a time through iteration, such as with a `for` loop. Examples include lists, tuples, strings, dictionaries, and sets.
- **Ordered:** A collection or sequence whose elements have a defined order. This does not mean its values are sorted alphabetically or numerically.
- **Unordered:** A collection that does not guarantee a particular iteration order. Examples include sets and frozensets.

> [!NOTE]
> These characteristics are not separate data types. They describe how objects of different types behave. For example, a list is mutable, iterable, and ordered, while a string is immutable, iterable, and ordered.

The following table summarizes the characteristics of Python's built-in data types.

| Data Type | Mutable? | Iterable? | Ordered? |
|---|---|---|---|
| `int` | No | No | -- |
| `float` | No | No | -- |
| `complex` | No | No | -- |
| `str` | No | Yes | Yes |
| `bool` | No | No | -- |
| `list` | Yes | Yes | Yes |
| `tuple` | No | Yes | Yes |
| `range` | No | Yes | Yes |
| `dict` | Yes | Yes | Yes |
| `set` | Yes | Yes | No |
| `frozenset` | No | Yes | No |
| `bytes` | No | Yes | Yes |
| `bytearray` | Yes | Yes | Yes |
| `memoryview` | Depends¹ | Yes | Yes |
| `NoneType` | No | No | -- |

¹ A `memoryview` provides access to an underlying buffer. Depending on that buffer, the view may allow its elements to be modified or may be read-only.

**--** means that the characteristic does not apply to the data type.

### Mutable and Immutable Objects

**Mutable objects** allow their existing contents to be modified in place, such as adding, removing, or replacing elements in a list.

**Immutable objects** cannot be modified in place. To produce a different value, an operation must create or return another object, which can then be assigned to the same variable or a different variable.

Mutable objects often provide methods that modify their contents directly, while immutable objects may provide methods that return new objects without changing the original.

The following operations demonstrate how mutability affects lists and strings:

- **`.append(value)`** — Adds one element to the end of an existing list, modifying it in place. The method is called using dot notation (`variable.method()`).
- **`.remove(value)`** — Removes the first matching value from an existing list. Raises a `ValueError` if the value is not found.
- **`.pop(index)`** — Removes and returns an element at the specified index. If no index is provided, `.pop()` removes the last element. Raises an `IndexError` if the index is invalid or the list is empty.
- **`variable[index] = value`** — Replaces an element at a specified index. Lists support this operation, while strings do not.
- **`+` (Addition or Concatenation)** — Combines compatible values. When used with lists or strings, it creates a new object containing the combined elements or characters.
- **`=` (Assignment)** — Associates a variable name with an object. Reassignment changes which object the variable references rather than modifying the original.

> [!IMPORTANT]
> **Mutability describes whether an object can be modified, not whether every operation modifies it.** For example, `.append()` modifies an existing list, while `+` creates a new list.
>
> **Modification and reassignment are different operations.** Modification changes an existing object's contents, while reassignment changes which object a variable references.

#### Mutable List Examples

**Example 1: Adding an element using `.append()`.** The `.append(4)` method adds `4` to the end of the existing list without requiring reassignment.

```python
numbers = [1, 2, 3]
numbers.append(4)

print(numbers)
```

**Output:**

```text
[1, 2, 3, 4]
```

**Example 2: Replacing an element using indexing.** Assigning a value to `numbers[0]` replaces the first element of the existing list.

```python
numbers = [1, 2, 3]
numbers[0] = 10

print(numbers)
```

**Output:**

```text
[10, 2, 3]
```

The first element changes from `1` to `10`, while the remaining elements stay unchanged.

**Example 3: Removing an element using `.remove()`.** The `.remove(20)` method removes the first matching value rather than removing an element by its index.

```python
numbers = [10, 20, 30, 20]
numbers.remove(20)

print(numbers)
```

**Output:**

```text
[10, 30, 20]
```

Only the first occurrence of `20` is removed. The second occurrence remains in the list.

**Example 4: Removing an element using `.pop()`.** The `.pop(1)` method removes the element at index `1` and returns its value. Unlike `.remove()`, `.pop()` identifies the element by its index.

```python
numbers = [10, 20, 30]

print(numbers.pop(1))
print(numbers)
```

**Output:**

```text
20
[10, 30]
```

The first `print()` displays the removed value (`20`), while the second displays the modified list.

**Example 5: Combining two lists using `+`.** The `+` operator combines two lists into a new list, preserving the order of elements from each list. Unlike `.append()`, this does not modify either original list. The assignment operator (`=`) reassigns `numbers` to reference the combined list.

```python
numbers = [1, 2, 3]
numbers = [4] + numbers

print(numbers)
```

**Output:**

```text
[4, 1, 2, 3]
```

In this example, `[4]` and `numbers` are two separate lists. Because `[4]` appears before `numbers` in the expression, its element appears first in the resulting list.

#### Immutable String Examples

Strings are immutable, meaning their existing characters cannot be added, removed, or replaced in place.

Unlike lists, strings do not provide methods such as `.append()` for modifying their contents. Attempting to call an unavailable method raises an `AttributeError`, while attempting to replace an individual character through index assignment raises a `TypeError`.

To produce a string with additional or different characters, an operation such as `+` creates a new string, which can then be assigned to a variable.

**Example 1: Attempting to use `.append()` on a string.** Python raises an `AttributeError` because strings do not provide an `.append()` method.

```python
message = "Hello"
message.append("!")
```

**Output (error):**

```text
AttributeError: 'str' object has no attribute 'append'
```

**Example 2: Attempting to replace a character using indexing.** Python raises a `TypeError` because strings do not support modifying individual characters through index assignment.

```python
message = "Hello"
message[0] = "J"
```

**Output (error):**

```text
TypeError: 'str' object does not support item assignment
```

**



: Combining strings using `+`.** We use the `+` operator to concatenate the existing string referenced by `message` with `"!"`, creating a new string containing `"Hello!"`. The assignment operator (`=`) then reassigns `message` to reference this new string without modifying the original.

```python
message = "Hello"
message = message + "!"

print(message)
```

**Output:**

```text
Hello!
```

#### Comparing Mutable and Immutable Operations

| Operation | List | String |
|---|---|---|
| `.append()` | Modifies the existing list | Not supported |
| `.remove()` | Removes an element by value | Not supported |
| `.pop()` | Removes and returns an element | Not supported |
| `+` | Creates a new list | Creates a new string |
| `[index]` | Accesses an element | Accesses a character |
| `[index] = value` | Modifies the existing list | Raises a `TypeError` |

The distinction is that **lists support in-place modification, while strings require operations that produce new objects when their contents need to change**.

Notice that `print()` receives a variable as an argument to display its value, while methods such as `.append()` are called directly on an object using dot notation (`variable.method()`).

### Iterable and Non-Iterable Objects

An **iterable** is an object whose elements can be accessed one at a time through iteration.

Python commonly uses a `for` loop to iterate over an object's elements. The loop assigns each element to a variable and executes its indented code block once for each element.

Common iterable types include lists, tuples, strings, dictionaries, sets, and ranges. Numeric objects such as integers and floats are not iterable.

**Example 1: Iterating over a list.** The `for` loop assigns each element in `numbers` to the variable `number`, then uses `print()` to display it.

```python
numbers = [10, 20, 30]

for number in numbers:
    print(number)
```

**Output:**

```text
10
20
30
```

The loop displays each element in the order it appears in the list.

**Example 2: Iterating over a string.** Strings are iterable, allowing a `for` loop to access each character individually. Each character is assigned to `character` and displayed separately.

```python
message = "Hi"

for character in message:
    print(character)
```

**Output:**

```text
H
i
```

**Example 3: Attempting to iterate over an integer.** Integers are not iterable because they represent individual numeric values rather than collections of elements. Attempting to iterate over an integer raises a `TypeError`.

```python
number = 123

for digit in number:
    print(digit)
```

**Output (error):**

```text
TypeError: 'int' object is not iterable
```

Although `123` contains multiple digits in its written representation, Python treats it as a single integer object rather than an iterable sequence of digits.

### Ordered and Unordered Collections

**Ordered collections** maintain a defined order for their elements. This means elements retain a predictable sequence, but not necessarily alphabetical or numerical order. For example, lists preserve the positions of their elements, while dictionaries preserve the order in which keys are inserted.

**Unordered collections** do not guarantee a particular iteration order. Python's built-in `set` and `frozenset` are examples. They store unique elements without preserving insertion order and do not support positional indexing.

> [!NOTE]
> **Unordered does not mean randomly shuffled.** Elements may appear in a consistent order, but that order is not guaranteed.
>
> Python's built-in sets (`set` and `frozenset`) use hash values to organize their elements internally. Adding or removing elements may change their iteration order.
>
> A **hash value** is an integer calculated from an object's value using a hashing function, such as Python's built-in `hash()`. Hash-based collections use these values to help organize and locate elements efficiently.
>
> However, hashing and ordering are separate concepts. Not every unordered data structure uses hashing, and Python dictionaries also use hash-based storage while preserving insertion order.

**Example 1: Ordered list.** A list preserves the order of its elements, so iteration follows their existing positions.

```python
numbers = [30, 10, 20]

for number in numbers:
    print(number)
```

**Output:**

```text
30
10
20
```

The elements are displayed in their original order, even though the numbers are not sorted.

**Example 2: Unordered set.** Python uses a hash table to organize set elements based on their hash values rather than insertion order. The `.add(value)` method inserts an element into an existing set if it is not already present.

```python
numbers = {30, 10, 20, 40}

print(numbers)
print(numbers)

numbers.add(50)

print(numbers)
```

**Output (order may vary):**

```text
{40, 10, 20, 30}
{40, 10, 20, 30}
{40, 10, 20, 50, 30}
```

The first two `print()` calls generally display the same order because the set has not changed. Adding `50` modifies the set and may affect its internal arrangement, but Python does not guarantee any particular display order.

[🔼 Back to Overview](#overview)

## Checking Data Types

Python provides built-in functions for inspecting and validating the types of objects. This is useful when debugging unexpected behavior, checking user input, or ensuring a value supports the operations we intend to perform.

Two commonly used functions are:

- **`type(object)`** — Returns the object's type. This is useful for identifying what kind of object a variable references, especially when debugging or investigating unexpected behavior.
- **`isinstance(object, class_or_tuple)`** — Returns `True` if an object belongs to the specified class, a subclass, or any of the classes in a supplied tuple; otherwise returns `False`. This is useful for checking whether an object belongs to an accepted type before performing an operation.

Although both functions help identify types, they answer different questions: `type()` tells us **what type an object has**, while `isinstance()` tells us **whether an object belongs to an accepted type or type hierarchy**.

> [!NOTE]
> In Python, `bool` is a subclass of `int`. Therefore, `isinstance(True, int)` returns `True`, even though `type(True)` returns `bool`. This is one reason the two functions should not be treated as interchangeable.

### Using `type()`

The `type()` function accepts an object as its argument and returns its type. It can be used with a variable or directly with a literal.

When printed, the result commonly appears in the format `<class 'type_name'>`. The word `class` appears because Python's built-in types are classes.

**Example 1: Identifying data types.** We use `type()` to inspect the types of several objects.

```python
username = "Alice"
age = 25
height = 5.8
is_active = True
middle_name = None

print(type(username))
print(type(age))
print(type(height))
print(type(is_active))
print(type(middle_name))
```

**Output:**

```text
<class 'str'>
<class 'int'>
<class 'float'>
<class 'bool'>
<class 'NoneType'>
```

**Example 2: Checking literals directly.** We can inspect the type of a literal without assigning it to a variable first.

```python
print(type(100))
print(type("Hello"))
```

**Output:**

```text
<class 'int'>
<class 'str'>
```

**Example 3: Debugging an unexpected data type.** Suppose we expect `age` to be a number, but it was assigned a string. We can use `type()` to identify the problem before attempting arithmetic.

```python
age = "25"

print(age)
print(type(age))
```

**Output:**

```text
25
<class 'str'>
```

Although the displayed value looks like a number, `type()` reveals that `age` references a string. This helps explain why certain numerical operations might fail.

### Using `isinstance()`

The `isinstance()` function checks whether an object belongs to a specified type or one of its subclasses. Unlike `type()`, it returns a Boolean (`True` or `False`).

> [!IMPORTANT]
> `isinstance()` does not automatically raise an error when an object does not match the specified type. It returns `False`. We can use a conditional statement and `raise` to trigger an error when validation fails.

The basic syntax is:

```python
isinstance(object, type)
```

To accept multiple types, provide a tuple of classes as the second argument:

```python
isinstance(object, (type1, type2))
```

**Example 1: Checking individual types.** We use `isinstance()` to verify whether `age` and `username` belong to specific types.

```python
age = 25
username = "Alice"

print(isinstance(age, int))
print(isinstance(age, str))
print(isinstance(username, str))
```

**Output:**

```text
True
False
True
```

**Example 2: Checking multiple accepted types.** We provide a tuple of classes so Python accepts either an integer or a floating-point number.

```python
number = 25.5

print(isinstance(number, (int, float)))
```

**Output:**

```text
True
```

The result is `True` because `number` references a `float`, which is one of the accepted types.


**Example 3: Validating input and raising an error.** We use `isinstance()` inside a conditional statement to verify that `price` references an accepted numeric type.

The `not` operator reverses the Boolean result returned by `isinstance()`. If the result is `False`, `not` makes the condition `True`, causing the `raise` statement to trigger a `TypeError` with a custom message.

```python
price = "25"

if not isinstance(price, (int, float)):
    raise TypeError("Price must be a number")

print(price)
```

**Output (error):**

```text
TypeError: Price must be a number
```

Python raises the error because `price` references a string rather than an integer or float. The `print()` statement does not execute because the exception interrupts execution.

### Comparing `type()` and `isinstance()`

The difference between `type()` and `isinstance()` becomes important when working with **inheritance**. A **subclass** is a class derived from another class, called its **parent class**. A subclass inherits behavior from its parent class and is also recognized as an instance of that parent.

In Python, `bool` is a subclass of `int`. This means a Boolean value has an actual type of `bool`, but Python also recognizes it as an instance of `int`.

There are two important distinctions:

- **`type(object) == SomeType`** — Uses the equality operator (`==`) to compare the object's actual type against a specified type. It returns `True` only when the types match exactly. Subclasses do not count as an exact match.
- **`isinstance(object, SomeType)`** — Checks whether the object is an instance of the specified type **or one of its subclasses**. It can return `True` even when the object's actual type differs from the specified type.

| Function | Purpose | Return Value | Supports Subclasses? |
|---|---|---|---|
| `type(object)` | Identifies an object's actual type | Type object | N/A |
| `type(object) == SomeType` | Checks whether the type matches exactly | `True` or `False` | No |
| `isinstance(object, SomeType)` | Checks whether an object belongs to an accepted type | `True` or `False` | Yes |

**Example: Comparing exact types and subclasses.** We use `True`, which is a Boolean value, to demonstrate how the two approaches behave when checking against `int` and `bool`.

```python
value = True

print(type(value))
print(type(value) == int)
print(isinstance(value, int))
print(isinstance(value, bool))
```

**Output:**

```text
<class 'bool'>
False
True
True
```

Each result demonstrates a different behavior:

- **`type(value)` → `<class 'bool'>`** — Identifies the actual type of `True` as `bool`.
- **`type(value) == int` → `False`** — The equality comparison fails because `bool` and `int` are different types, even though `bool` is a subclass of `int`.
- **`isinstance(value, int)` → `True`** — Python recognizes the Boolean value as an instance of `int` because `bool` inherits from `int`.
- **`isinstance(value, bool)` → `True`** — The value is also an instance of its actual type, `bool`.

[🔼 Back to Overview](#overview)

## Dynamic and Strong Typing

Python is both **dynamically typed** and **strongly typed**. These characteristics describe how Python handles data types during program execution.

- **Dynamic typing:** Variables do not have fixed, declared types. Python determines an object's type at runtime (while the program is executing), allowing a variable to be reassigned to an object of a different type.
- **Strong typing:** Python generally enforces compatibility between data types rather than automatically treating incompatible values as interchangeable. Operations involving incompatible types may raise an error unless an appropriate conversion is performed.

> [!NOTE]
> **Dynamic typing does not mean an existing object changes its type.** When a variable is reassigned, it references a different object, which may have a different type.
>
> **Strong typing does not prevent every automatic conversion.** Python supports certain implicit conversions between compatible types, such as adding an `int` and a `float`.

### Dynamic Typing

Python does not require us to declare a variable's type before assigning a value. A variable can reference objects of different types throughout program execution.

This differs from languages that require variables to be declared with a particular type and restrict reassignment to compatible values.

**Example 1: Reassigning a variable to a different data type.** The variable `value` initially references an integer. We then reassign it to a string and use `type()` to inspect the type of the object it references.

```python
value = 25
print(type(value))

value = "Twenty-five"
print(type(value))
```

**Output:**

```text
<class 'int'>
<class 'str'>
```

The first output identifies the integer object. After reassignment, `value` references a string object, so the second output identifies `str`. Neither object changes its type.

### Strong Typing

Python generally requires values to have compatible types for an operation.

For example, the `+` operator can add numbers or concatenate strings, but it cannot directly combine a string and an integer.

When an operation receives incompatible types, Python may raise a `TypeError`. Even if a string contains numeric characters, Python does not automatically treat it as a number.

**Example 1: Attempting to add a string and an integer.** Although `"25"` looks like a number, Python treats it as a string. Adding it directly to an integer raises a `TypeError`.

```python
number = 5
text = "25"

result = number + text
```

**Output (error):**

```text
TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

The expression fails because `number` references an `int`, while `text` references a `str`. Since the addition fails, the assignment to `result` does not complete.

To perform the addition successfully, we must first convert the string into a compatible numeric type. We'll demonstrate this in the following section.

[🔼 Back to Overview](#overview)

## Type Conversion

**Type conversion** is the process of producing a value of one data type from a value of another data type. Python supports both automatic conversions during certain operations and conversions explicitly requested by the developer.

Type conversion is useful when values need to be compatible with an operation. As demonstrated with strong typing, Python cannot directly add an integer and a string, even when the string contains numeric characters.

Python supports two approaches:

- **Implicit conversion:** Python automatically converts compatible values as part of an operation. For example, adding an integer and a floating-point number typically produces a `float`.
- **Explicit conversion:** The developer calls a conversion function, such as `int()`, `float()`, or `str()`, to request a value of a particular type.

Common conversion functions include:

| Function | Purpose | Example result |
|---|---|---|
| `int(value)` | Converts a compatible value to an integer | `int("25")` → `25` |
| `float(value)` | Converts a compatible value to a floating-point number | `float("3.5")` → `3.5` |
| `str(value)` | Produces a string representation of a value | `str(25)` → `"25"` |
| `bool(value)` | Converts a value to `True` or `False` based on its truthiness | `bool(0)` → `False` |
| `list(iterable)` | Creates a list from an iterable | `list((1, 2))` → `[1, 2]` |
| `tuple(iterable)` | Creates a tuple from an iterable | `tuple([1, 2])` → `(1, 2)` |
| `set(iterable)` | Creates a set containing unique elements | `set([1, 1, 2])` → `{1, 2}` |

> [!IMPORTANT]
> **Not every type conversion is valid or preserves the original value exactly.**
>
> - `int(3.9)` produces `3`, discarding the fractional part rather than rounding.
> - `int("3.5")` raises a `ValueError` because the string is not written as a valid integer.
> - `int("Hello")` raises a `ValueError` because the string cannot be interpreted as an integer.
>
> Conversion functions produce a result rather than changing an existing object's type in place. The original variable continues referencing its original object unless it is explicitly reassigned.

### Implicit Conversion

**Implicit conversion** occurs automatically when Python performs certain operations between compatible data types.

For example, when adding an `int` and a `float`, Python produces a floating-point result without requiring us to call a conversion function.

However, implicit conversion does not mean Python automatically converts every compatible-looking value. A numeric string such as `"25"` remains a string unless explicitly converted.

**Example 1: Adding an integer and a float.** Python combines the two numeric values and produces a `float`.

```python
whole_number = 10
decimal_number = 2.5

result = whole_number + decimal_number

print(result)
print(type(result))
```

**Output:**

```text
12.5
<class 'float'>
```

The result is `12.5`, and `type()` confirms that the resulting object is a `float`. The original values remain an `int` and a `float`, respectively.

### Explicit Conversion

**Explicit conversion** occurs when we call a conversion function to request a value of a particular data type.

The conversion does not automatically reassign the original variable. We can assign the converted result to a separate variable or reassign the original variable name.

An important distinction is how the `+` operator behaves with different types:

- **Numeric addition (`+`):** Adds numeric values, such as `25 + 5`, producing `30`.
- **String concatenation (`+`):** Joins strings, such as `"25" + "5"`, producing `"255"`.

Although both operations use `+`, their behavior depends on the types of the objects involved.

**Example 1: Converting strings into numbers.** We use `int()` and `float()` to convert numeric strings into their corresponding numeric types, assigning each result to a new variable.

```python
age_text = "25"
age_number = int(age_text)

price_text = "3.5"
price_number = float(price_text)

print(age_text, type(age_text))
print(age_number, type(age_number))
print(price_text, type(price_text))
print(price_number, type(price_number))
```

**Output:**

```text
25 <class 'str'>
25 <class 'int'>
3.5 <class 'str'>
3.5 <class 'float'>
```

The output shows that `age_text` and `price_text` remain strings, while `age_number` and `price_number` reference newly produced numeric objects.

**Example 2: Converting a string before numeric addition.** We use `int()` to convert a numeric string into an integer before performing addition. This resolves the incompatible-type error demonstrated in Strong Typing.

```python
age_text = "25"
age_number = 5

age_total = int(age_text) + age_number

print(age_total)
print(type(age_total))
```

**Output:**

```text
30
<class 'int'>
```

The expression `int(age_text)` produces the integer `25`. Python then performs numeric addition (`25 + 5`), producing `30`.

**Example 3: Converting a number into a string.** If our goal is to combine text rather than perform arithmetic, we can use `str()` to convert a numeric value into a string before concatenating it.

```python
MAX_RETRIES = 5
message = "Max retry limit: " + str(MAX_RETRIES)

print(message)
print(type(message))
```

**Output:**

```text
Max retry limit: 5
<class 'str'>
```

The `str(MAX_RETRIES)` call produces the string `"5"`, allowing the `+` operator to concatenate it with `"Max retry limit: "`. The resulting object is a string.

[🔼 Back to Overview](#overview)

---

[🔼 Back to Python Basics](README.md)