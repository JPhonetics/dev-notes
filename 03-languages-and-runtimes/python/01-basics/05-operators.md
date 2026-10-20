---
Created: 2026-10-20
Modified:
---

# Operators

Operators are symbols or keywords that tell Python to perform an operation on one or more values. They are used to perform calculations, compare values, assign values, evaluate conditions, and work with collections.

Understanding operators is important because the same operator can behave differently depending on the data types involved. For example, `+` can add numbers, concatenate strings, or combine lists.

This guide covers Python's operator categories, how they behave with different data types, and the rules that determine how expressions are evaluated.

## Overview

1. [Understanding Operators](#understanding-operators)
2. [Operator Precedence and Associativity](#operator-precedence-and-associativity)
3. [Arithmetic Operators](#arithmetic-operators)
4. [Assignment Operators](#assignment-operators)
5. [Comparison Operators](#comparison-operators)
6. [Logical Operators](#logical-operators)
7. [Identity Operators](#identity-operators)
8. [Membership Operators](#membership-operators)
9. [Bitwise Operators](#bitwise-operators)

## Understanding Operators

An **operator** is a symbol or keyword that instructs Python to perform a specific operation. The values or objects involved in that operation are called **operands**.

An **expression** is a combination of values, variables, operators, or function calls that Python evaluates to produce a result.

For example, in the expression `10 + 5`:

- **`10`** — The first operand.
- **`+`** — The arithmetic addition operator.
- **`5`** — The second operand.
- **`10 + 5`** — The complete expression, which evaluates to `15`.

Operators can be categorized by the number of operands they use:

- **Unary operators:** Operate on one operand, such as `-5` or `not True`.
- **Binary operators:** Operate on two operands, such as `10 + 5` or `10 > 5`.
- **Conditional expressions:** Use the syntax `value_if_true if condition else value_if_false` to choose between two values based on a condition. This is an expression form rather than a single operator symbol.

The following table summarizes the main operator categories covered in this guide.

| Category | Operators | Purpose |
|---|---|---|
| Arithmetic | `+`, `-`, `*`, `/`, `//`, `%`, `**` | Perform mathematical operations |
| Assignment | `=`, `+=`, `-=`, `*=`, `/=`, `//=`, `%=`, `**=`, and others | Assign or update variable references |
| Comparison | `==`, `!=`, `>`, `<`, `>=`, `<=` | Compare values |
| Logical | `and`, `or`, `not` | Combine or negate conditions |
| Identity | `is`, `is not` | Check whether references identify the same object |
| Membership | `in`, `not in` | Check whether a value belongs to a collection |
| Bitwise | `&`, `|`, `^`, `~`, `<<`, `>>` | Perform operations on integer bits |

### Operator Compatibility

Operators do not always require operands of the same data type. Whether an operation succeeds depends on the operator and the types of its operands.

Some operators support different data types directly, such as `"Hi" * 3`, which repeats a string three times. Others support implicit conversion between compatible numeric types, such as `10 + 2.5`, which produces `12.5`.

However, unsupported combinations, such as `"25" + 5`, raise a `TypeError`. Python does not automatically convert every incompatible value into a compatible type.

The following table demonstrates how operators behave with different combinations of data types.

| Expression | Operand Types | Valid? | Result |
|---|---|---|---|
| `10 + 5` | `int` + `int` | Yes | `15` |
| `10 + 2.5` | `int` + `float` | Yes | `12.5` |
| `10 - "5"` | `int` − `str` | No | `TypeError` |
| `10 == 10.0` | `int` and `float` | Yes | `True` |
| `"Hello" + "!"` | `str` + `str` | Yes | `"Hello!"` |
| `"25" + 5` | `str` + `int` | No | `TypeError` |
| `"Hi" * 3` | `str` × `int` | Yes | `"HiHiHi"` |
| `[1, 2] + [3]` | `list` + `list` | Yes | `[1, 2, 3]` |
| `[1, 2] + 3` | `list` + `int` | No | `TypeError` |
| `[1, 2] + "3"` | `list` + `str` | No | `TypeError` |
| `[1, 2] * 3` | `list` × `int` | Yes | `[1, 2, 1, 2, 1, 2]` |

> [!NOTE]
> **Operator compatibility is different from dynamic typing.** Dynamic typing allows a variable to reference objects of different types through reassignment, while operator compatibility determines whether an operation is supported for the operands involved.

### Operators and Operands

**Example 1: Performing arithmetic with two operands.** The `+` operator adds the two integers and produces a numeric result.

```python
number1 = 10
number2 = 5

result = number1 + number2

print(result)
```

**Output:**

```text
15
```

In this example, `number1` and `number2` reference the operands, while `+` performs the addition. The resulting value is assigned to `result`.

**Example 2: Operators behaving differently with different data types.** The `+` operator performs numeric addition with integers but concatenation with strings.

```python
print(10 + 5)
print("10" + "5")
```

**Output:**

```text
15
105
```

The first expression produces the integer `15`, while the second produces the string `"105"` by joining the characters.

**Example 3: Repeating a list using multiplication.** The `*` operator can multiply numbers, but when used with a list and an integer, it creates a new list by repeating the original list's elements the specified number of times.

```python
numbers = [1, 2, 3]
repetitions = 3

result = numbers * repetitions

print(result)
```

**Output:**

```text
[1, 2, 3, 1, 2, 3, 1, 2, 3]
```

In this example, `numbers` references a list and `repetitions` references an integer. The `*` operator repeats the list's elements three times rather than multiplying each individual number by `3`.

[🔼 Back to Overview](#overview)

## Operator Precedence and Associativity

When an expression contains multiple operators, Python follows a set of rules to determine how the expression is interpreted and which operations are grouped together.

These rules are called **operator precedence** and **operator associativity**.

- **Operator precedence:** Determines which operators take priority when an expression contains operators from different precedence levels. For example, multiplication (`*`) has higher precedence than addition (`+`).
- **Operator associativity:** Determines how operators at the same precedence level are grouped. Most arithmetic operators associate from left to right, while exponentiation (`**`) associates from right to left.
- **Parentheses (`()`):** Allow developers to explicitly group expressions and override the default precedence and associativity rules.
- **Evaluation order:** Determines when Python evaluates operands and subexpressions. Precedence controls how an expression is grouped, but it does not necessarily mean that every higher-precedence operand is evaluated before every lower-precedence operand.

### Operator Precedence

Operators have different precedence levels. Operators with higher precedence bind more tightly than operators with lower precedence.

For example, in `10 + 5 * 2`, multiplication has higher precedence than addition. Python groups the expression as `10 + (5 * 2)` rather than `(10 + 5) * 2`.

The following table summarizes Python's operator precedence, ordered from highest to lowest.

| Priority | Operators | Description |
|---|---|---|
| Highest | `()`, `[]`, function calls | Group expressions, access elements, and call functions |
| 1 | `**` | Exponentiation |
| 2 | `+value`, `-value`, `~value` | Unary positive, unary negative, and bitwise NOT |
| 3 | `*`, `/`, `//`, `%` | Multiplication, division, floor division, and modulus |
| 4 | `+`, `-` | Addition and subtraction |
| 5 | `<<`, `>>` | Bitwise shifts |
| 6 | `&` | Bitwise AND |
| 7 | `^` | Bitwise XOR |
| 8 | `\|` | Bitwise OR |
| 9 | `==`, `!=`, `<`, `<=`, `>`, `>=`, `is`, `is not`, `in`, `not in` | Comparison, identity, and membership |
| 10 | `not` | Logical NOT |
| 11 | `and` | Logical AND |
| 12 | `or` | Logical OR |
| Lowest | `value_if_true if condition else value_if_false` | Conditional expression |

> [!NOTE]
> This table covers the main operators introduced in this guide. Python also includes other expression forms, such as `await`, lambda expressions, and assignment expressions (`:=`), which have their own precedence rules.
>
> Assignment statements such as `x = 5` are not ordinary expressions and do not follow this table in the same way as arithmetic operators.

**Example 1: Multiplication before addition.** Multiplication (`*`) has higher precedence than addition (`+`), so Python groups the multiplication first.

```python
result = 10 + 5 * 2

print(result)
```

**Output:**

```text
20
```

Breaking down the calculation:

1. **Multiplication:** `5 * 2 = 10`
2. **Addition:** `10 + 10 = 20`

Therefore, `10 + 5 * 2` produces `20`, not `30`.

**Example 2: Multiple precedence levels.** Exponentiation (`**`) has higher precedence than multiplication (`*`), and multiplication has higher precedence than addition (`+`).

```python
result = 10 + 2 * 3 ** 2

print(result)
```

**Output:**

```text
28
```

Breaking down the calculation:

1. **Exponentiation:** `3 ** 2 = 9`
2. **Multiplication:** `2 * 9 = 18`
3. **Addition:** `10 + 18 = 28`

### Operator Associativity

**Associativity** determines how operators with the same precedence are grouped.

For most arithmetic operators, Python groups expressions from **left to right**. However, exponentiation (`**`) groups from **right to left**.

| Associativity | Operators | Grouping |
|---|---|---|
| Left to right | `+`, `-`, `*`, `/`, `//`, `%` | `(a operator b) operator c` |
| Right to left | `**` | `a ** (b ** c)` |

> [!IMPORTANT]
> Associativity describes how operations are **grouped**, not necessarily the order in which every operand is evaluated.

**Example 1: Left-to-right associativity.** Division (`/`) and multiplication (`*`) have equal precedence, so Python groups them from left to right.

```python
result = 20 / 5 * 2

print(result)
```

**Output:**

```text
8.0
```

Breaking down the calculation:

1. **Division:** `20 / 5 = 4.0`
2. **Multiplication:** `4.0 * 2 = 8.0`

Therefore, Python groups the expression as `(20 / 5) * 2`, not `20 / (5 * 2)`.

**Example 2: Right-to-left associativity.** Exponentiation (`**`) is right-associative, meaning consecutive exponentiation operations are grouped from right to left.

```python
result = 2 ** 3 ** 2

print(result)
```

**Output:**

```text
512
```

Breaking down the calculation:

1. **Rightmost exponentiation:** `3 ** 2 = 9`
2. **Remaining exponentiation:** `2 ** 9 = 512`

Therefore, Python groups the expression as `2 ** (3 ** 2)`, not `(2 ** 3) ** 2`.

### Using Parentheses to Control Precedence

**Parentheses (`()`)** allow us to explicitly group expressions rather than relying entirely on Python's default precedence and associativity rules.

Parenthesized expressions are treated as grouped units. This makes calculations easier to understand and allows us to change the result of an expression by changing its grouping.

**Example 1: Comparing expressions with and without parentheses.** By default, multiplication has higher precedence than addition. Parentheses allow us to group the addition first.

```python
result1 = 10 + 5 * 2
result2 = (10 + 5) * 2

print(result1)
print(result2)
```

**Output:**

```text
20
30
```

Breaking down the results:

- **Without parentheses:** `10 + (5 * 2) = 10 + 10 = 20`
- **With parentheses:** `(10 + 5) * 2 = 15 * 2 = 30`

**Example 2: Overriding associativity.** Parentheses can also change how operators at the same precedence level are grouped.

```python
result1 = 20 / 5 * 2
result2 = 20 / (5 * 2)

print(result1)
print(result2)
```

**Output:**

```text
8.0
2.0
```

The first expression groups from left to right, while the second uses parentheses to calculate `5 * 2` before dividing `20` by the result.

### Unary Operators and Exponentiation

Exponentiation (`**`) has a special interaction with unary negative (`-`) and unary positive (`+`) operators.

A unary operator operates on a single value, such as `-2`.

In Python, exponentiation binds more tightly than a unary operator on its **left**, but a unary operator on its **right** is included in the exponent.

This means:

- **`-2 ** 2`** is grouped as `-(2 ** 2)`, producing `-4`.
- **`(-2) ** 2`** uses parentheses to make `-2` the base, producing `4`.
- **`2 ** -2`** is grouped as `2 ** (-2)`, producing `0.25`.

**Example 1: Comparing negative bases and negative exponents.**

```python
print(-2 ** 2)
print((-2) ** 2)
print(2 ** -2)
```

**Output:**

```text
-4
4
0.25
```

Breaking down the results:

1. **`-2 ** 2`:** Calculate `2 ** 2 = 4`, then apply unary minus to produce `-4`.
2. **`(-2) ** 2`:** The parentheses establish `-2` as the base. Multiplying `-2 * -2` produces `4`.
3. **`2 ** -2`:** Apply the negative exponent rule: `1 / (2 ** 2) = 1 / 4 = 0.25`.

> [!IMPORTANT]
> When using negative numbers with exponentiation, parentheses help distinguish between negating the result and raising a negative base to a power.

### Evaluation Order

**Evaluation order** describes the sequence in which Python evaluates the parts of an expression.

Although precedence and associativity determine how an expression is grouped, Python generally evaluates the operands of an operator from **left to right**.

This distinction becomes important when operands contain function calls, because those functions may perform actions such as printing messages or modifying objects.

For example, even though multiplication has higher precedence in `a() + b() * c()`, Python evaluates the function calls in the order `a()`, then `b()`, then `c()`.

**Example 1: Evaluation order with function calls.** Each function prints a message before returning a number. This allows us to observe when Python evaluates each operand.

```python
def first():
    print("First")
    return 10

def second():
    print("Second")
    return 5

def third():
    print("Third")
    return 2

result = first() + second() * third()

print(result)
```

**Output:**

```text
First
Second
Third
20
```

The output demonstrates two separate behaviors:

- **Evaluation order:** Python calls `first()`, `second()`, and `third()` from left to right.
- **Operator precedence:** After obtaining the values, Python multiplies `5 * 2` before adding `10`, producing `20`.

Therefore, **precedence determines how operations are grouped, while evaluation order determines when operands are evaluated**.

[🔼 Back to Overview](#overview)

## Arithmetic Operators

**Arithmetic operators** perform mathematical operations on numeric values, such as addition, subtraction, multiplication, and division.

Python provides seven primary arithmetic operators:

| Operator | Name | Description | Example | Result |
|---|---|---|---|---|
| `+` | Addition | Adds two values | `10 + 3` | `13` |
| `-` | Subtraction | Subtracts the right operand from the left operand | `10 - 3` | `7` |
| `*` | Multiplication | Multiplies two values | `10 * 3` | `30` |
| `/` | Division | Divides the left operand by the right operand | `10 / 3` | `3.3333333333333335` |
| `//` | Floor division | Divides and rounds the quotient down toward negative infinity | `10 // 3` | `3` |
| `%` | Modulus | Returns the remainder from floor division | `10 % 3` | `1` |
| `**` | Exponentiation | Raises the left operand to the power of the right operand | `10 ** 3` | `1000` |

> [!IMPORTANT]
> **Division (`/`) and floor division (`//`) behave differently.**
>
> - `/` produces a floating-point result when dividing two integers, even when the division is exact. For example, `10 / 2` produces `5.0`.
> - `//` rounds the mathematical quotient down toward negative infinity, not toward zero. For example, `-10 // 3` produces `-4`, while `10 // 3` produces `3`.
> - `%` returns the remainder associated with floor division. For example, `10 % 3` produces `1`.
>
> Dividing by zero with `/`, `//`, or `%` raises a `ZeroDivisionError`.

### Addition, Subtraction, and Multiplication

The `+`, `-`, and `*` operators perform basic arithmetic using two numeric operands.

Although these operators commonly work with numbers, `+` and `*` also support certain operations involving sequences, such as string concatenation and list repetition.

**Example 1: Performing basic arithmetic.** We use addition, subtraction, and multiplication to calculate results from two integers.

```python
number1 = 10
number2 = 3

print(number1 + number2)
print(number1 - number2)
print(number1 * number2)
```

**Output:**

```text
13
7
30
```

Each expression uses the same two operands but produces a different result based on the operator.

**Example 2: Arithmetic with different numeric types.** Python supports operations between compatible numeric types, such as integers and floats. When an integer and a float are combined through these operations, the result is typically a float.

```python
whole_number = 10
decimal_number = 2.5

print(whole_number + decimal_number)
print(whole_number - decimal_number)
print(whole_number * decimal_number)
```

**Output:**

```text
12.5
7.5
25.0
```

### Division and Floor Division

Python provides two division operators with different behaviors:

- **`/` (Division):** Performs division and returns a floating-point result when both operands are integers.
- **`//` (Floor division):** Divides two numeric values and rounds the mathematical quotient down toward negative infinity. When both operands are integers, the result is an integer. If either operand is a float, the result is generally a float.

The distinction becomes particularly important when working with negative numbers. **Floor division does not simply remove the decimal portion of a result.** It rounds downward to the next integer when necessary.

**Example 1: Comparing division and floor division.**

```python
number1 = 10
number2 = 4

print(number1 / number2)
print(number1 // number2)
```

**Output:**

```text
2.5
2
```

The `/` operator returns the floating-point quotient, while `//` returns the floor of that quotient.

**Example 2: Dividing numbers evenly.** Even when two integers divide evenly, `/` returns a float, while `//` returns an integer.

```python
print(10 / 2)
print(10 // 2)
```

**Output:**

```text
5.0
5
```

**Example 3: Floor division with negative numbers.** Floor division (`//`) rounds the result down toward negative infinity rather than simply removing the decimal portion.

For example, `-20 / 3` is approximately `-6.67`. Because floor division rounds downward, Python produces `-7` rather than `-6`.

```python
print(-20 / 3)
print(-20 // 3)
```

**Output:**

```text
-6.666666666666667
-7
```

In this example:

1. **Regular division (`/`):** `-20 / 3` produces approximately `-6.67`.
2. **Floor division (`//`):** Python rounds the quotient down to the next lower integer, which is `-7`.
3. **Why not `-6`?** Although `-6` is closer to zero, it is greater than `-6.67`. Floor division rounds toward negative infinity, not toward zero.

Therefore, `-20 // 3` produces `-7` because `-7` is the greatest integer less than or equal to the actual quotient.

### Modulus (`%`)

The **modulus operator (`%`)** returns the remainder associated with floor division.

For integer operands, Python's division and modulus operations satisfy the following relationship:

`a == (a // b) * b + (a % b)`

This means the original dividend can be reconstructed using the floor-division quotient and the remainder.

A common use of modulus is determining whether a number is evenly divisible by another number. For example, `number % 2 == 0` checks whether an integer is even.

**Example 1: Finding a remainder.**

```python
number1 = 10
number2 = 4

print(number1 % number2)
```

**Output:**

```text
2
```

Dividing `10` by `4` gives a floor-division quotient of `2`, with `2` remaining.

**Example 2: Checking even and odd numbers.** An integer is even when dividing it by `2` produces a remainder of `0`. Otherwise, it is odd.

```python
number1 = 10
number2 = 7

print(number1 % 2)
print(number2 % 2)

print(number1 % 2 == 0)
print(number2 % 2 == 0)
```

**Output:**

```text
0
1
True
False
```

The comparison operator `==` checks whether each remainder equals `0`, producing a Boolean result.

**Example 3: Modulus with negative numbers.** When working with negative numbers, Python calculates the remainder using floor division (`//`), which rounds the quotient down toward negative infinity.

For example, `-10 / 3` is approximately `-3.33`. Floor division rounds this down to `-4`, rather than toward zero to `-3`.

The modulus operator (`%`) calculates the remainder using the following relationship:

`dividend = (quotient * divisor) + remainder`

```python
print(-10 // 3)
print(-10 % 3)
```

**Output:**

```text
-4
2
```

In this example, we know three of the four values:

- **Dividend:** `-10` — The number being divided.
- **Quotient:** `-4` — The result of floor division (`-10 // 3`).
- **Divisor:** `3` — The number we're dividing by.
- **Remainder:** Unknown — The value we're trying to calculate.

Substituting the known values gives us:

`-10 = (-4 * 3) + remainder`

We can now solve for the remainder:

1. **Perform multiplication:** `-10 = -12 + remainder`
2. **Add 12 to both sides:** `-10 + 12 = -12 + 12 + remainder`
3. **Simplify:** `2 = remainder`

Therefore, `-10 % 3` produces `2`.

We can verify the result by substituting the remainder back into the original equation:

`-10 = (-4 * 3) + 2` → `-10 = -12 + 2` → `-10 = -10`

### Exponentiation (`**`)

The **exponentiation operator (`**`)** raises a value to a specified power.

The left operand is called the **base**, and the right operand is called the **exponent**.

For example, `2 ** 3` means multiplying `2` by itself three times (`2 × 2 × 2`), producing `8`.

An exponent of `0` generally produces `1` for a nonzero base, while a negative exponent calculates the reciprocal of the corresponding positive power.

> [!NOTE]
> In Python, `**` is exponentiation, while `^` is the bitwise XOR operator. They perform different operations.

**Example 1: Raising numbers to a power.**

```python
print(2 ** 3)
print(5 ** 2)
print(10 ** 0)
```

**Output:**

```text
8
25
1
```

**Example 2: Using a negative exponent.** A negative exponent tells us to divide `1` by the base raised to the corresponding positive exponent, rather than multiplying by a negative number.

This is called taking the **reciprocal**. A reciprocal is `1` divided by a nonzero number.

To calculate a negative exponent, we can use the following formula:

`answer = 1 / (base ** positive_exponent)`

Where:

- **`answer`** — The result of the calculation.
- **`1`** — The numerator of the fraction.
- **`base`** — The number being raised to a power.
- **`positive_exponent`** — The positive version of the original negative exponent.

> [!NOTE]
> A negative exponent does not mean the result will be negative. It tells us to take the reciprocal of the corresponding positive power.
>
> The formula uses the **positive version of the exponent** because we're already accounting for its negative sign by taking the reciprocal. Using `-3` inside the denominator would incorrectly apply the reciprocal twice.

For example, `2 ** -3` has a base of `2` and a negative exponent of `-3`. Applying the negative exponent rule means taking the reciprocal of `2` raised to the positive power `3`. This gives us `1 / (2 ** 3)`, which produces `0.125`.

```python
print(2 ** -3)
```

**Output:**

```text
0.125
```

Breaking down the calculation:

1. **Apply the negative exponent rule:** `answer = 1 / (2 ** 3)`
2. **Calculate the exponent:** `2 ** 3 = 2 * 2 * 2 = 8`
3. **Substitute the result:** `answer = 1 / 8`
4. **Perform division:** `answer = 0.125`

Therefore, `2 ** -3` produces `0.125`.

### Unary Positive and Negative Operators

The `+` and `-` symbols can also be used as **unary operators**, meaning they operate on a single operand rather than two.

- **Unary plus (`+value`):** Returns the numeric value without changing its sign. For example, `+10` produces `10`, while `+(-10)` produces `-10`.
- **Unary minus (`-value`):** Negates a numeric value, reversing its sign. For example, `-10` produces `-10`, while `-(-10)` produces `10`.

Unlike addition or subtraction, unary operators require only one operand.

**Example 1: Applying unary operators.** We use unary plus and unary minus with both positive and negative values.

```python
number = 10
negative_number = -10

print(+number)
print(-number)
print(+negative_number)
print(-negative_number)
```

**Output:**

```text
10
-10
-10
10
```

The results demonstrate that unary plus preserves the value's sign, while unary minus reverses it.

### Division by Zero

Division by zero is undefined for ordinary Python numeric arithmetic.

Attempting to use a zero divisor with `/`, `//`, or `%` raises a `ZeroDivisionError`. Raising zero to a negative power, such as `0 ** -1`, also raises this exception because a negative exponent requires calculating a reciprocal, which would involve division by zero.

**Example 1: Attempting to divide by zero.**

```python
number = 10

print(number / 0)
```

**Output (error):**

```text
ZeroDivisionError: division by zero
```

[🔼 Back to Overview](#overview)

## Assignment Operators

**Assignment operators** associate variable names with objects or update existing variable references using the results of operations.

Python provides two main forms of assignment:

- **Regular assignment (`=`):** Associates a variable name with an object. If the variable already exists, reassignment changes which object it references.
- **Augmented assignment (`+=`, `-=`, `*=`, etc.):** Combines an operation with assignment. Depending on the object's type, it may modify the existing object in place or produce a new object and reassign the variable.

### Assignment Operator Reference

The following table summarizes common assignment operators.

| Operator | Name | Example | Equivalent Operation for Integers |
|---|---|---|---|
| `=` | Assignment | `x = 5` | Assigns `5` to `x` |
| `+=` | Addition assignment | `x += 5` | `x = x + 5` |
| `-=` | Subtraction assignment | `x -= 5` | `x = x - 5` |
| `*=` | Multiplication assignment | `x *= 5` | `x = x * 5` |
| `/=` | Division assignment | `x /= 5` | `x = x / 5` |
| `//=` | Floor division assignment | `x //= 5` | `x = x // 5` |
| `%=` | Modulus assignment | `x %= 5` | `x = x % 5` |
| `**=` | Exponentiation assignment | `x **= 5` | `x = x ** 5` |
| `&=` | Bitwise AND assignment | `x &= 5` | `x = x & 5` |
| `\|=` | Bitwise OR assignment | `x \|= 5` | `x = x \| 5` |
| `^=` | Bitwise XOR assignment | `x ^= 5` | `x = x ^ 5` |
| `<<=` | Left shift assignment | `x <<= 2` | `x = x << 2` |
| `>>=` | Right shift assignment | `x >>= 2` | `x = x >> 2` |

> [!NOTE]
> The equivalent operations in the table describe their behavior for integers. Augmented assignment is not always identical to writing `x = x + value`, particularly for mutable objects.
>
> Bitwise assignment operators will become clearer when we cover Bitwise Operators later in this guide.

### Regular Assignment (`=`)

The **assignment operator (`=`)** associates a variable name with an object.

If the variable already references an object, assigning another value reassigns the name. This does not modify the original object.

> [!IMPORTANT]
> Assignment (`=`) is different from equality comparison (`==`).
>
> - `=` assigns an object to a variable name.
> - `==` compares two values and returns a Boolean result.

**Example 1: Assigning and reassigning a variable.** We assign an integer to `number`, then reassign the variable to reference another integer.

```python
number = 10

print(number)

number = 20

print(number)
```

**Output:**

```text
10
20
```

After reassignment, `number` references the integer `20` instead of `10`.

### Augmented Assignment

**Augmented assignment** combines an operation with assignment using a single operator.

For example, `number += 5` adds `5` to the current value referenced by `number` and assigns the result back to `number`.

For immutable numeric objects, this behaves like writing `number = number + 5`.

Augmented assignment is useful for updating counters, accumulating totals, changing quantities, and performing repeated calculations.

**Example 1: Using addition and subtraction assignment.** The `+=` and `-=` operators update a variable using its current value.

```python
number = 10

number += 5
print(number)

number -= 3
print(number)
```

**Output:**

```text
15
12
```

The first operation produces `15`. The second subtracts `3` from that result, producing `12`.

**Example 2: Using multiplication and division assignment.** The `*=` and `/=` operators apply arithmetic operations and assign the results back to the variable.

```python
number = 10

number *= 3
print(number)

number /= 2
print(number)
print(type(number))
```

**Output:**

```text
30
15.0
<class 'float'>
```

The `/=` operation produces a floating-point result, so `number` references a `float` after division.

**Example 3: Using floor division, modulus, and exponentiation assignment.** These operators use the same arithmetic rules as `//`, `%`, and `**`.

```python
number = 10

number //= 3
print(number)

number %= 2
print(number)

number **= 3
print(number)
```

**Output:**

```text
3
1
1
```

Each operation uses the result of the previous assignment.

### Augmented Assignment and Mutable Objects

Augmented assignment can behave differently depending on whether the object is mutable or immutable.

For immutable objects such as integers and strings, augmented assignment generally produces a new object and reassigns the variable.

For mutable objects such as lists, certain augmented assignment operations can modify the existing object directly.

This distinction is particularly important when multiple variables reference the same mutable object.

**Example 1: Using `+=` with an integer.** Integers are immutable, so `+=` produces another integer object and reassigns the variable.

```python
number = 10
number += 5

print(number)
```

**Output:**

```text
15
```

**Example 2: Using `+=` with a list.** Lists are mutable, and list augmented addition extends the existing list in place rather than creating a replacement list.

```python
numbers = [1, 2, 3]
numbers += [4, 5]

print(numbers)
```

**Output:**

```text
[1, 2, 3, 4, 5]
```

Unlike regular list concatenation with `+`, the `+=` operation modifies the existing list.

**Example 3: Comparing `+` and `+=` with shared references.** Two variables can reference the same list object. When one variable is reassigned using `+`, the other continues referencing the original list. When `+=` modifies the list in place, both variables observe the change.

```python
numbers = [1, 2, 3]
other_numbers = numbers

numbers = numbers + [4]

print(numbers)
print(other_numbers)

numbers = other_numbers
numbers += [5]

print(numbers)
print(other_numbers)
```

**Output:**

```text
[1, 2, 3, 4]
[1, 2, 3]
[1, 2, 3, 5]
[1, 2, 3, 5]
```

Breaking down the behavior:

1. **Shared reference:** `other_numbers = numbers` associates both variable names with the original list.
2. **Regular concatenation:** `numbers = numbers + [4]` creates a new list and reassigns `numbers`. `other_numbers` still references the original list.
3. **Shared reference restored:** `numbers = other_numbers` makes both variables reference the same original list again.
4. **In-place modification:** `numbers += [5]` extends that list directly, so both variables display the updated contents.

> [!IMPORTANT]
> Augmented assignment should not always be interpreted as shorthand for regular assignment.
>
> With mutable objects, operations such as list `+=` may modify an existing object in place, affecting every variable that references it.

[🔼 Back to Overview](#overview)

## Comparison Operators

**Comparison operators** compare two values to determine how they relate to each other. The result is a Boolean (`True` or `False`), which can be used to evaluate conditions, validate values, and control program behavior.

Python provides six primary comparison operators:

| Operator | Name | Description | Example | Result |
|---|---|---|---|---|
| `==` | Equal to | Checks whether two values are equal | `10 == 10` | `True` |
| `!=` | Not equal to | Checks whether two values are different | `10 != 5` | `True` |
| `>` | Greater than | Checks whether the left operand is greater than the right | `10 > 5` | `True` |
| `<` | Less than | Checks whether the left operand is less than the right | `10 < 5` | `False` |
| `>=` | Greater than or equal to | Checks whether the left operand is greater than or equal to the right | `10 >= 10` | `True` |
| `<=` | Less than or equal to | Checks whether the left operand is less than or equal to the right | `10 <= 5` | `False` |

> [!IMPORTANT]
> **Assignment (`=`) and equality comparison (`==`) are different operations.**
>
> - **`=`** associates a variable name with an object.
> - **`==`** compares two values to determine whether they are equal.
>
> Comparison operators do not always require operands of the same data type. For example, `10 == 10.0` returns `True` because the numeric values are equal. However, some combinations of incompatible types raise a `TypeError`.

### Equality and Inequality Operators

The **equality operator (`==`)** checks whether two values are equal, while the **inequality operator (`!=`)** checks whether they are different.

Both operators return a Boolean result:

- **`==`** returns `True` when the values are equal and `False` when they are not.
- **`!=`** returns `True` when the values are different and `False` when they are equal.

Equality depends on how the objects' types define comparison. Different types can sometimes compare as equal, particularly compatible numeric types.

For example, `25 == 25.0` returns `True` because the integer and float represent the same numeric value. However, `25 == "25"` returns `False` because Python does not automatically convert the string to an integer.

**Example 1: Comparing equal and unequal integers.** We use `==` and `!=` to compare two integer values.

```python
number1 = 10
number2 = 5

print(number1 == number2)
print(number1 != number2)
```

**Output:**

```text
False
True
```

The values are not equal, so `==` returns `False`, while `!=` returns `True`.

**Example 2: Comparing different numeric types.** Python allows equality comparisons between compatible numeric types, such as integers and floats.

```python
number1 = 25
number2 = 25.0

print(number1 == number2)
print(number1 != number2)
print(type(number1) == type(number2))
```

**Output:**

```text
True
False
False
```

Although `number1` and `number2` have equal numeric values, their data types are different. The final comparison returns `False` because `int` and `float` are not the same type.

**Example 3: Comparing a number and a numeric string.** Python does not automatically convert a string to a number during equality comparisons.

```python
number = 25
text = "25"

print(number == text)
print(number != text)
```

**Output:**

```text
False
True
```

Although both values appear to represent `25`, one is an integer and the other is a string. They do not compare as equal.

### Greater Than and Less Than Operators

The **greater than (`>`)** and **less than (`<`)** operators determine whether one value is larger or smaller than another.

The operators `>=` and `<=` also include equality:

- **`>` (Greater than):** Returns `True` when the left operand is larger than the right operand.
- **`<` (Less than):** Returns `True` when the left operand is smaller than the right operand.
- **`>=` (Greater than or equal to):** Returns `True` when the left operand is larger than or equal to the right operand.
- **`<=` (Less than or equal to):** Returns `True` when the left operand is smaller than or equal to the right operand.

These comparisons are commonly used to check limits, minimum requirements, maximum values, and acceptable ranges.

**Example 1: Comparing two numbers.** We use all four operators to compare an integer against another value.

```python
number = 10

print(number > 5)
print(number < 5)
print(number > 10)
print(number < 10)
print(number >= 10)
print(number <= 10)
```

**Output:**

```text
True
False
False
False
True
True
```

Both `>=` and `<=` return `True` when the operands are equal because these operators include equality.

**Example 2: Checking a minimum requirement.** A practical use of comparison operators is checking whether a value meets a required threshold.

```python
age = 18
minimum_age = 18

print(age >= minimum_age)
```

**Output:**

```text
True
```

The result is `True` because `age` meets the minimum requirement exactly.

### Comparing Strings

Python also supports comparison operators with strings.

Strings are compared **lexicographically**, meaning Python examines their characters from left to right using their Unicode code point values.

A **Unicode code point** is a numeric value assigned to a character. Python uses these values to determine how individual characters compare.

Important rules include:

- **Case sensitivity:** Uppercase and lowercase letters are different characters. For example, `"A" == "a"` returns `False`.
- **Character order:** Python compares corresponding characters from left to right until it finds a difference.
- **String length:** If all compared characters match but one string ends earlier, the shorter string is considered smaller.
- **Unicode order:** Comparisons are based on Unicode values, not necessarily alphabetical or dictionary ordering.

Python provides the built-in `ord()` function to retrieve a character's Unicode code point as an integer. This can help explain why one character compares as greater than or less than another.

> [!NOTE]
> Python's default string comparisons are case-sensitive and do not automatically follow human alphabetical sorting conventions. For example, uppercase letters `A` through `Z` have lower Unicode code points than lowercase letters `a` through `z`.

**Example 1: Comparing identical and different strings.** Equality comparisons check whether the strings contain equal sequences of characters.

```python
username = "Alice"

print(username == "Alice")
print(username == "alice")
print(username != "Bob")
```

**Output:**

```text
True
False
True
```

The second comparison returns `False` because `"Alice"` and `"alice"` differ in capitalization.

**Example 2: Comparing strings alphabetically.** Python compares characters from left to right using their Unicode values.

```python
print("Apple" < "Banana")
print("Cat" > "Car")
print("Apple" < "Apples")
```

**Output:**

```text
True
True
True
```

Breaking down the comparisons:

1. **`"Apple" < "Banana"`:** The first characters differ. `"A"` has a lower Unicode value than `"B"`, so the result is `True`.
2. **`"Cat" > "Car"`:** The first two characters match (`"Ca"`), but `"t"` has a higher Unicode value than `"r"`, so the result is `True`.
3. **`"Apple" < "Apples"`:** All characters in `"Apple"` match the beginning of `"Apples"`. Because `"Apple"` is shorter, it compares as smaller.

**Example 3: Understanding case sensitivity with `ord()`.** We use `ord()` to inspect the Unicode code points of uppercase and lowercase letters.

```python
print(ord("A"))
print(ord("a"))

print("A" < "a")
```

**Output:**

```text
65
97
True
```

The result is `True` because uppercase `"A"` has a lower Unicode code point (`65`) than lowercase `"a"` (`97`).

### Chained Comparisons

Python supports **chained comparisons**, allowing multiple comparison operations to be written in a single expression.

For example, `18 <= age < 65` checks whether `age` is at least `18` and less than `65`.

This is equivalent in meaning to:

`18 <= age and age < 65`

The logical `and` operator requires both comparisons to be true for the overall condition to succeed.

Chained comparisons have two useful characteristics:

- **Shared operands:** The middle value participates in both comparisons without needing to repeat it.
- **Short-circuit evaluation:** Python stops evaluating the chain once a comparison is false, and a shared operand is evaluated at most once.

> [!NOTE]
> Chained comparisons are especially useful when checking whether a value falls within a range. They also work with other comparison operators, such as `==` and `!=`.

**Example 1: Checking whether a value is within a range.** We use a chained comparison to verify that `age` is at least `18` but less than `65`.

```python
age = 25

print(18 <= age < 65)
```

**Output:**

```text
True
```

Both comparisons are true because `25` is greater than or equal to `18` and less than `65`.

**Example 2: Comparing values outside a range.** We use the same comparison with different values to demonstrate how its result changes.

```python
age1 = 16
age2 = 25
age3 = 70

print(18 <= age1 < 65)
print(18 <= age2 < 65)
print(18 <= age3 < 65)
```

**Output:**

```text
False
True
False
```

Only `age2` falls within the accepted range.

### Common Comparison Mistakes

Comparison operators can behave unexpectedly when different data types are involved or when similar-looking operators are confused.

Some important distinctions include:

- **`=` versus `==`:** Assignment and equality comparison perform different operations.
- **Value equality versus type equality:** Two values may compare as equal even when their data types differ.
- **Incompatible ordering comparisons:** Operators such as `<` and `>` generally cannot compare unrelated types, such as an integer and a string.
- **Floating-point precision:** Some decimal values cannot be represented exactly as binary floating-point numbers, so direct equality comparisons may produce unexpected results.

**Example 1: Comparing incompatible types.** Equality between an integer and a numeric string is valid and typically returns `False`, but ordering comparisons between those types raise a `TypeError`.

```python
number = 10
text = "10"

print(number < text)
```

**Output (error):**

```text
TypeError: '<' not supported between instances of 'int' and 'str'
```

Python raises the error because it does not define an ordering comparison between `int` and `str`.

**Example 2: Comparing floating-point calculations.** Python uses binary floating-point representation for `float` values. Some decimal fractions cannot be represented exactly, which can affect equality comparisons.

```python
result = 0.1 + 0.2

print(result)
print(result == 0.3)
```

**Output:**

```text
0.30000000000000004
False
```

The result is slightly different from the floating-point value represented by `0.3`, so the exact equality comparison returns `False`.

> [!IMPORTANT]
> When comparing floating-point calculations, exact equality is not always appropriate. Python provides functions such as `math.isclose()` for comparing whether numeric values are sufficiently close within a specified tolerance.
>
> We'll explore floating-point precision in more detail when covering numbers.

[🔼 Back to Overview](#overview)

## Logical Operators

**Logical operators** combine, evaluate, or reverse conditions. They are commonly used when a program needs to make decisions based on multiple requirements.

Python provides three logical operators:

| Operator | Name | Description | Example | Result |
|---|---|---|---|---|
| `and` | Logical AND | Returns the first falsy operand, or the last operand if all are truthy | `True and False` | `False` |
| `or` | Logical OR | Returns the first truthy operand, or the last operand if all are falsy | `True or False` | `True` |
| `not` | Logical NOT | Reverses a value's truthiness and returns a Boolean | `not True` | `False` |

When logical operators are used with Boolean values, their results follow these rules:

- **`and`:** Both conditions must be `True` for the result to be `True`.
- **`or`:** At least one condition must be `True` for the result to be `True`.
- **`not`:** Reverses `True` to `False`, or `False` to `True`.

> [!IMPORTANT]
> The `and` and `or` operators do not always return Boolean values. They return one of their original operands based on truthiness. The `not` operator always returns `True` or `False`.

### Logical AND (`and`)

The **`and` operator** evaluates whether both conditions are true.

If the first operand is falsy, Python returns it without evaluating the second operand. Otherwise, Python evaluates and returns the second operand.

This behavior makes `and` useful when multiple conditions must be satisfied.

The following truth table shows how `and` behaves with Boolean operands.

| First Condition | Second Condition | Result |
|---|---|---|
| `True` | `True` | `True` |
| `True` | `False` | `False` |
| `False` | `True` | `False` |
| `False` | `False` | `False` |

**Example 1: Checking multiple requirements.** We use `and` to verify that a person meets a minimum age requirement and has an active membership.

```python
age = 25
membership_active = True

print(age >= 18 and membership_active)
```

**Output:**

```text
True
```

Both conditions are true because `25` meets the minimum age requirement and `membership_active` is `True`.

**Example 2: One requirement is not satisfied.** Both conditions must be true, so a single false condition makes the overall result false.

```python
age = 16
membership_active = True

print(age >= 18 and membership_active)
```

**Output:**

```text
False
```

The age requirement is not satisfied, so the result is `False`.

### Logical OR (`or`)

The **`or` operator** evaluates whether at least one condition is true.

If the first operand is truthy, Python returns it without evaluating the second operand. Otherwise, Python evaluates and returns the second operand.

This makes `or` useful when several alternative conditions can satisfy a requirement.

| First Condition | Second Condition | Result |
|---|---|---|
| `True` | `True` | `True` |
| `True` | `False` | `True` |
| `False` | `True` | `True` |
| `False` | `False` | `False` |

**Example 1: Checking alternative requirements.** We use `or` to allow access when a user is either an administrator or the owner of a resource.

```python
is_admin = False
is_owner = True

print(is_admin or is_owner)
```

**Output:**

```text
True
```

The result is `True` because the user satisfies the ownership condition, even though they are not an administrator.

**Example 2: Neither requirement is satisfied.** If both operands are false, `or` returns `False`.

```python
is_admin = False
is_owner = False

print(is_admin or is_owner)
```

**Output:**

```text
False
```

### Logical NOT (`not`)

The **`not` operator** reverses the truthiness of its operand and always produces a Boolean result.

Unlike `and` and `or`, `not` is a **unary operator**, meaning it uses only one operand.

| Operand | Result |
|---|---|
| `True` | `False` |
| `False` | `True` |

**Example 1: Reversing a Boolean condition.** We use `not` to check whether an account is inactive.

```python
is_active = False

print(not is_active)
```

**Output:**

```text
True
```

Because `is_active` is `False`, applying `not` produces `True`.

### Truthiness and Logical Operators

**Truthiness** describes whether Python treats a value as true or false when evaluating a condition.

Values commonly considered falsy include:

- `False`
- `None`
- Numeric zero, such as `0` and `0.0`
- Empty strings, such as `""`
- Empty collections, such as `[]`, `()`, `{}`, and `set()`

Most other values are truthy, including nonzero numbers, nonempty strings, and nonempty collections.

When used with non-Boolean values:

- **`and`** returns the first falsy operand, or the last operand if none are falsy.
- **`or`** returns the first truthy operand, or the last operand if none are truthy.
- **`not`** converts the operand's truthiness into the opposite Boolean value.

**Example 1: Logical operators with non-Boolean values.**

```python
print(0 or 25)
print("" or "Guest")
print("Hello" and "World")
print(not [])
```

**Output:**

```text
25
Guest
World
True
```

Breaking down the results:

1. **`0 or 25`:** `0` is falsy, so Python returns `25`.
2. **`"" or "Guest"`:** The empty string is falsy, so Python returns `"Guest"`.
3. **`"Hello" and "World"`:** The first string is truthy, so Python returns the second operand, `"World"`.
4. **`not []`:** An empty list is falsy, so `not` produces `True`.

### Short-Circuit Evaluation

**Short-circuit evaluation** means Python stops evaluating a logical expression once it has enough information to determine which operand to return.

This applies to `and` and `or`:

- **`and`:** Stops when it encounters a falsy operand.
- **`or`:** Stops when it encounters a truthy operand.

Short-circuit evaluation is useful for avoiding unnecessary calculations or preventing operations that would fail under certain conditions.

**Example 1: Avoiding division by zero.** We check that the divisor is not zero before attempting division.

```python
number = 10
divisor = 0

print(divisor != 0 and number / divisor > 2)
```

**Output:**

```text
False
```

The first comparison is `False`, so Python does not evaluate `number / divisor > 2`. This prevents a `ZeroDivisionError`.

**Example 2: Short-circuit evaluation using `or`.** The `or` operator stops evaluating when it encounters a truthy operand because only one condition needs to be true.

For example, a user may be allowed to access a resource if they are an administrator or have been granted permission. We can use a function to check the user's permissions, but that check is unnecessary if the user is already an administrator.

The function `check_permission()` displays a message when called and returns a Boolean indicating whether the user has permission.

```python
is_admin = True

def check_permission():
    print("Checking user permissions...")
    return False

has_access = is_admin or check_permission()

print(has_access)
```

**Output:**

```text
True
```

In this example:

1. **First condition:** `is_admin` is `True`.
2. **Short-circuit:** Python already knows that at least one condition is true, so it does not call `check_permission()`.
3. **Result:** `has_access` is assigned `True`.

Notice that `"Checking user permissions..."` never appears in the output. This confirms that Python skipped the function call.

If `is_admin` were `False`, Python would call `check_permission()` to evaluate the second condition.

[🔼 Back to Overview](#overview)

## Identity Operators

**Identity operators** determine whether two references identify the exact same object, rather than merely checking whether their values are equal.

Python provides two identity operators:

| Operator | Name | Description | Example |
|---|---|---|---|
| `is` | Identity | Returns `True` when both references identify the same object | `a is b` |
| `is not` | Non-identity | Returns `True` when the references identify different objects | `a is not b` |

The distinction between **equality** and **identity** is important:

- **Equality (`==`):** Checks whether two objects have equal values.
- **Identity (`is`):** Checks whether two references identify the exact same object.
- **Non-identity (`is not`):** Checks whether two references identify different objects.

Two separate objects can contain equal values without being the same object.

### Object Identity and References

Every Python object has an **identity** that uniquely distinguishes it from other objects during its lifetime.

When Python creates an object, it allocates memory to represent that object. Variables reference objects rather than directly containing their values. Multiple variables can reference the same object, while separate objects can contain identical values.

This distinction is important when comparing objects:

- **Equality (`==`):** Checks whether two objects have equal values, even when they are separate objects in memory.
- **Identity (`is`):** Checks whether two references identify the exact same object.
- **Non-identity (`is not`):** Checks whether two references identify different objects.

Python provides two useful functions for inspecting object identity:

- **`id(object)`** — Returns an integer that uniquely identifies an object during its lifetime.
- **`hex(number)`** — Converts an integer into a hexadecimal (base-16) string. Hexadecimal uses the digits `0–9` and letters `a–f` and is commonly used to display memory addresses. The resulting string begins with `0x`.

> [!NOTE]
> In **CPython**, the most common Python implementation, `id()` typically represents an object's memory address. However, Python does not guarantee that other implementations use memory addresses for object identity.
>
> Two references to the same object have identical `id()` values. Two different objects that exist simultaneously have different `id()` values.
>
> An object's identity remains unchanged during its lifetime, even if the object's contents are modified. However, an identifier may be reused after an object no longer exists.

**Example 1: Comparing equal values and object identity.** We create two separate lists containing the same elements, then assign a third variable to reference the first list.

```python
numbers1 = [1, 2, 3]
numbers2 = [1, 2, 3]
numbers3 = numbers1

print(numbers1 == numbers2)
print(numbers1 is numbers2)
print(numbers1 is numbers3)
```

**Output:**

```text
True
False
True
```

Breaking down the results:

1. **`numbers1 == numbers2`:** Returns `True` because the lists contain equal elements.
2. **`numbers1 is numbers2`:** Returns `False` because they are separate list objects.
3. **`numbers1 is numbers3`:** Returns `True` because both names reference the same list.

**Example 2: Inspecting and comparing object identities using `id()`.** We use `id()` to inspect each object's identifier and `hex()` to display the identifier in hexadecimal notation.

```python
numbers1 = [1, 2, 3]
numbers2 = numbers1
numbers3 = [1, 2, 3]

print(hex(id(numbers1)))
print(hex(id(numbers2)))
print(hex(id(numbers3)))

print(id(numbers1) == id(numbers2))
print(id(numbers1) == id(numbers3))
```

**Example output (identifiers will vary):**

```text
0x7f1234501000
0x7f1234501000
0x7f1234502000
True
False
```

Breaking down the results:

1. **`hex(id(numbers1))`:** Displays the identity of the original list in hexadecimal notation.
2. **`hex(id(numbers2))`:** Displays the same identifier because `numbers2` references the original list.
3. **`hex(id(numbers3))`:** Displays a different identifier because `numbers3` references a separate list.
4. **`id(numbers1) == id(numbers2)`:** Returns `True` because both variables reference the same object.
5. **`id(numbers1) == id(numbers3)`:** Returns `False` because the variables reference different objects.

### Identity and Mutable Objects

When multiple variables reference the same mutable object, modifications made through one variable are visible through the other references.

This differs from reassignment, which changes only the association between a particular variable name and an object.

**Example 1: Modifying a shared list.**

```python
numbers1 = [1, 2, 3]
numbers2 = numbers1

numbers2.append(4)

print(numbers1)
print(numbers2)
print(numbers1 is numbers2)
```

**Output:**

```text
[1, 2, 3, 4]
[1, 2, 3, 4]
True
```

Both variables reference the same list, so the modification is visible through either name.

### Comparing Values with `None`

The `None` object represents the absence of a value.

Python provides exactly one `None` object, making identity comparison the recommended way to check whether a value is `None`.

Use:

- **`value is None`** — Checks whether the value is `None`.
- **`value is not None`** — Checks whether the value is anything other than `None`.

> [!IMPORTANT]
> Use `is` and `is not` when checking against `None`.
>
> For ordinary value comparisons, use `==` and `!=` instead. Python may reuse certain immutable objects internally, so identity comparisons between numbers or strings should not be used as substitutes for value equality.

**Example 1: Checking whether a value is `None`.**

```python
middle_name = None
first_name = "Alice"

print(middle_name is None)
print(first_name is not None)
```

**Output:**

```text
True
True
```

The first comparison confirms that `middle_name` references `None`, while the second confirms that `first_name` references a different object.

[🔼 Back to Overview](#overview)

## Membership Operators

**Membership operators** determine whether a value is present in a collection or sequence.

Python provides two membership operators:

| Operator | Name | Description | Example | Result |
|---|---|---|---|---|
| `in` | Membership | Returns `True` when a value is found | `3 in [1, 2, 3]` | `True` |
| `not in` | Non-membership | Returns `True` when a value is not found | `4 not in [1, 2, 3]` | `True` |

Membership operators commonly work with lists, tuples, strings, sets, and dictionaries.

Their behavior depends on the collection type:

- **Lists and tuples:** Check whether an element is present.
- **Strings:** Check whether a substring appears within the string.
- **Sets:** Check whether an element belongs to the set.
- **Dictionaries:** Check whether a key exists, not whether a value exists.

> [!NOTE]
> Membership operators return Boolean values (`True` or `False`). They do not return the matching element or its index.

### Membership in Lists and Tuples

For lists and tuples, membership operators check whether an element is present by comparing values.

**Example 1: Checking list membership.**

```python
numbers = [10, 20, 30]

print(20 in numbers)
print(40 in numbers)
print(40 not in numbers)
```

**Output:**

```text
True
False
True
```

The value `20` exists in the list, while `40` does not.

### Membership in Strings

For strings, `in` checks whether a character or substring appears within another string.

String membership is **case-sensitive**, meaning uppercase and lowercase characters are treated differently.

**Example 1: Checking whether a substring exists.**

```python
message = "Hello World"

print("Hello" in message)
print("World" in message)
print("world" in message)
```

**Output:**

```text
True
True
False
```

The lowercase substring `"world"` is not found because the original string contains uppercase `"World"`.

### Membership in Sets

Sets contain unique elements and support membership testing without requiring positional indexing.

Python's built-in sets use hash-based storage to locate elements efficiently.

**Example 1: Checking set membership.**

```python
numbers = {10, 20, 30}

print(20 in numbers)
print(40 not in numbers)
```

**Output:**

```text
True
True
```

The first comparison finds `20` in the set. The second confirms that `40` is absent.

### Membership in Dictionaries

Dictionaries associate **keys** with values.

When using `in` or `not in` directly with a dictionary, Python checks its keys rather than its values.

To check whether a value exists, we can use the dictionary's `.values()` method, which returns a view of its values.

**Example 1: Checking dictionary keys and values.**

```python
user = {
    "name": "Alice",
    "age": 25
}

print("name" in user)
print("Alice" in user)
print("Alice" in user.values())
```

**Output:**

```text
True
False
True
```

Breaking down the results:

1. **`"name" in user`:** Returns `True` because `"name"` is a dictionary key.
2. **`"Alice" in user`:** Returns `False` because `"Alice"` is a value rather than a key.
3. **`"Alice" in user.values()`:** Returns `True` because the value exists in the dictionary.

[🔼 Back to Overview](#overview)

## Bitwise Operators

**Bitwise operators** perform operations on the individual bits of integer values.

A **bit**, short for *binary digit*, is the smallest unit of binary information and can have one of two values: `0` or `1`.

Computers use binary to represent information because digital hardware can distinguish between two states, commonly represented as off (`0`) and on (`1`).

Python provides six primary bitwise operators:

| Operator | Name | Description | Example | Result |
|---|---|---|---|---|
| `&` | Bitwise AND | Produces `1` when both corresponding bits are `1` | `5 & 3` | `1` |
| `\|` | Bitwise OR | Produces `1` when at least one corresponding bit is `1` | `5 \| 3` | `7` |
| `^` | Bitwise XOR | Produces `1` when corresponding bits are different | `5 ^ 3` | `6` |
| `~` | Bitwise NOT | Inverts the bits of an integer | `~5` | `-6` |
| `<<` | Left shift | Shifts bits to the left | `5 << 1` | `10` |
| `>>` | Right shift | Shifts bits to the right | `5 >> 1` | `2` |

Unlike logical operators, which evaluate the truthiness of values, bitwise operators work with their binary representation.

For example:

- **Logical AND (`and`):** Evaluates whether conditions are truthy.
- **Bitwise AND (`&`):** Compares individual bits and calculates an integer result.

> [!IMPORTANT]
> Bitwise operators work primarily with integers, including Python's `bool` type, which is a subclass of `int`.
>
> Bitwise operations are commonly used when working with flags, permissions, networking, binary data, and hardware.

### Binary Representation in Python

Before working with bitwise operators, it's important to understand how Python represents integers in binary.

**Binary (base 2)** is a number system that uses only two digits: `0` and `1`. Each digit is called a **bit**, and each position represents a power of `2`.

Unlike the decimal system (base 10), which uses place values of `1`, `10`, `100`, and so on, binary uses place values of `1`, `2`, `4`, `8`, `16`, and so on.

For example, the decimal number `5` is represented as binary `101`:

| Binary Digit | 1 | 0 | 1 |
|---|---|---|---|
| Place Value | 4 | 2 | 1 |
| Contribution | 4 | 0 | 1 |

Adding the contributions gives us:

`4 + 0 + 1 = 5`

Therefore, binary `101` represents decimal `5`.

Python provides two useful ways to work with binary representations:

- **`0b` prefix:** Identifies an integer written in binary notation. For example, `0b101` represents decimal `5`.
- **`bin(number)`:** Returns a string (`str`) containing the binary representation of an integer, including the `0b` prefix. It does not convert the integer into a different numeric data type.

Although decimal and binary use different notations, they can represent the same integer. Python's `int` type does not change based on how the number is written.

> [!NOTE]
> This section provides a brief introduction to binary so we can understand how bitwise operators work.
>
> For a detailed explanation of number systems, binary place values, decimal/binary conversions, signed integers, and two's complement, see [Binary and Number Systems](../02-concepts/binary-and-number-systems.md).

**Example 1: Displaying binary representations in Python.** We use `bin()` to display an integer in binary notation and the `0b` prefix to write a binary integer directly.

```python
number = 5

print(number)
print(bin(number))
print(0b101)
print(type(0b101))
```

**Output:**

```text
5
0b101
5
<class 'int'>
```

Breaking down the results:

1. **`print(number)`:** Displays the integer `5` in decimal notation.
2. **`bin(number)`:** Returns `"0b101"`, the binary representation of `5`.
3. **`print(0b101)`:** Displays `5` because binary `101` represents decimal `5`.
4. **`type(0b101)`:** Confirms that a binary integer literal is still an `int`.

### Bitwise AND (`&`)

The **bitwise AND operator (`&`)** compares corresponding bits of two integers.

A **corresponding bit** is a bit occupying the same position in each number's binary representation.

To perform bitwise AND:

1. Represent both integers in binary.
2. Align their bits by place value, starting from the right.
3. Compare the bits in each position.
4. Write `1` when both corresponding bits are `1`; otherwise, write `0`.
5. Convert the resulting binary value back into decimal if needed.

If one binary number has fewer digits than the other, we can add leading zeros to align them without changing its value.

The following truth table shows the rule for each pair of bits:

| First Bit | Second Bit | Result |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

**Example 1: Applying bitwise AND.** We compare `5` and `3` using their binary representations.

```python
number1 = 5
number2 = 3

print(number1 & number2)
print(bin(number1 & number2))
```

**Output:**

```text
1
0b1
```

Breaking down the calculation:

**Step 1: Convert both integers to binary.**

- Decimal `5` = Binary `101`
- Decimal `3` = Binary `011`

**Step 2: Align and compare the bits.**

```text
    101    (5)
  & 011    (3)
  -----
    001    (1)
```

**Step 3: Compare each position.**

| Place Value | First Bit | Second Bit | AND Result |
|---|---|---|---|
| 4 | 1 | 0 | 0 |
| 2 | 0 | 1 | 0 |
| 1 | 1 | 1 | 1 |

**Step 4: Convert the result to decimal.**

`001 = (0 * 4) + (0 * 2) + (1 * 1) = 1`

Therefore, `5 & 3` produces `1`.

### Bitwise OR (`|`)

The **bitwise OR operator (`|`)** compares corresponding bits and produces `1` when at least one of the two bits is `1`.

Unlike bitwise AND, both bits do not need to be `1`.

| First Bit | Second Bit | Result |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

**Example 1: Applying bitwise OR.** We compare the same two integers, `5` and `3`, using the OR rules.

```python
number1 = 5
number2 = 3

print(number1 | number2)
print(bin(number1 | number2))
```

**Output:**

```text
7
0b111
```

Breaking down the calculation:

**Step 1: Convert both integers to binary.**

- Decimal `5` = Binary `101`
- Decimal `3` = Binary `011`

**Step 2: Apply bitwise OR to each position.**

```text
    101    (5)
  | 011    (3)
  -----
    111    (7)
```

**Step 3: Compare each position.**

| Place Value | First Bit | Second Bit | OR Result |
|---|---|---|---|
| 4 | 1 | 0 | 1 |
| 2 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 |

**Step 4: Convert the result to decimal.**

`111 = (1 * 4) + (1 * 2) + (1 * 1) = 7`

Therefore, `5 | 3` produces `7`.

### Bitwise XOR (`^`)

The **bitwise XOR operator (`^`)** stands for *exclusive OR*.

Like bitwise OR, XOR compares corresponding bits. However, XOR produces `1` only when the two bits are **different**.

- If one bit is `1` and the other is `0`, the result is `1`.
- If both bits are `0`, the result is `0`.
- If both bits are `1`, the result is also `0`.

The distinction from ordinary bitwise OR is that **XOR requires exactly one of the two bits to be `1`**.

| First Bit | Second Bit | Result |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

> [!IMPORTANT]
> The `^` operator performs bitwise XOR, not exponentiation.
>
> Python uses `**` for exponentiation.

**Example 1: Applying bitwise XOR.**

```python
number1 = 5
number2 = 3

print(number1 ^ number2)
print(bin(number1 ^ number2))
```

**Output:**

```text
6
0b110
```

Breaking down the calculation:

**Step 1: Convert both integers to binary.**

- Decimal `5` = Binary `101`
- Decimal `3` = Binary `011`

**Step 2: Apply bitwise XOR.**

```text
    101    (5)
  ^ 011    (3)
  -----
    110    (6)
```

**Step 3: Compare each position.**

| Place Value | First Bit | Second Bit | XOR Result |
|---|---|---|---|
| 4 | 1 | 0 | 1 |
| 2 | 0 | 1 | 1 |
| 1 | 1 | 1 | 0 |

**Step 4: Convert the result to decimal.**

`110 = (1 * 4) + (1 * 2) + (0 * 1) = 6`

Therefore, `5 ^ 3` produces `6`.

### Bitwise NOT (`~`)

The **bitwise NOT operator (`~`)** is a unary operator that inverts the bits of an integer.

Unlike `&`, `|`, and `^`, which compare two operands, bitwise NOT operates on **one integer**.

Inverting bits means changing:

- `0` to `1`
- `1` to `0`

However, Python integers can represent both positive and negative values, and Python does not restrict them to a fixed number of bits. Bitwise operations behave as though signed integers have an indefinitely extended two's-complement representation. This is why inverting the visible digits of `101` is not enough to calculate `~5` correctly.

For Python integers, the result of bitwise NOT can be calculated using:

`~number = -(number + 1)`

For example, `~5` is `-6`, not `2`. The underlying signed-binary representation and *two's complement* are covered in [Binary and Number Systems](../concepts/binary-and-number-systems.md).

> [!NOTE]
> Bitwise NOT (`~`) and logical NOT (`not`) are different operations:
>
> - **`~`** inverts integer bits and produces an integer.
> - **`not`** reverses truthiness and produces a Boolean.

**Example 1: Applying bitwise NOT.**

```python
number = 5

print(~number)
print(not number)
```

**Output:**

```text
-6
False
```

Breaking down the bitwise calculation:

1. **Start with the integer:** `number = 5`
2. **Apply the formula:** `~number = -(number + 1)`
3. **Substitute the value:** `~5 = -(5 + 1)`
4. **Perform addition:** `~5 = -(6)`
5. **Apply the negative sign:** `~5 = -6`

Therefore, `~5` produces `-6`.

The logical expression `not number` produces `False` because the integer `5` is truthy.

### Bitwise Left Shift (`<<`)

The **bitwise left shift operator (`<<`)** shifts the binary digits of an integer to the left by a specified number of positions.

The expression has two operands:

- **Left operand:** The integer whose bits are being shifted.
- **Right operand:** The number of positions to shift.

For example, `5 << 2` shifts the bits of `5` two positions to the left.

For nonnegative integers, left shifting effectively adds zero bits on the right and moves the existing bits into higher-value positions.

Each position to the left is worth twice as much as the previous position.

Therefore:

- Shifting left by **1** position multiplies the value by `2`.
- Shifting left by **2** positions multiplies the value by `4`.
- Shifting left by **3** positions multiplies the value by `8`.

The general formula is:

`number << positions  →  number * (2 ** positions)`

This relationship holds for Python integers, including negative integers.

**Example 1: Shifting bits to the left.**

```python
number = 5

print(number << 1)
print(number << 2)
```

**Output:**

```text
10
20
```

Breaking down the calculations:

**Step 1: Convert `5` to binary.**

`5 = 101`

**Step 2: Shift left by one position.**

```text
101     (5)
1010    (10)
```

Using the formula:

`5 * (2 ** 1) = 5 * 2 = 10`

**Step 3: Shift left by two positions.**

```text
101      (5)
10100    (20)
```

Using the formula:

`5 * (2 ** 2) = 5 * 4 = 20`

Therefore, `5 << 1` produces `10`, while `5 << 2` produces `20`.

### Bitwise Right Shift (`>>`)

The **bitwise right shift operator (`>>`)** shifts the binary digits of an integer to the right by a specified number of positions.

Like left shift, it uses two operands:

- **Left operand:** The integer whose bits are being shifted.
- **Right operand:** The number of positions to shift.

For nonnegative integers, shifting right moves bits toward lower-value positions. Bits that move beyond the rightmost position are discarded.

Because each position to the right is worth half as much as the previous position, right shifting is related to floor division.

Therefore:

- Shifting right by **1** position is equivalent to floor-dividing by `2`.
- Shifting right by **2** positions is equivalent to floor-dividing by `4`.
- Shifting right by **3** positions is equivalent to floor-dividing by `8`.

The general formula is:

`number >> positions  →  number // (2 ** positions)`

This relationship also applies to negative Python integers. As with floor division, negative results round toward negative infinity.

> [!NOTE]
> A shift count must be nonnegative. Attempting a negative shift count, such as `5 >> -1`, raises a `ValueError`.

**Example 1: Shifting bits to the right.**

```python
number = 20

print(number >> 1)
print(number >> 2)
```

**Output:**

```text
10
5
```

Breaking down the calculations:

**Step 1: Convert `20` to binary.**

`20 = 10100`

**Step 2: Shift right by one position.**

```text
10100    (20)
 1010    (10)
```

Using the formula:

`20 // (2 ** 1) = 20 // 2 = 10`

**Step 3: Shift right by two positions.**

```text
10100    (20)
  101    (5)
```

Using the formula:

`20 // (2 ** 2) = 20 // 4 = 5`

Therefore, `20 >> 1` produces `10`, while `20 >> 2` produces `5`.

**Example 2: Right shifting when bits are discarded.** Right shifting can produce a result that is not an exact mathematical division because discarded bits represent the remainder.

```python
number = 13

print(number >> 1)
print(number >> 2)
```

**Output:**

```text
6
3
```

Breaking down the calculations:

1. **Convert to binary:** `13 = 1101`
2. **Shift by one:** `1101` becomes `110`, representing decimal `6`. The rightmost `1` is discarded.
3. **Shift by two:** `1101` becomes `11`, representing decimal `3`. The two rightmost bits are discarded.

We can confirm the results using floor division:

- `13 // 2 = 6`
- `13 // 4 = 3`

### Practical Uses of Bitwise Operators

Bitwise operators are useful when individual bits represent independent settings or states.

Common applications include:

- **Permissions:** Representing read, write, or execute access.
- **Flags:** Storing multiple on/off settings in a single integer.
- **Networking:** Working with IP addresses, subnet masks, and protocol fields.
- **Hardware:** Reading or modifying individual bits in device registers.
- **Binary data:** Interpreting information stored in compact binary formats.

One particularly useful application is representing multiple permissions using a single integer.

#### Understanding Bit Flags

A **bit flag** is an individual bit used to represent whether a particular setting is enabled.

For example, we can represent three permissions using three separate bit positions:

| Permission | Binary Value | Decimal Value | Meaning |
|---|---|---|---|
| READ | `001` | 1 | Permission to read |
| WRITE | `010` | 2 | Permission to write |
| EXECUTE | `100` | 4 | Permission to execute |

Notice that each permission uses a different bit position.

This matters because multiple permissions can be combined without losing track of which ones are enabled.

For example, combining READ and WRITE using bitwise OR:

```text
    001    (READ)
  | 010    (WRITE)
  -----
    011    (READ + WRITE)
```

The resulting binary value `011` means:

- The READ bit is `1`, so READ is enabled.
- The WRITE bit is `1`, so WRITE is enabled.
- The EXECUTE bit is `0`, so EXECUTE is disabled.

The integer `3` therefore represents both READ and WRITE permissions.

To check whether a particular permission is enabled, we can use **bitwise AND (`&`)**.

When we perform bitwise AND between the combined permissions and a particular permission flag:

- A **nonzero result** indicates that the tested flag's bit is enabled.
- A **zero result** indicates that the flag's bit is not enabled.

Python's built-in **`bool(value)`** function converts a value into a Boolean based on its truthiness:

- `bool(0)` returns `False`.
- `bool(1)` returns `True`.
- `bool(2)` returns `True`.

We can therefore use `bool()` to display permission checks as `True` or `False` rather than integers.

**Example 1: Combining and checking permissions.** We define three permission flags, combine READ and WRITE using bitwise OR, and test each permission using bitwise AND.

```python
READ = 0b001
WRITE = 0b010
EXECUTE = 0b100

permissions = READ | WRITE

print(bin(permissions))
print(bool(permissions & READ))
print(bool(permissions & WRITE))
print(bool(permissions & EXECUTE))
```

**Output:**

```text
0b11
True
True
False
```

Breaking down the example:

**Step 1: Define the permissions.**

```text
READ     = 001    (1)
WRITE    = 010    (2)
EXECUTE  = 100    (4)
```

**Step 2: Combine READ and WRITE using bitwise OR.**

```text
    001    (READ)
  | 010    (WRITE)
  -----
    011    (3)
```

The variable `permissions` now references the integer `3`, represented in binary as `011`.

Python's `bin()` function displays it as `0b11` because leading zeros are omitted.

**Step 3: Check the READ permission.**

```text
    011    (Current permissions)
  & 001    (READ)
  -----
    001    (1)
```

The result is `1`, so `bool(1)` returns `True`.

**Step 4: Check the WRITE permission.**

```text
    011    (Current permissions)
  & 010    (WRITE)
  -----
    010    (2)
```

The result is `2`, so `bool(2)` returns `True`.

**Step 5: Check the EXECUTE permission.**

```text
    011    (Current permissions)
  & 100    (EXECUTE)
  -----
    000    (0)
```

The result is `0`, so `bool(0)` returns `False`.

Therefore, the combined permissions enable READ and WRITE but not EXECUTE.

[🔼 Back to Overview](#overview)

---

[🔼 Back to Python Basics](README.md)