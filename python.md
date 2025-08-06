# Python  

# Data Types, Strings, Slicing, and String Methods

---

## 📘 1. Python Data Types
Python has several built-in data types used to store different kinds of data.

### 🔹 Numeric Types
| Type   | Description             | Example       |
|--------|-------------------------|---------------|
| `int`  | Integer values          | `10`, `-2`    |
| `float`| Floating point numbers  | `10.5`, `3.14`|



### 🔹 Boolean Type
| Type   | Description         | Example        |
|--------|---------------------|----------------|
| `bool` | Boolean value       | `True`, `False`|

```python
is_active = True
print(type(is_active))  # <class 'bool'>
```

### 🔹 Text Type
| Type   | Description   | Example         |
|--------|---------------|-----------------|
| `str`  | String/Text   | "Hello World"  |

```python
name = "Satish"
print(type(name))  # <class 'str'>
```

### 🔹 Sequence Types
| Type     | Description                  | Example         |
|----------|------------------------------|-----------------|
| `list`   | Mutable sequence             | `[1, 2, 3]`     |
| `tuple`  | Immutable sequence           | `(1, 2, 3)`     |

### 🔹 Mapping Type
| Type     | Description         | Example               |
|----------|---------------------|-----------------------|
| `dict`   | Key-value pairs     | `{"name": "Satish"}` |

### 🔹 Set Type
| Type   | Description         | Example      |
|--------|---------------------|--------------|
| `set`  | Unordered, unique   | `{1, 2, 3}`  |

### 🔹 None Type
| Type        | Description                  | Example    |
|-------------|------------------------------|------------|
| `NoneType`  | Represents null or no value  | `None`     |

---

## 📘 2. Variable Assignment

Variables are assigned using `=` operator:
```python
x = 10
name = "satish"
pi = 3.14
is_valid = True
```

---

## 📘 3. Strings in Python

A string is a sequence of characters enclosed in quotes:
```python
text = "Hello World"
print(type(text))  # <class 'str'>
```

### String Indexing
Strings are indexed from 0.
```python
word = "Sam"
print(word[0])  # 'S'
print(word[1])  # 'a'
print(word[2])  # 'm'
```

### String Slicing
```python
word = "Python"
print(word[1:])     # 'ython'
print(word[:4])     # 'Pyth'
print(word[1:4])    # 'yth'
print(word[::-1])   # 'nohtyP' (reversed)
```

### Real Example:
```python
word = "Sam"
new = word[1:]           # 'am'
print('P' + new)         # Output: 'Pam'
```

---

## 📘 4. String Methods
Python provides several useful methods for strings:

### 🔹 `upper()` and `lower()`
```python
text = "hello world"
print(text.upper())  # 'HELLO WORLD'
print(text.lower())  # 'hello world'
```

### 🔹 `split()`
```python
text = "hello world satish"
print(text.split())       # ['hello', 'world', 'satish']
print(text.split('o'))    # ['hell', ' w', 'rld satish']
```

### 🔹 `format()`
```python
print('Hello {}'.format('satish'))
print('Hello {} {} {} {} {}'.format('satish', 'Shiva', 'and', 'Anush', 'Rehansh'))
print('Hello {0} {1} {3} {2} {4}'.format('satish', 'Shiva', 'and', 'Anush', 'Rehansh'))
print('Hello {k} {n} {a} {s} {r}'.format(s='satish', k='Shiva', a='and', n='Anush', r='Rehansh'))
```

### 🔹 Float formatting
```python
result = 77/100.2
print("The Result was {r:3.3f}".format(r=result))  # The Result was 0.769
```

### 🔹 f-strings (Python 3.6+)
```python
name = 'rockstar'
print(f'My name is {name}')  # My name is rockstar
```

---

## 📘 5. Quick Summary

- Strings are immutable, indexed, and sliceable.
- Use `upper()`, `lower()`, `split()`, `format()` to manipulate strings.
- Use string slicing to extract parts of a string.
- Use `f""` for cleaner inline formatting.

---



# Python Data Types

Python has several built-in data types, which are used to classify and manage different kinds of data. Understanding these types is crucial for writing effective code. Here are some of the most common ones.

## Numeric Types 🔢

Numeric types are used to store numerical values.

- **int**: Represents integers, which are whole numbers (positive, negative, or zero) without a decimal point.  
  Example: `age = 30`, `temperature = -5`

- **float**: Represents floating-point numbers, which are numbers with a decimal point.  
  Example: `pi = 3.14`, `price = 19.99`

- **complex**: Represents complex numbers, which have a real and an imaginary part, written with 'j' or 'J' as the imaginary unit.  
  Example: `c = 5 + 2j`

```python
x = 5           # int
y = 3.14        # float
print(type(x))  # <class 'int'>
print(type(y))  # <class 'float'>
```

## Text Type 📝

The text type, called strings, is used to store sequences of characters.

- **str**: Represents a string, which is an immutable sequence of characters enclosed in single quotes (`'...'`), double quotes (`"..."`), or triple quotes (`'''...'''` or `"""..."""`).  
  Example: `name = "Alice"`, `message = 'Hello, World!'`

```python
name = "Satish"
print(type(name))  # <class 'str'>
```
You can access individual characters or a range of characters using indexing and slicing.  
Example: `message[0]` would give `'H'`.

## Sequence Types 📦

Sequence types are ordered collections of items.

- **list**: A list is an ordered, mutable collection of items. This means you can change, add, or remove items after the list is created. Lists are defined using square brackets `[]`.  
  Example: `fruits = ["apple", "banana", "cherry"]`

- **tuple**: A tuple is an ordered, immutable collection of items. Once created, you cannot change its elements. Tuples are defined using parentheses `()`. They are generally used for data that shouldn't change.  
  Example: `coordinates = (10.0, 20.0)`

- **range**: Represents a sequence of numbers, often used in loops. The `range()` function generates a sequence of numbers and is immutable.  
  Example: `for i in range(5):` generates numbers from 0 up to (but not including) 5.

## Mapping Type 🗺️

Mapping types store data in key-value pairs.

- **dict**: A dictionary is an unordered, mutable collection of items. Each item is a key-value pair. Dictionaries are defined using curly braces `{}`. Keys must be unique and immutable (like strings or numbers), while values can be of any type.  
  Example: `person = {"name": "Bob", "age": 25}`

You access values using their keys: `person["name"]` would give `'Bob'`.

## Set Types 🧩

Set types are unordered collections of unique items.

- **set**: A set is an unordered, mutable collection of unique items. Duplicate elements are automatically removed. Sets are defined using curly braces `{}` or the `set()` function.  
  Example: `colors = {"red", "green", "blue"}`

- **frozenset**: A `frozenset` is an immutable version of a set. Once created, you cannot add or remove items. This makes them hashable, so they can be used as keys in a dictionary.  
  Example: `immutable_set = frozenset([1, 2, 3])`

## Boolean Type ✅

The Boolean type represents truth values.

- **bool**: Represents a Boolean value, which can only be `True` or `False`. These are often the result of comparison operations.  
  Example: `is_adult = True`, `is_senior = False`

`5 > 3` would evaluate to `True`.
```python
is_active = True
print(type(is_active))  # <class 'bool'>
```

## Binary Types 💾

These are used to handle binary data.

- **bytes**: An immutable sequence of single bytes.  
  Example: `x = b"hello"`

- **bytearray**: A mutable sequence of single bytes.  
  Example: `y = bytearray(5)`

- **memoryview**: Provides a memory-efficient way to access the internal data of an object without making a copy.  
  Example: `mem = memoryview(b"abc")`



































