````markdown
# Programming in Science – Python Style Guide

This style guide is for the **Programming in Science (420-SN1-RE)** course.

It covers only things you actually **see and do in this course**:

- Basic Python syntax
- Variables, arithmetic, input/output
- Conditionals and loops
- Functions (including simple recursion)
- Lists and 2D lists (arrays/matrices)
- File I/O (text, CSV)
- NumPy arrays
- Matplotlib plots
- Simple testing/debugging and GitHub Classroom patterns


## 1. General Formatting

### Indentation

- Use **4 spaces** per indentation level.
- Never mix tabs and spaces.

  
```python
if speed > 0:
    distance = speed * time

    print(distance)
````

### Line length

* Aim for lines of **79 characters or less**.

* For long lines (e.g., long function calls), break them across lines using parentheses.

```python
result = compute_free_fall_height(
    initial_height = h0,
    time_seconds = t,
    gravity = G,
)
```

### Blank lines

* Separate logical sections of code with a **single blank line**.

* Leave a blank line between function definitions.

```python
def compute_speed(distance: float, time: float) -> float:
    return distance / time

def compute_height(h0: float, t: float, g: float = 9.8) -> float:
    return h0 - 0.5 * g * t**2
```

## 2. Naming Rules (Variables, Functions, Constants)

Use names that match the math or physics you are coding.

### Variables and functions

* Use **snake_case**: `distance`, `initial_height`, `compute_speed`.

* Names should say *what the value represents*.

```python
time_seconds = 3.0
height_meters = 50.0

def free_fall_height(h0: float, t: float, g: float = 9.8) -> float:
    return h0 - 0.5 * g * t**2
```

### Constants

* Use **UPPER_CASE** for constants (especially physics constants).

```python
G = 9.8          # gravitational acceleration (m/s^2)
PI = 3.14159265
```

### Booleans

* Use names that read like questions: `is_valid`, `is_prime`, `has_converged`.

```python
if is_prime(n):
    print("n is prime")
```

## 3. Basic Statements, Expressions & I/O

### One idea per line

* Do **not** chain multiple statements on one line with `;`.

```python
# ✅ Clear
distance = speed * time
print(distance)

# ❌ Messy
distance = speed * time; print(distance)
```

### Input and output

* Use `input()` in the **top-level script**, not inside reusable functions.

* Convert to the proper type immediately.

```python
time_str = input("Enter time (s): ")
time_seconds = float(time_str)

print(f"Time: {time_seconds} s")
```

### Course rule

* **Functions should take parameters and return values**.
* Avoid having functions call `input()` or `print()` internally so they can be tested easily and reused.

## 4. Whitespace

### Around operators and commas

* Put spaces **around binary operators** and **after commas**.

```python
height = h0 - 0.5 * G * t**2
position = (x, y)
value = func(a, b, c)
```

### No random spaces

* Don’t put spaces before colons or inside parentheses unnecessarily.

```python
# ✅ Good
if t >= 0:
    height = h0 - 0.5 * G * t**2

# ❌ Bad
if t >= 0 :
    height=h0-0.5*G*t**2
```

## 5. Comments & Docstrings

You use comments and docstrings to explain your scientific code and to make testing easier.

### Comments

* Explain **why**, not what is obvious from the code.

```python
# Using constant gravity g = 9.8 m/s^2
height = h0 - 0.5 * G * t**2
```

### Docstrings for functions

* Short description on the first line.

* Blank line.

* `Returns:` on its own line with a short explanation.

```python
def free_fall_height(h0: float, t: float, g: float = 9.8) -> float:
    """Compute height in free fall after time t.

    Returns:
        float: Height (meters) at time t.
    """
    return h0 - 0.5 * g * t**2
```

Use docstrings for:

* Functions you reuse in multiple files/scripts.
* Functions that will be tested by GitHub Classroom / `pytest`.

## 6. Conditionals (if / elif / else)

### Basic pattern

* Use `if`, `elif`, `else` to handle cases clearly.

```python
if t < 0:
    print("Time cannot be negative.")
elif t == 0:
    print("Start of experiment.")
else:
    print("Valid time.")
```

### Boolean conditions

* Don’t do `if is_prime(n) == True:` — just use the boolean directly.

```python
if is_prime(n):
    print("Prime")
