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

```python
x = 5           # int
y = 3.14        # float
print(type(x))  # <class 'int'>
print(type(y))  # <class 'float'>
```

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
