---
Created: 2026-10-21
Modified: 
---

# Numbers

Python uses **numeric data types** to represent quantities and perform calculations. A value such as `25` may look similar to `25.0`, but Python treats them as different types with different characteristics. Knowing those distinctions helps prevent unexpected results when doing arithmetic, comparing values, converting data, or working with money.

This guide explains Python's built-in numeric types (`int`, `float`, and `complex`), how numeric values are created and converted, how floating-point precision works, and how to choose the appropriate type for a calculation. The underlying mathematics of binary, hexadecimal, and two's complement is covered separately in [Binary and Number Systems](../../../02-concepts/binary-and-number-systems.md). Arithmetic and bitwise operator rules are covered in [Operators](05-operators.md).

## Overview

1. [Understanding Numeric Types](#understanding-numeric-types)
2. [Choosing the Appropriate Numeric Type](#choosing-the-appropriate-numeric-type)
3. [Integers (`int`)](#integers-int)
4. [Floating-Point Numbers (`float`)](#floating-point-numbers-float)
5. [Complex Numbers (`complex`)](#complex-numbers-complex)
6. [Checking Numeric Types](#checking-numeric-types)
7. [Converting Between Numeric Types](#converting-between-numeric-types)
8. [Arithmetic Results and Numeric Types](#arithmetic-results-and-numeric-types)
9. [Floating-Point Precision](#floating-point-precision)
10. [Comparing Numbers Safely](#comparing-numbers-safely)
11. [Rounding and Formatting Numbers](#rounding-and-formatting-numbers)
12. [Useful Built-in Numeric Functions](#useful-built-in-numeric-functions)
13. [Numeric Limits and Special Values](#numeric-limits-and-special-values)
14. [Working with Money and Decimal Numbers](#working-with-money-and-decimal-numbers)

## Understanding Numeric Types

A **numeric type** determines how a number is represented and which numeric operations it supports. Python provides three built-in numeric types:

| Type | Name | Represents | Example |
|---|---|---|---|
| `int` | Integer | Whole numbers, including negative numbers and zero | `25`, `-8`, `0` |
| `float` | Floating-point number | Approximate real numbers, including numbers with fractional parts | `25.0`, `-3.75`, `1e6` |
| `complex` | Complex number | Numbers with real and imaginary components | `3 + 4j` |

Python determines the type of a numeric **literal** (a value written directly in code) from its notation. For example, `25` is an integer literal, while `25.0` is a floating-point literal. A combined expression such as `3 + 4j` uses a complex literal (`4j`) and arithmetic to produce a complex number.

Two objects of different numeric types may still represent equal mathematical values. **Type equality and numeric value equality are different questions.** For example, `25 == 25.0` is `True`, even though `type(25) == type(25.0)` is `False`.

> [!NOTE]
> Python's `bool` type is a subclass of `int`, and `True` and `False` behave numerically like `1` and `0` in many operations. Booleans are still best treated as logical values; their main explanation belongs in the separate Booleans guide.

**Example 1: Recognizing numeric types.** The built-in `type(object)` function reports the type of an object.

```python
whole_number = 25
floating_number = 25.0
complex_number = 3 + 4j

print(type(whole_number))
print(type(floating_number))
print(type(complex_number))
```

**Output:**

```text
<class 'int'>
<class 'float'>
<class 'complex'>
```

Each variable references a numeric object, but the objects belong to different built-in types.

### Number Systems, Numeric Types, and Literals

When working with numbers in Python, it's important to distinguish between a **number system**, a **numeric data type**, and a **numeric literal**.

Although these concepts are related, they describe different characteristics of a number:

- **Number system:** Determines how a number is written using digits and place values. Common number systems include decimal (base 10), binary (base 2), octal (base 8), and hexadecimal (base 16).
- **Numeric data type:** Determines how Python represents and works with a numeric value. Python's built-in numeric types include `int`, `float`, and `complex`.
- **Numeric literal:** A number written directly in source code rather than obtained from a variable or calculation. Python recognizes different literal notations to determine the value and numeric type. The spelling of a number in your code and its stored representation are not necessarily the same.

#### Understanding Decimal Notation

The word **decimal** can be confusing because it is commonly used to describe numbers containing a decimal point, such as `3.14`.

However, in computer science, **decimal also refers to the base-10 number system**, which uses the digits `0` through `9`. A number does not need a decimal point to be written in decimal notation.

For example:

- `42` is an integer written in decimal (base-10) notation.
- `42.0` is a floating-point literal written in decimal (base-10) notation.
- `0b101010` is an integer written in binary (base-2) notation.

Although all three represent the same mathematical value, Python assigns `42.0` the `float` type and the other two the `int` type.

#### Numeric Literal Notation

Python supports several ways of writing numeric values directly in source code.

| Python Code | Number System / Notation | Python Data Type | Meaning |
|---|---|---|---|
| `125` | Decimal integer (base 10) | `int` | Integer one hundred twenty-five |
| `-125` | Decimal integer (base 10) with unary minus | `int` | Negative one hundred twenty-five |
| `125.0` | Decimal (base 10) with a decimal point | `float` | Floating-point value of 125 |
| `1.25e2` | Decimal (base 10) scientific notation | `float` | `1.25 × 10² = 125.0` |
| `0b101` | Binary (base 2) | `int` | Decimal value `5` |
| `0o12` | Octal (base 8) | `int` | Decimal value `10` |
| `0xFF` | Hexadecimal (base 16) | `int` | Decimal value `255` |
| `2j` | Imaginary literal (decimal notation) | `complex` | Complex value `0 + 2j` |

> [!NOTE]
> The negative sign (`-`) in `-125` is technically a **unary operator** applied to the integer literal `125`, rather than part of the literal itself. The resulting expression evaluates to an `int`.
>
> Scientific notation is another way of writing decimal values, not a separate number system. The letter `e` or `E` represents multiplication by a power of `10`. Scientific notation produces a `float`, even without a decimal point.
>
> Binary, octal, and hexadecimal conversions are explained in [Binary and Number Systems](../../../02-concepts/binary-and-number-systems.md). This guide focuses on how Python interprets these notations.

#### Improving Readability with Underscores

Python allows the underscore (`_`) character to be placed between digits as a **visual separator**, making large or grouped numbers easier to read.

For example:

- `1_000_000` is equivalent to `1000000`.
- `0b1111_0000` is equivalent to `0b11110000`.
- `0xFF_A0` is equivalent to `0xFFA0`.

Underscores do not change a number's value or data type. They only improve source-code readability. Place them between digits or other permitted digit groups, rather than inserting them arbitrarily (for example, `1__000` is invalid).

**Example 1: Reading numeric notation.** The following example demonstrates how Python interprets numeric literals written using different notations and visual separators.

```python
population = 1_000_000
plain_population = 1000000
hex_value = 0xFF

print(population == plain_population)
print(population)
print(hex_value)
print(type(hex_value))
```

**Output:**

```text
True
1000000
255
<class 'int'>
```

Breaking down the results:

1. **`population == plain_population`:** Returns `True` because underscores do not change the numeric value. Both variables contain the integer `1000000`.
2. **`print(population)`:** Displays `1000000` without underscores because they are only used for readability in source code.
3. **`print(hex_value)`:** Displays `255` because Python prints integers in decimal notation by default, even when they are written using hexadecimal literals.
4. **`type(hex_value)`:** Returns `<class 'int'>`, confirming that hexadecimal notation produces an integer.

> [!IMPORTANT]
> **A number system determines how a number is written, while a numeric data type determines how Python represents and processes it.**
>
> For example, `42` is a decimal integer, `42.0` is a floating-point value written in decimal notation, and `0b101010` is a binary integer. All represent the same mathematical value, but Python does not necessarily assign them the same numeric type.

[🔼 Back to Overview](#overview)

## Choosing the Appropriate Numeric Type

Choosing a numeric type depends on the **meaning of the value** and the **accuracy requirements** of the operation. It is not simply a question of whether the value contains a decimal point.

| Situation | Common Choice | Reason |
|---|---|---|
| Counting users, items, or requests | `int` | Counts are whole numbers and require exact integer arithmetic |
| Indexing list positions | `int` | Indexes are integer positions |
| General scientific or measurement calculations | `float` | Efficient representation of approximate real values |
| Mathematical calculations involving imaginary components | `complex` | Supports real and imaginary parts |
| Financial calculations requiring decimal precision and specified rounding rules | `decimal.Decimal` | Supports decimal arithmetic and configurable rounding without typical binary floating-point conversion errors |
| Money represented in the smallest unit (e.g., cents) | `int` | Exact whole-number storage when currency rules permit |

Remember these distinctions:

- **`int` versus `float`:** `5` and `5.0` compare as equal (`5 == 5.0` is `True`), but `int` stores integers exactly, while `float` uses binary floating-point representation and cannot represent every decimal fraction exactly.
- **`float` versus `Decimal`:** Floats are appropriate for many approximate calculations. Decimals are useful when exact decimal representation and specific rounding rules matter.
- **Arithmetic result types:** Operators can produce a different numeric type than their operands; `/` is a common example.
- **Comparisons:** Use exact integer equality where appropriate, and choose a meaningful tolerance for approximate floating-point comparisons.

The core principle is to **choose a representation based on what the value means and what errors are acceptable**, rather than on which notation looks most convenient.

[🔼 Back to Overview](#overview)

## Integers (`int`)

An **integer** is a whole number without a fractional component. Integers can be positive, negative, or zero. A negative integer such as `-12` is still an integer; the minus sign changes its sign, not its numeric type. The `int` type is appropriate for counts, quantities, indexes, whole-number identifiers, and calculations that should not introduce fractional values.

Python integers have **arbitrary precision**: unlike many fixed-width integer types in other programming languages, Python's built-in `int` can represent very large numbers as long as the environment has sufficient memory. This does **not** mean every integer calculation is fast or memory-free—larger integers require more storage and processing.

Important characteristics of `int` include:

- **Whole-number values:** `5`, `0`, and `-12` are integers; `5.5` is not.
- **Immutable objects:** An integer object's value cannot be changed in place. Calculations produce results, and reassignment associates a variable name with the resulting object.
- **Exact integer arithmetic:** Integer addition, subtraction, and multiplication do not accumulate floating-point rounding errors.
- **Multiple literal notations:** Integers can be written in decimal, binary (`0b`), octal (`0o`), or hexadecimal (`0x`). The number system changes how the value is written, not the `int` type.

> [!IMPORTANT]
> Binary, octal, and hexadecimal are **number systems**, not separate Python numeric data types. The `0b`, `0o`, and `0x` prefixes tell Python how to interpret the digits of an integer literal.
>
> For a deeper explanation of these number systems and how to convert between them, see [Binary and Number Systems](../../../02-concepts/binary-and-number-systems.md).

**Example 1: Creating and using integers.** We work with positive, negative, and zero values and verify that integer addition produces an integer.

```python
items = 12
returned_items = -2
starting_balance = 0

total = items + returned_items

print(items)
print(returned_items)
print(starting_balance)
print(total)
print(type(total))
```

**Output:**

```text
12
-2
0
10
<class 'int'>
```

The total is `10` because `12 + (-2) = 10`. The result remains an integer.

**Example 2: Integers written in different number systems.** Python supports integer literals written in decimal (base 10), binary (base 2), octal (base 8), and hexadecimal (base 16).

Each number system uses a different notation:

- **Decimal:** Uses digits `0–9` and does not require a prefix.
- **Binary:** Uses digits `0–1` and the `0b` prefix.
- **Octal:** Uses digits `0–7` and the `0o` prefix.
- **Hexadecimal:** Uses digits `0–9` and letters `A–F` (or `a–f`) with the `0x` prefix.

The following literals all represent the same integer value, `42`, even though they use different number systems.

```python
decimal_number = 42
binary_number = 0b101010
octal_number = 0o52
hexadecimal_number = 0x2A

print(decimal_number, type(decimal_number))
print(binary_number, type(binary_number))
print(octal_number, type(octal_number))
print(hexadecimal_number, type(hexadecimal_number))

print(decimal_number == binary_number == octal_number == hexadecimal_number)
```

**Output:**

```text
42 <class 'int'>
42 <class 'int'>
42 <class 'int'>
42 <class 'int'>
True
```

Breaking down the results:

1. **Numeric values:** Python displays all four integers as `42` because `print()` uses decimal notation by default.
2. **Data types:** Every `type()` call returns `<class 'int'>`, confirming that all four literals create integer values.
3. **Equality:** The chained comparison returns `True` because all four variables reference numerically equal integer values.

### Large Integers

Python's `int` type supports **arbitrary-precision integers**, meaning integers are not restricted to a fixed number of bits, such as 32 or 64 bits.

A **bit** is a binary digit that can represent either `0` or `1`, and **8 bits make 1 byte**. A **bit width** describes how many bits are used to represent a value. For a fixed-width integer, more bits allow more distinct bit patterns—and therefore a larger possible numeric range. Whether negative numbers are supported also affects that range.

In many programming languages, integer types have a fixed bit width, which limits the range of values they can represent.

For example:

- **32-bit integer:** Uses a fixed number of 32 bits to represent a value.
- **64-bit integer:** Uses a fixed number of 64 bits to represent a value.
- **Python `int`:** Can use additional storage as needed to represent larger values.

Python integers can therefore grow beyond the range of traditional fixed-width integer types. Their practical limit depends on available memory and implementation constraints. Arbitrary precision does not mean unlimited performance or storage.

> [!NOTE]
> Bit width and memory usage are related but different concepts. Python integer objects also require memory for information beyond the numeric value itself.
>
> For a detailed explanation of bits, bytes, bit widths, signed integers, and numeric ranges, see [Binary and Number Systems](../../../02-concepts/binary-and-number-systems.md).

**Example 3: Using an integer larger than 64 bits.** The exponentiation operator `**` raises the left operand to the power of the right operand.

```python
large_number = 2 ** 100

print(large_number)
print(type(large_number))
```

**Output:**

```text
1267650600228229401496703205376
<class 'int'>
```

The calculation is exact and the result is still an `int`, even though it is too large to fit into an unsigned 64-bit integer.

[🔼 Back to Overview](#overview)

## Floating-Point Numbers (`float`)

A **floating-point number**, or **float**, represents a number using a finite binary approximation. Floats can represent values with fractional parts, very large or very small magnitudes, and special values such as positive infinity.

In typical Python implementations, the built-in `float` uses **IEEE 754 binary64**, often called *double precision*. It has roughly **15–17 significant decimal digits** of precision. The exact limits matter because many decimal fractions cannot be stored perfectly in binary floating-point format.

A decimal point or exponent notation can create a float even when the mathematical value is a whole number:

- `5` is an `int`.
- `5.0` is a `float`.
- `5e2` is a `float` representing `500.0`.
- `1.25e-3` is a `float` representing `0.00125`.

**Scientific notation** is a way of writing numbers using a coefficient and a power of `10`. It uses the letter `e` or `E` to mean *multiplied by 10 raised to a specified power*.

Scientific notation is not a separate number system or numeric data type. Python uses decimal (base-10) notation for these expressions, even though floating-point values are represented internally using binary.

For example:

- **`1.5e3`:** Represents `1.5 × 10³ = 1500.0`.
- **`5e2`:** Represents `5 × 10² = 500.0`.
- **`1.25e-3`:** Represents `1.25 × 10⁻³ = 0.00125`.

A positive exponent moves the decimal point to the right, while a negative exponent moves it to the left.

> [!NOTE]
> Numeric literals written using scientific notation produce a `float`, even when they do not contain a decimal point. For example, `5e2` produces `500.0`, not the integer `500`.

**Example 1: Creating floats.** We create floating-point values using decimal and exponent notation.

```python
price = 19.95
whole_as_float = 5.0
large_value = 1.5e3
small_value = 2.5e-3

print(price)
print(whole_as_float)
print(large_value)
print(small_value)
print(type(whole_as_float))
```

**Output:**

```text
19.95
5.0
1500.0
0.0025
<class 'float'>
```

The `e3` notation multiplies `1.5` by `1000`; `e-3` divides `2.5` by `1000`. Even `5.0` remains a float rather than an integer.

[🔼 Back to Overview](#overview)

## Complex Numbers (`complex`)

A **complex number** contains two parts:

- **Real part:** The ordinary numeric component.
- **Imaginary part:** A component multiplied by the imaginary unit, conventionally written as *i* in mathematics and `j` in Python.

The imaginary unit is defined by the property that its square equals `-1`. Python writes it as `1j`, so `(1j) ** 2` produces `(-1+0j)`. A complex number is usually written in mathematics as `a + bj`, where `a` is the real part and `b` is the imaginary coefficient. In Python, actual numeric literals use forms such as `3 + 4j`; if `a` and `b` are variables, write `a + b * 1j`.

For example, `3 + 4j` has real part `3` and imaginary part `4`. Python exposes these through the `.real` and `.imag` attributes, which return floats for the built-in `complex` type. The built-in `abs()` function returns a complex number's **magnitude**: its distance from zero on a two-dimensional plane. For real part `a` and imaginary coefficient `b`, the magnitude is `√(a² + b²)`, using the Pythagorean theorem.

Complex numbers are useful in scientific computing, electrical engineering, signal processing, and certain mathematical calculations. They are less common in everyday application development.

> [!NOTE]
> Complex numbers cannot be ordered using `<`, `>`, `<=`, or `>=`. Equality comparisons are allowed, but Python does not define a single natural greater-than or less-than ordering for complex values.

### Creating Complex Numbers with `complex()`

The built-in **`complex(real, imag=0)`** function creates a complex number. The optional second argument specifies the imaginary coefficient.

**Example 1: Constructing a complex value.**

```python
number = complex(2, 3)

print(number)
print(number == 2 + 3j)
```

**Output:**

```text
(2+3j)
True
```

Both expressions represent the same complex value.

**Example 2: Inspecting a complex number.** We inspect its components and calculate its magnitude.

```python
number = 3 + 4j

print(number)
print(number.real)
print(number.imag)
print(abs(number))
print(type(number))
```

**Output:**

```text
(3+4j)
3.0
4.0
5.0
<class 'complex'>
```

The magnitude is `5.0` because `sqrt(3² + 4²) = sqrt(9 + 16) = sqrt(25) = 5`.

**Example 3: Calculating the electrical impedance of a microwave oven.** Complex numbers are used in electrical engineering to represent **impedance**, which describes how an electrical circuit opposes alternating current (AC).

Imagine an engineer evaluating a household microwave oven advertised as **1,000 watts**.

The 1,000 W rating typically describes the microwave's cooking output rather than the electrical power consumed from the wall outlet. Because the appliance is not 100% efficient, its electrical input power is generally higher.

Suppose the engineer measures the following operating characteristics:

| Measurement | Value | Description |
|---|---|---|
| Microwave cooking output | 1,000 W | Power delivered for heating food |
| Electrical input power | 1,440 W | Real electrical power consumed |
| Supply voltage | 120 V RMS | Voltage supplied by the wall outlet |
| Power factor | 0.80 | Ratio of real power to apparent power |

These are **hypothetical measurements** chosen to demonstrate the calculation, not specifications for a particular microwave model. Here **RMS** (root mean square) is the conventional effective AC voltage or current measurement; **power factor** is real power divided by apparent power. This example assumes a steady, sinusoidal AC load with an inductive phase angle. Those assumptions matter because real microwaves may draw distorted current.

In AC circuits, impedance has two components:

- **Resistance (real component):** Represents the portion of impedance associated with real power consumption.
- **Reactance (imaginary component):** Represents the portion associated with energy being temporarily stored and returned by inductive or capacitive effects.

Both are measured in **ohms (Ω)**.

The engineer wants to determine the microwave's equivalent input impedance to understand how the electrical load relates to current, voltage, and power.

First, the engineer calculates the **RMS current** using the real power, voltage, and power factor:

`current = input_power / (voltage * power_factor)`

Substituting the measured values:

`current = 1440 / (120 * 0.80) = 15 A`

Next, using the simplified sinusoidal-load model, the engineer calculates the **impedance magnitude** (overall opposition to AC current):

`impedance_magnitude = voltage / current`

`120 / 15 = 8 Ω`

The magnitude tells us the overall opposition to AC current, but it does not distinguish resistance from reactance.

Because the model assumes an ordinary series resistance–inductance equivalent, the engineer uses the power factor to calculate the resistance component:

`resistance = impedance_magnitude * power_factor`

`8 * 0.80 = 6.4 Ω`

With the model's **inductive** assumption, reactance is positive, giving the imaginary term `+4.8j`. A capacitive equivalent would use a negative reactance instead. The reactance component can then be calculated using the Pythagorean theorem:

`reactance = √(impedance_magnitude² - resistance²)`

`reactance = √(8² - 6.4²) = 4.8 Ω`

The engineer can now represent the microwave's equivalent input impedance as a complex number:

`impedance = 6.4 + 4.8j`

Python uses `j` to represent the imaginary component.

The built-in **`abs()`** function calculates the magnitude of a complex number, allowing the engineer to verify the calculated impedance magnitude.

```python
impedance = 6.4 + 4.8j

print(impedance)
print(impedance.real)
print(impedance.imag)
print(abs(impedance))
```

**Output:**

```text
(6.4+4.8j)
6.4
4.8
8.0
```

Breaking down the results:

1. **`impedance`:** Represents the microwave's simplified equivalent input impedance as `(6.4+4.8j)`.
2. **`impedance.real`:** Returns `6.4`, representing the resistive component in ohms.
3. **`impedance.imag`:** Returns `4.8`, representing the inductive reactance in ohms.
4. **`abs(impedance)`:** Returns `8.0`, representing the total impedance magnitude in ohms.

The magnitude is calculated as follows:

1. **Square the resistance:** `6.4² = 40.96`
2. **Square the reactance:** `4.8² = 23.04`
3. **Add the results:** `40.96 + 23.04 = 64`
4. **Calculate the square root:** `√64 = 8`

Therefore, the microwave has an equivalent impedance magnitude of `8 Ω` under the assumed operating conditions.

**What does this tell the engineer?**

At `120 V RMS`, the calculated current is:

`current = 120 / 8 = 15 A`

This helps the engineer estimate current demand and apparent power (`120 V × 15 A = 1,800 VA`) alongside the measured real power of `1,440 W`. A 15 A current estimate is not, by itself, a recommendation for breaker or circuit sizing. The complex impedance also describes the relationship between voltage and current, which the magnitude alone cannot fully capture.

> [!NOTE]
> This is a simplified AC circuit model used to demonstrate Python's `complex` type. Actual microwave ovens contain multiple electrical components, and their input characteristics vary by design and operating condition. In particular, conventional transformer-based and inverter-based microwaves may behave differently, and some appliances draw nonsinusoidal current that cannot be fully described by a single complex impedance.
>
> The 1,000 W cooking-output rating alone is not enough to calculate impedance. The additional electrical measurements and assumptions are necessary.

[🔼 Back to Overview](#overview)

## Checking Numeric Types

Python provides built-in functions for identifying the types of objects:

- **`type(object)`** returns the object's exact type.
- **`isinstance(object, type_or_tuple)`** returns `True` if an object is an instance of the specified type **or a subclass of that type**.

These are not always equivalent tests. For example, `bool` is a subclass of `int`, so `isinstance(True, int)` returns `True` even though `type(True) == int` is `False`.

Use `type()` when you specifically want to inspect an object's exact type. Use `isinstance()` when accepting subclasses is appropriate.

**Example 1: Comparing `type()` and `isinstance()`.**

```python
number = 42
flag = True

print(type(number))
print(isinstance(number, int))
print(type(flag) == int)
print(isinstance(flag, int))
print(type(flag))
```

**Output:**

```text
<class 'int'>
True
False
True
<class 'bool'>
```

The last three results show why checking `isinstance(value, int)` also accepts Booleans. If an application specifically needs a non-Boolean integer, that distinction matters.

[🔼 Back to Overview](#overview)

## Converting Between Numeric Types

**Type conversion** creates a value of a requested type from a compatible value. Python provides built-in constructors for common numeric conversions:

| Function | Purpose | Example | Result |
|---|---|---|---|
| `int(value)` | Convert to an integer | `int(3.9)` | `3` |
| `float(value)` | Convert to a float | `float(5)` | `5.0` |
| `complex(value)` | Convert to a complex number | `complex(5)` | `(5+0j)` |
| `complex(real, imag)` | Construct a complex number from two components | `complex(2, 3)` | `(2+3j)` |

Conversion does **not** necessarily preserve every detail of the original value. Converting a float to an integer removes its fractional portion **toward zero**. Converting a large integer to a float may lose precision because floats have a finite number of significant digits.

### Converting Values to Integers

**`int(value)`** converts a supported numeric value into an integer. For floats, it **truncates toward zero**; it does not round to the nearest integer or always round down.

This distinction is especially important for negative floats:

- `int(3.9)` produces `3`.
- `int(-3.9)` produces `-3`.
- `-3.9 // 1` produces `-4.0`, because floor division rounds toward negative infinity.

**Example 1: Converting floats into integers.**

```python
print(int(3.9))
print(int(-3.9))
print(int(3.0))
```

**Output:**

```text
3
-3
3
```

The fractional portion is removed in each case. For `-3.9`, truncating toward zero gives `-3`, not `-4`.

### Converting Integers to Floats

**`float(value)`** converts compatible numeric values to floating-point values. Converting a sufficiently small integer may preserve its exact value; converting a very large integer may round to the nearest representable float or overflow.

**Example 1: Converting whole numbers to floats.**

```python
number = 25
converted = float(number)

print(converted)
print(type(converted))
print(number == converted)
```

**Output:**

```text
25.0
<class 'float'>
True
```

The resulting object has a different type, but its mathematical value still compares as equal to integer `25`.

### Converting Strings into Numbers

A **numeric string** is a string containing characters that represent a number, such as `"25"` or `"25.5"`. Although these strings look like numbers, Python treats them as text (`str`) until they are explicitly converted into a numeric data type.

> [!IMPORTANT]
> Python does not automatically convert numeric strings during incompatible arithmetic. For example, `"25" + 5` raises `TypeError` because Python cannot directly add a string and an integer.
>
> If a string cannot be interpreted as a valid number for the requested conversion, `int()` or `float()` raises `ValueError`. Programs accepting user input should handle this possibility.

Python provides two built-in functions for converting numeric strings:

- **`int()`:** Converts a valid integer string into an `int`. By default, Python interprets the string using decimal (base-10) notation. An optional second argument specifies a different number system.
- **`float()`:** Converts a valid floating-point string into a `float`. It accepts decimal notation, including scientific notation such as `"1.5e2"`.

For example:

- **`int("25")`:** Converts the decimal (base-10) string `"25"` into integer `25`.
- **`float("25.5")`:** Converts the decimal (base-10) string `"25.5"` into float `25.5`.
- **`float("25")`:** Converts the string `"25"` into float `25.0`, even though the string has no decimal point.
- **`int("25.5")`:** Raises `ValueError` because the string is not a valid integer representation.
- **`int("101", 2)`:** Interprets `"101"` as binary (base 2) and returns integer `5`. The second argument (`2`) determines the number system and its place values. Python displays the result using decimal (base-10) notation by default.

> [!NOTE]
> The number system determines the value of each digit's position. For example, binary (base 2) uses powers of 2, while octal (base 8) uses powers of 8.
>
> For a detailed explanation of positional values and converting between number systems, see [Binary and Number Systems](../../../02-concepts/binary-and-number-systems.md).

**Example 1: Converting numeric strings.**

```python
quantity_text = "25"
price_text = "19.95"

quantity = int(quantity_text)
price = float(price_text)

print(quantity, type(quantity))
print(price, type(price))
print(quantity + 5, type(quantity + 5))
```

**Output:**

```text
25 <class 'int'>
19.95 <class 'float'>
30 <class 'int'>
```

The conversion produces numeric objects that can be used in arithmetic. The original strings remain strings.

**Example 2: Handling invalid numeric text.** We catch the `ValueError` raised when the input cannot be converted into an integer.

```python
text = "twenty-five"

try:
    number = int(text)
    print(number)
except ValueError:
    print("Enter a valid whole number.")
```

**Output:**

```text
Enter a valid whole number.
```

Python enters the `except` block because the string does not contain a valid integer representation. The `try`/`except` statement allows the program to handle the conversion error instead of stopping. The `try` block attempts the conversion; the `except ValueError` block runs only if that kind of error occurs.

**Example 3: Interpreting numeric strings using different number systems.** The second argument of `int()` specifies which base Python should use to interpret the string's digits.

```python
print(int("101", 2))
print(int("101", 8))
print(int("101", 10))
```

**Output:**

```text
5
65
101
```

Breaking down the results:

1. **`int("101", 2)`:** Interprets `"101"` using binary (base 2), producing decimal value `5`.
2. **`int("101", 8)`:** Interprets `"101"` using octal (base 8), producing decimal value `65`.
3. **`int("101", 10)`:** Interprets `"101"` using decimal (base 10), producing value `101`.

The digits remain the same, but the specified number system changes their place values. Python returns an `int` in all three cases and displays the results using decimal notation by default.

[🔼 Back to Overview](#overview)

## Arithmetic Results and Numeric Types

The types of arithmetic results depend on the **operator** and the **operand types**. Python does not always return the same type that was used for every operand.

| Expression | Result | Result Type | Explanation |
|---|---|---|---|
| `5 + 2` | `7` | `int` | Both operands are integers |
| `5 + 2.0` | `7.0` | `float` | Integer and float arithmetic yields float |
| `5 / 2` | `2.5` | `float` | True division returns float for integer operands |
| `6 / 2` | `3.0` | `float` | Exact integer division still produces float |
| `5 // 2` | `2` | `int` | Floor division of integers yields integer |
| `5 // 2.0` | `2.0` | `float` | A float operand generally yields float |
| `5 % 2` | `1` | `int` | Integer remainder |
| `2 ** -2` | `0.25` | `float` | Negative integer power gives reciprocal |
| `2 + 3j` | `(2+3j)` | `complex` | Mixed integer and complex arithmetic |

**Regular division (`/`)** and **floor division (`//`)** must not be confused. Regular division produces a float when both operands are integers. Floor division rounds the mathematical quotient down toward negative infinity. With negative inputs, that can differ from truncation toward zero.

For the full breakdown of arithmetic operators, negative floor division, modulus, and exponentiation, see [Arithmetic Operators](05-operators.md#arithmetic-operators).

**Example 1: Inspecting the types of arithmetic results.**

```python
print(6 / 2, type(6 / 2))
print(6 // 2, type(6 // 2))
print(6 // 2.0, type(6 // 2.0))
```

**Output:**

```text
3.0 <class 'float'>
3 <class 'int'>
3.0 <class 'float'>
```

The mathematical quotient is `3` in every case, but the returned type depends on the operator and operands.

[🔼 Back to Overview](#overview)

## Floating-Point Precision

Floating-point precision is one of the most important concepts to understand when working with Python numbers.

A `float` cannot store every possible decimal number exactly. Most computers represent floats using a limited number of binary digits. Some fractions that look simple in decimal have **repeating binary expansions**, just as `1 / 3` has the repeating decimal representation `0.3333...`.

For example, `0.1` has no finite representation in binary floating point. Python stores the closest representable value, and calculations use these approximations. The difference may not be visible when a float is printed in its shortest convenient form, but it can appear in later calculations.

**Precision** describes how many meaningful digits a representation can retain. **Range** describes how large or small a number can be. These are separate characteristics: a float can represent very large magnitudes while being unable to distinguish some nearby integer values at that magnitude.

> [!IMPORTANT]
> Floating-point rounding is expected behavior, not a Python bug. Avoid assuming that every decimal fraction or every result of float arithmetic is exact.

### Why `0.1 + 0.2` Is Not Exactly `0.3`

The decimal fractions `0.1` and `0.2` are both approximated when stored in binary floating point. Their computed sum is a nearby representable float, which is not the same float as the one produced by the literal `0.3`.

**Example 1: Observing floating-point precision.**

```python
result = 0.1 + 0.2

print(result)
print(result == 0.3)
print(format(0.1, ".20f"))
```

**Output:**

```text
0.30000000000000004
False
0.10000000000000000555
```

Breaking down the output:

1. **`result`:** Displays the nearest floating-point result of adding the two stored approximations.
2. **`result == 0.3`:** Returns `False` because those two floating-point values are not identical.
3. **`format(0.1, ".20f")`:** Shows twenty digits after the decimal point, revealing part of the stored approximation.

### Loss of Precision with Large Integers

Binary64 floats can exactly represent every integer from `-(2 ** 53)` through `2 ** 53`, but they cannot distinguish **all** consecutive integers beyond that threshold. The value `2 ** 53 + 1`, for instance, is between two representable floats. Converting an integer to a float can therefore discard meaningful digits.

**Example 1: Converting a large integer to a float.**

```python
large_integer = 2 ** 53 + 1
converted = float(large_integer)

print(large_integer)
print(converted)
print(large_integer == converted)
```

**Output:**

```text
9007199254740993
9007199254740992.0
False
```

The float rounds to a nearby representable value. Unlike the original integer, it can no longer represent the final digit exactly.

[🔼 Back to Overview](#overview)

## Comparing Numbers Safely

Ordinary numeric equality (`==`) is appropriate when exact equality is meaningful, especially with integers. For floating-point calculations, however, small rounding differences may make two mathematically equivalent expressions compare as unequal.

Python's `math` module provides **`math.isclose(a, b, *, rel_tol=..., abs_tol=...)`**, which checks whether two values are sufficiently close under a specified tolerance.

- **`rel_tol` (relative tolerance):** An allowed difference relative to the size of the values. The default is `1e-9`.
- **`abs_tol` (absolute tolerance):** An allowed absolute difference, particularly useful when comparing values near zero. The default is `0.0`.

More precisely, it checks whether `abs(a - b) <= max(rel_tol * max(abs(a), abs(b)), abs_tol)` for ordinary finite values. Here `abs(a - b)` is the distance between the numbers, and the tolerances define how much difference is acceptable. **Choosing a tolerance is a decision about the application's acceptable error**, not a universal rule.

> [!NOTE]
> `math.isclose()` handles ordinary real-valued numeric comparisons. For complex numbers, Python provides `cmath.isclose()`.

**Example 1: Comparing floating-point results with a tolerance.**

```python
import math

result = 0.1 + 0.2

print(result == 0.3)
print(math.isclose(result, 0.3))
```

**Output:**

```text
False
True
```

Exact equality detects the small difference, while `math.isclose()` accepts it under its default tolerance.

### Comparing Values Near Zero

Relative tolerance alone is often insufficient when one value is exactly zero, because a percentage of zero is still zero. In those cases, set an appropriate **absolute tolerance**.

**Example 1: Comparing a small value with zero.**

```python
import math

small_number = 1e-10

print(math.isclose(small_number, 0.0))
print(math.isclose(small_number, 0.0, abs_tol=1e-9))
```

**Output:**

```text
False
True
```

The default absolute tolerance is zero, so the first check fails: `1e-10` is not exactly zero, and relative tolerance cannot make a nonzero value close to zero under this comparison. The second check allows an absolute difference up to `1e-9`.

[🔼 Back to Overview](#overview)

## Rounding and Formatting Numbers

**Rounding** and **formatting** solve different problems:

- **Rounding:** Produces a numeric result according to a rounding rule.
- **Formatting:** Produces text controlling how a number is displayed, such as two decimal places or comma-separated thousands.

### Understanding Rounding

Python's built-in **`round(number, ndigits)`** function rounds a number to a specified number of decimal places.

- **`number`:** The numeric value to round.
- **`ndigits`:** Optional argument specifying the decimal place to round to. Positive values retain that many places after the decimal point (`2` means hundredths), `0` means the ones place, and negative values round to tens (`-1`), hundreds (`-2`), and so on. If omitted, `round()` returns the nearest integer for ordinary floats; if `ndigits=0` is supplied, a float input produces a float result.

For ordinary values, Python rounds to the nearest candidate:

- **`2.3` rounds to `2`:** Because `2.3` is closer to `2` than `3`.
- **`2.7` rounds to `3`:** Because `2.7` is closer to `3` than `2`.

However, what happens when the number is **exactly halfway** between two integers?

For example, `2.5` is equally distant from `2` and `3`. Neither is closer, so Python needs a rule to break the tie.

Python uses **round half to even**, sometimes called *banker's rounding*.

This means that when a value is exactly halfway between two rounding candidates, Python chooses whichever candidate is **even**.

An **even integer** is divisible by `2` without a remainder, such as `2`, `4`, `6`, or `8`.

For example:

| Value | Two Nearest Integers | Even Integer | Result |
|---|---|---|---|
| `2.5` | `2` and `3` | `2` | `2` |
| `3.5` | `3` and `4` | `4` | `4` |
| `4.5` | `4` and `5` | `4` | `4` |
| `5.5` | `5` and `6` | `6` | `6` |
| `-2.5` | `-3` and `-2` | `-2` | `-2` |

**Why does Python use this rule?**

Always rounding halfway values upward can introduce a systematic upward bias when performing many calculations containing ties. Round-half-to-even helps reduce this bias by distributing halfway rounding decisions between the lower and higher candidates, depending on which is even.

> [!IMPORTANT]
> Round-half-to-even applies **only when the value is exactly halfway** between the two rounding candidates. Otherwise, Python rounds to whichever candidate is closest.
>
> Floating-point numbers are stored using binary representation, which cannot represent every decimal fraction exactly. Consequently, a value that appears to be an exact halfway case in decimal notation might not be an exact tie internally.
>
> Therefore, `round()` should not be assumed to eliminate floating-point precision errors. For example, `round(2.675, 2)` typically gives `2.67`, because the stored float is slightly below the exact decimal halfway value.

**Example 1: Understanding round-half-to-even.**

```python
print(round(2.5))
print(round(3.5))
print(round(-2.5))
```

**Output:**

```text
2
4
-2
```

Breaking down the results:

1. **`round(2.5)`:** The number is exactly halfway between `2` and `3`. Since `2` is even, Python returns `2`.
2. **`round(3.5)`:** The number is exactly halfway between `3` and `4`. Since `4` is even, Python returns `4`.
3. **`round(-2.5)`:** The number is exactly halfway between `-3` and `-2`. Since `-2` is even, Python returns `-2`.

Notice that Python does not always round halfway values upward, downward, or away from zero. It chooses the nearest even candidate. For decimal-place rounding, “even” refers to the final **retained** digit, not necessarily the whole-number portion.

**Example 2: Choosing which decimal place to round.** Positive `ndigits` values round after the decimal point; negative values round to positions before it.

```python
print(round(1234.567, 2))
print(round(1234.567, 0))
print(round(1234.567, -1))
print(round(1234.567, -2))
```

**Output:**

```text
1234.57
1235.0
1230.0
1200.0
```

Breaking down the results:

1. **`ndigits=2`:** Keeps two decimal places, so `1234.567` becomes `1234.57`.
2. **`ndigits=0`:** Rounds to the nearest whole-number value, but returns a `float` (`1235.0`) because the input is a float.
3. **`ndigits=-1`:** Rounds to the nearest ten (`1230.0`).
4. **`ndigits=-2`:** Rounds to the nearest hundred (`1200.0`).

### Formatting Numbers

The built-in **`format(value, specification)`** function and **f-string formatting** produce a string according to a format specification. They control how a number is displayed without modifying the original numeric object.

A format specification appears after the colon in an f-string: `f"{value:,.2f}"`. Here `,` requests grouping, `.2` specifies two digits after the decimal point, and `f` selects fixed-point notation. The equivalent built-in call is `format(value, ",.2f")`. Formatting may round what is **shown**, but it still produces text and does not change the original value.

Common formatting specifications include:

| Format | Meaning | Example Result |
|---|---|---|
| `.2f` | Exactly two digits after the decimal point | `12.35` |
| `,.2f` | Thousands separators and two decimal places | `12,345.68` |
| `.1%` | Percentage display with one decimal place | `25.0%` |
| `_` | Underscore grouping for integers | `1_000_000` |

**Example 3: Formatting without changing the numeric value.**

```python
price = 12345.678
ratio = 0.25

print(f"{price:,.2f}")
print(f"{ratio:.1%}")
print(type(f"{price:.2f}"))
print(price)
```

**Output:**

```text
12,345.68
25.0%
<class 'str'>
12345.678
```

Breaking down the results:

1. **`f"{price:,.2f}"`:** Formats `12345.678` with comma-separated thousands and two decimal places, producing `"12,345.68"`.
2. **`f"{ratio:.1%}"`:** Multiplies the displayed value by `100`, adds a percent sign, and shows one decimal place, producing `"25.0%"`.
3. **`type(f"{price:.2f}")`:** Returns `<class 'str'>`, confirming that formatting produces a string rather than changing the numeric type.
4. **`print(price)`:** Displays the original value `12345.678`, confirming that formatting does not modify the variable.

**Example 4: Comparing rounding and formatting.** Both operations can display similar results, but `round()` returns a numeric value while formatting returns text.

```python
price = 12.3456

rounded_price = round(price, 2)
formatted_price = f"{price:.2f}"

print(rounded_price, type(rounded_price))
print(formatted_price, type(formatted_price))
print(price)
```

**Output:**

```text
12.35 <class 'float'>
12.35 <class 'str'>
12.3456
```

Breaking down the results:

1. **`round(price, 2)`:** Produces the floating-point value `12.35`.
2. **`f"{price:.2f}"`:** Produces the string `"12.35"`, formatted with exactly two decimal places.
3. **`print(price)`:** Confirms that neither operation changed the original value stored in `price`.

Although the first two results look identical when printed, they have different data types. The rounded value can be used directly in numeric calculations, while the formatted string is intended for display.

[🔼 Back to Overview](#overview)

## Useful Built-in Numeric Functions

Python provides several built-in functions for common numeric operations:

| Function | Purpose | Example | Result |
|---|---|---|---|
| `abs(number)` | Absolute value or complex magnitude | `abs(-8)` | `8` |
| `round(number, ndigits)` | Round to a number of decimal places | `round(3.14159, 2)` | `3.14` |
| `pow(base, exponent)` | Raise a number to a power | `pow(2, 3)` | `8` |
| `pow(base, exponent, modulus)` | Efficient modular exponentiation using integers | `pow(2, 5, 7)` | `4` |
| `divmod(a, b)` | Return floor quotient and remainder | `divmod(17, 5)` | `(3, 2)` |
| `min(values)` | Find the smallest value | `min(3, 8, 1)` | `1` |
| `max(values)` | Find the largest value | `max(3, 8, 1)` | `8` |
| `sum(iterable)` | Add numeric items | `sum([2, 4, 6])` | `12` |

**Absolute value** is a number's nonnegative distance from zero. For example, `abs(-8)` is `8`, not because the input was positive, but because both `8` and `-8` are eight units from zero.

**`divmod(a, b)`** returns a **tuple** (an ordered group of values) containing `a // b` and `a % b`. It is useful when both the quotient and remainder are needed.

**Example 1: Using common numeric functions.**

```python
print(abs(-8))
print(divmod(17, 5))
print(pow(2, 3))
print(sum([2, 4, 6]))
```

**Output:**

```text
8
(3, 2)
8
12
```

Breaking down the results:

1. **`abs(-8)`:** Returns the nonnegative magnitude `8`.
2. **`divmod(17, 5)`:** Returns quotient `3` and remainder `2`, because `17 = (3 * 5) + 2`.
3. **`pow(2, 3)`:** Calculates `2 ** 3`, producing `8`.
4. **`sum([2, 4, 6])`:** Adds the list's integers, producing `12`.

### The `math` Module

The standard-library **`math` module** provides additional real-number functions and constants. Import it using `import math` before accessing its names.

Common examples include:

- **`math.sqrt()`:** Calculates the square root of a number.
- **`math.floor()`:** Rounds **down** to the nearest integer (toward negative infinity).
- **`math.ceil()`:** Rounds **up** to the nearest integer (toward positive infinity).
- **`math.pi`:** Provides the mathematical constant π (pi).

Unlike `round()`, which rounds to the nearest value, `math.floor()` and `math.ceil()` always round in their respective directions.

For example:

- **Positive number:** `math.floor(3.1)` returns `3`, while `math.ceil(3.1)` returns `4`.
- **Negative number:** `math.floor(-3.1)` returns `-4`, while `math.ceil(-3.1)` returns `-3`.

> [!NOTE]
> **Rounding down does not mean moving toward zero**, and rounding up does not mean moving away from zero. For negative numbers, rounding down makes the result more negative, while rounding up makes it less negative.

**Example 1: Using `math` functions.**

```python
import math

print(math.sqrt(81))
print(math.floor(3.9))
print(math.ceil(3.1))
print(round(math.pi, 2))
```

**Output:**

```text
9.0
3
4
3.14
```

`math.sqrt()` returns a float; `math.floor()` and `math.ceil()` return integers for ordinary finite floats; `round(math.pi, 2)` returns the floating-point value `3.14` (an approximation of π).

**Example 2: Comparing floor, ceiling, and truncation near zero.** The following example demonstrates how `math.floor()` and `math.ceil()` round positive and negative values, compared with `int()`, which truncates toward zero.

```python
import math

number = 0.9
negative_number = -0.9

print(math.floor(number))
print(math.ceil(number))
print(int(number))

print(math.floor(negative_number))
print(math.ceil(negative_number))
print(int(negative_number))
```

**Output:**

```text
0
1
0
-1
0
0
```

Breaking down the results:

1. **`math.floor(0.9)`:** Rounds down to `0`, moving toward negative infinity.
2. **`math.ceil(0.9)`:** Rounds up to `1`, moving toward positive infinity.
3. **`int(0.9)`:** Removes the fractional portion, producing `0` (toward zero).
4. **`math.floor(-0.9)`:** Rounds down to `-1`, moving away from zero toward negative infinity.
5. **`math.ceil(-0.9)`:** Rounds up to `0`, moving toward zero.
6. **`int(-0.9)`:** Removes the fractional portion, producing `0` (toward zero).

The following table summarizes the differences:

| Function | Positive `0.9` | Negative `-0.9` | Rule |
|---|---|---|---|
| `math.floor()` | `0` | `-1` | Rounds down toward negative infinity |
| `math.ceil()` | `1` | `0` | Rounds up toward positive infinity |
| `int()` | `0` | `0` | Truncates toward zero |

> [!IMPORTANT]
> **`math.floor()` always rounds down, `math.ceil()` always rounds up, and `int()` truncates toward zero.**
>
> Rounding down does not necessarily mean moving toward zero, and rounding up does not necessarily mean moving away from zero. This distinction is especially noticeable with negative numbers.

[🔼 Back to Overview](#overview)

## Numeric Limits and Special Values

Python's `int` type has no fixed bit-width limit, but its `float` type has a finite precision and range. Values outside the normal finite float range may cause overflow during conversion or calculations, or may be represented as infinity in some operations.

The **`sys.float_info`** object provides information about the platform's floating-point implementation, including maximum finite magnitude and precision characteristics. Infinity is a special float value, not a way to represent arbitrarily large **finite** numbers exactly. To inspect it, first use `import sys`, then access attributes such as `sys.float_info.max` and `sys.float_info.dig`.

Python floats also support special IEEE 754 values:

- **Positive infinity (`inf`):** A value greater than all finite floats.
- **Negative infinity (`-inf`):** A value less than all finite floats.
- **Not a Number (`nan`):** Represents an undefined or unrepresentable floating-point result in certain contexts.

You can create these values with `float("inf")`, `float("-inf")`, and `float("nan")`. They do **not** make division by zero legal in ordinary Python arithmetic: `1.0 / 0.0` still raises `ZeroDivisionError`.

One especially important rule is that **NaN is not equal to itself**. Use `math.isnan()` to check for NaN and `math.isinf()` to check for infinity.

**Example 1: Inspecting special floating-point values.**

```python
import math

positive_infinity = float("inf")
not_a_number = float("nan")

print(math.isinf(positive_infinity))
print(math.isnan(not_a_number))
print(not_a_number == not_a_number)
print(math.isfinite(5.0))
```

**Output:**

```text
True
True
False
True
```

The `math` functions distinguish infinity, NaN, and ordinary finite values. The third result demonstrates NaN's unusual equality behavior.

[🔼 Back to Overview](#overview)

## Working with Money and Decimal Numbers

Financial calculations often require **decimal arithmetic with explicit rounding rules**. Binary floating-point approximations can introduce small errors that are unacceptable when calculating prices, account balances, or taxes.

Python's standard-library **`decimal` module** provides the `Decimal` type for decimal arithmetic. A `Decimal` is **not** one of Python's three built-in numeric types; it is a separate class in the standard library.

When precision matters, construct a `Decimal` from a **string**, rather than first creating an approximate float:

- `Decimal("0.1")` represents one tenth exactly.
- `Decimal(0.1)` captures the actual binary floating-point approximation of `0.1`, which is not exactly one tenth.

For calculations that require a fixed number of decimal places, the `.quantize()` method applies a specified decimal exponent and rounding rule. This matters for money because not every domain uses the same rounding policy.

> [!IMPORTANT]
> `Decimal` improves control over decimal arithmetic, but using it does not replace the need to define currency units, appropriate rounding policies, and how results are stored. Some applications store amounts as integer cents instead.

**Example 1: Adding decimal monetary amounts exactly.**

```python
from decimal import Decimal

price = Decimal("0.10")
tax = Decimal("0.20")
total = price + tax

print(total)
print(total == Decimal("0.30"))
```

**Output:**

```text
0.30
True
```

Unlike the equivalent binary-float calculation, the addition of these exact decimal values produces exactly `0.30`.

**Example 2: Comparing decimal rounding rules.** The `Decimal.quantize()` method rounds a decimal value to a requested number of places, using an explicit rounding policy. Here `Decimal("0.01")` specifies two decimal places (cents), and `rounding=` chooses the rule:

- **`ROUND_HALF_EVEN`:** When exactly halfway between candidates, chooses the result whose last retained digit is even.
- **`ROUND_HALF_UP`:** When exactly halfway between candidates, rounds away from zero.

```python
from decimal import Decimal, ROUND_HALF_EVEN, ROUND_HALF_UP

amount = Decimal("2.345")

half_even = amount.quantize(Decimal("0.01"), rounding=ROUND_HALF_EVEN)
half_up = amount.quantize(Decimal("0.01"), rounding=ROUND_HALF_UP)

print(half_even)
print(half_up)
```

**Output:**

```text
2.34
2.35
```

Breaking down the results:

1. **`ROUND_HALF_EVEN`:** The value `2.345` is exactly halfway between `2.34` and `2.35`. The result is `2.34` because its final retained digit (`4`) is even.
2. **`ROUND_HALF_UP`:** The same halfway value is rounded away from zero, producing `2.35`.

Both results follow valid rounding rules. The appropriate choice depends on the financial calculation's requirements; it should not be left to chance.

[🔼 Back to Overview](#overview)

---

[🔼 Back to Python Basics](README.md)