```

### Example: day-of-week classification

```python
def day_type(day_number: int) -> str:
    """Classify the day as Weekday or Weekend.

    Returns:
        str: Message about the type of day.
    """
    if day_number < 1 or day_number > 7:
        return "Not a proper day number!"

    if 1 <= day_number <= 5:
        return "It is a Weekday!"
      
    return "It is a Weekend!"
```

## 7. Loops (for & while)

### Use `for` when you know how many times

```python
total = 0
for n in range(1, 11):
    total += n
```

### Use `while` when looping until a condition

```python
row = 1
while row <= n:
    print("*" * row)
    row += 1
```

### Avoid infinite loops

* Always change something that affects the `while` condition.

## 8. Functions (User-Defined)

You write your own functions for things like `factorial`, `fibonacci`, `is_prime`, and physics formulas.

### Function style

* Name describes what it does.

* Docstring with a blank line before `Returns:`.

* Function **returns** a value, does not just print.

```python
def factorial(n: int) -> int:
    """Compute n! (factorial) for a non-negative integer n.

    Returns:
        int: The factorial of n.
    """
    if n < 0:
        raise ValueError("n must be non-negative")

    if n in (0, 1):
        return 1

    return n * factorial(n - 1)
```

### Example: prime test

```python
def is_prime(n: int) -> bool:
    """Return True if n is prime, else False.

    Returns:
        bool: True if n is prime, False otherwise.
    """
    if n < 2:
        return False

    i = 2
    while i * i <= n:
        if n % i == 0:
            return False
        i += 1

    return True
```

## 9. Lists & 2D Lists (Arrays / Matrices)

You use lists for sequences, and 2D lists for matrices and magic squares.

### Indexing and iterating

```python
values = [1, 2, 3, 4]

first = values[0]
last = values[-1]

for value in values:
    print(value)
```

### With indexes (when needed)

```python
for i, value in enumerate(values):
    print(i, value)
```

### 2D lists (matrix)

```python
matrix = [
    [8, 1, 6],
    [3, 5, 7],
    [4, 9, 2],
]

center = matrix[1][1]
```

### Modifying lists

* If you are removing items based on a condition, it’s often better to **build a new list** instead of modifying in-place while iterating.

## 10. NumPy Arrays

You use NumPy for numeric arrays and reading scientific data from files.

### Import

```python
import numpy as np
```

### Creating arrays

```python
data = np.array([1.0, 2.0, 3.0])

tensions = np.array([10.0, 20.0, 30.0])
MU = 1.0  # linear mass density
speeds = np.sqrt(tensions / MU)
```

### Reading from file with NumPy

```python
def read_values_from_file(filename: str) -> np.ndarray:
    """Read one numeric value per line into a NumPy array.

    Returns:
        np.ndarray: 1D array of float values.
    """
    return np.loadtxt(filename, dtype=float)
```

## 11. File I/O (Text, CSV)

You read/write text files and CSVs that contain experimental data.

### Text files with context manager

```python
def write_numbers(filename: str, numbers: list[int]) -> None:
    """Write one integer per line to a text file.

    Returns:
        None: Writes to the given file.
    """
    with open(filename, "w", encoding="utf-8") as f:
        for n in numbers:
            f.write(f"{n}\n")
```

### CSV with the `csv` module

```python
import csv

def read_lengths_and_amplitudes(filename: str) -> list[list[float]]:
    """Read [length, amplitude] rows from a CSV file.

    Returns:
        list[list[float]]: Each row is [length, amplitude].
    """
    rows: list[list[float]] = []

    with open(filename, newline="", encoding="utf-8") as f:
        reader = csv.reader(f)
        next(reader, None)  # skip header if present

        for row in reader:
            length = float(row[0])
            amplitude = float(row[1])
            rows.append([length, amplitude])

    return rows
```

## 12. Plotting with Matplotlib

You use Matplotlib for line plots, scatter plots (e.g., HR diagrams), and density plots.

### Standard import

```python
import matplotlib.pyplot as plt
```

### Line plot

```python
def plot_height_vs_time(time_values, height_values) -> None:
    """Plot height vs time for free fall.

    Returns:
        None: Shows the plot.
    """
    plt.plot(time_values, height_values)
    plt.xlabel("Time (s)")
    plt.ylabel("Height (m)")
    plt.title("Free Fall Height vs Time")
    plt.show()
```

### Scatter plot (e.g., HR diagram)

```python
def plot_hr_diagram(temperature, luminosity) -> None:
    """Plot an HR diagram with temperature vs luminosity.

    Returns:
        None: Shows the plot.
    """
    plt.scatter(temperature, luminosity)
    plt.gca().invert_xaxis()  # hotter stars on the left
    plt.xlabel("Temperature (K)")
    plt.ylabel("Luminosity")
    plt.title("HR Diagram")
    plt.show()
```

### Density (2D histogram)

```python
def plot_density(points) -> None:
    """Plot a 2D density heat map from (x, y) points.

    Returns:
        None: Shows the plot.
    """
    xs = [p[0] for p in points]
    ys = [p[1] for p in points]

    plt.hist2d(xs, ys, bins=50)
    plt.xlabel("x")
    plt.ylabel("y")
    plt.colorbar(label="Density")
    plt.title("Density Plot")
    plt.show()
```

## 13. Testing & Debugging (Course-Level)

You use **manual tests** and **GitHub Classroom feedback** (and possibly `pytest` behind the scenes).

### Use known values

* Test your functions with values where you can compute the answer by hand.

```python
print(free_fall_height(50, 1))  # expected ≈ 45.1
print(free_fall_height(50, 2))  # expected ≈ 30.4
print(free_fall_height(50, 3))  # expected ≈ 5.9
```

### Edge cases to test

* `t = 0`
* Negative inputs (should be rejected or handled clearly)
* Very small array sizes: `n = 0`, `n = 1`

## 14. Common Anti-Patterns in This Course (Fix → Into)

These are mistakes that **break GitHub Classroom tests** or make code hard to reuse.

### 14.1 Printing inside functions instead of returning

```python
# ❌ Fix this
def sum_even(numbers):
    total = 0
    for n in numbers:
        if n % 2 == 0:
            total += n
    
    print(total)  # only prints, no return
```

```python
# ✅ Into this
def sum_even(numbers):
    """Sum all even numbers in the list.

    Returns:
        int: Sum of even values.
    """
    total = 0
    for n in numbers:
        if n % 2 == 0:
            total += n
    
    return total
```

### 14.2 Using input() inside logic functions

```python
# ❌ Fix this
def distance_traveled():
    t = float(input("Time in seconds: "))
    
    return 20 * t
```

```python
# ✅ Into this
def distance_traveled(time_seconds: float) -> float:
    """Compute distance for constant 20 m/s speed.

    Returns:
        float: Distance in meters.
    """
    return 20 * time_seconds
```

### 14.3 Repeating the same formula instead of using a function

```python
# ❌ Fix this
h1 = h0 - 0.5 * G * t1**2
h2 = h0 - 0.5 * G * t2**2
h3 = h0 - 0.5 * G * t3**2
```

```python
# ✅ Into this
def free_fall_height(h0: float, t: float, g: float = 9.8) -> float:
    """Compute height in free fall.

    Returns:
        float: Height (meters) at time t.
    """
    return h0 - 0.5 * g * t**2


h1 = free_fall_height(h0, t1)
h2 = free_fall_height(h0, t2)
h3 = free_fall_height(h0, t3)
```

### 14.4 Off-by-one errors with range()

```python
# ❌ Fix this – stops at n-1
total = 0
for i in range(1, n):
    total += i
```

```python
# ✅ Into this – includes n
total = 0
for i in range(1, n + 1):
    total += i
```

### 14.5 Modifying a list while iterating over it

```python
# ❌ Fix this
for value in values:
    if value < 0:
        values.remove(value)
```

```python
# ✅ Into this – build a new list
def remove_negatives(values: list[float]) -> list[float]:
    """Return a new list with only non-negative values.

    Returns:
        list[float]: Filtered values.
    """
    result: list[float] = []
    for value in values:
        if value >= 0:
            result.append(value)
    
    return result
```

### 14.6 Plotting without labels or title

```python
# ❌ Fix this
plt.plot(time_values, height_values)
plt.show()
```

```python
# ✅ Into this
plt.plot(time_values, height_values)
plt.xlabel("Time (s)")
plt.ylabel("Height (m)")
plt.title("Free Fall Height vs Time")
plt.show()
```