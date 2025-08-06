# Python  

## 📘 1. Python Data Types

Python has several built-in data types, which are used to classify and manage different kinds of data. Understanding these types is crucial for writing effective code. Here are some of the most common ones.

### Numeric Types 🔢

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

### Text Type 📝

The text type, called strings, is used to store sequences of characters.

- **str**: Represents a string, which is an immutable sequence of characters enclosed in single quotes (`'...'`), double quotes (`"..."`), or triple quotes (`'''...'''` or `"""..."""`).  
Example: 

```python
`name = "Alice"`, `message = 'Hello, World!'`
name = "Satish"
print(type(name))  # <class 'str'>
```
You can access individual characters or a range of characters using indexing and slicing.  
Example: `message[0]` would give `'H'`.

### Sequence Types 📦

Sequence types are ordered collections of items.

- **list**: A list is an ordered, mutable collection of items. This means you can change, add, or remove items after the list is created. Lists are defined using square brackets `[]`.  
  Example: 
  ```python
  fruits = ["apple", "banana", "cherry"]
  mylist = ["one", "two", "three"]
  mylist[1] = "updated"
  print(mylist)
  ```
Nested List Indexing:
  ```python
  nested = [1, 1, [1, 2]]
  print(nested[2][1])  # Output: 2
  ```
- **tuple**: A tuple is an ordered, immutable collection of items. Once created, you cannot change its elements. Tuples are defined using parentheses `()`. They are generally used for data that shouldn't change.  
Example:
  ```python
  coordinates = (10.0, 20.0)
  t = ('a', 'b', 'c')
  print(t.count('a'))  # Count of 'a'
  print(t.index('c'))  # Index of 'c'
  ```
- **range**: Represents a sequence of numbers, often used in loops. The `range()` function generates a sequence of numbers and is immutable.  
  Example: `for i in range(5):` generates numbers from 0 up to (but not including) 5.

### Mapping Type 🗺️

Mapping types store data in key-value pairs.

- **dict**: A dictionary is an unordered, mutable collection of items. Each item is a key-value pair. Dictionaries are defined using curly braces `{}`. Keys must be unique and immutable (like strings or numbers), while values can be of any type.  
  Example:
  ```python
  person = {"name": "Bob", "age": 25}`
  # You access values using their keys: `person["name"]` would give `'Bob'`.
  
  my_dict = {'k1': 'v1', 'k2': ['v2', 'v3']}
  print(my_dict['k2'][1])
  
  # Retrieve keys, values, and items:
  print(my_dict.keys())
  print(my_dict.values())
  print(my_dict.items())
  ```


### Set Types 🧩

Set types are unordered collections of unique items.

- **set**: A set is an unordered, mutable collection of unique items. Duplicate elements are automatically removed. Sets are defined using curly braces `{}` or the `set()` function.  
  Example:
  ```python
  colors = {"red", "green", "blue"}`
  mylist = [1, 2, 2, 3]
  print(set(mylist))  # Output: {1, 2, 3}
  ```
- **frozenset**: A `frozenset` is an immutable version of a set. Once created, you cannot add or remove items. This makes them hashable, so they can be used as keys in a dictionary.  
  Example: `immutable_set = frozenset([1, 2, 3])`

### Boolean Type ✅

The Boolean type represents truth values.

- **bool**: Represents a Boolean value, which can only be `True` or `False`. These are often the result of comparison operations.  
  Example:
```python
is_adult = True
is_senior = False
print(type(True))   # <class 'bool'>
print(3 > 2)        # True
```

`5 > 3` would evaluate to `True`.
```python
is_active = True
print(type(is_active))  # <class 'bool'>
```

### Binary Types 💾

These are used to handle binary data.

- **bytes**: An immutable sequence of single bytes.  
  Example: `x = b"hello"`

- **bytearray**: A mutable sequence of single bytes.  
  Example: `y = bytearray(5)`

- **memoryview**: Provides a memory-efficient way to access the internal data of an object without making a copy.  
  Example: `mem = memoryview(b"abc")`

## 📘 2. Variable Assignment in Python

In Python, variable assignment is the process of giving a name to a value. The variable acts as a label or a container for the data, allowing you to refer to it and manipulate it throughout your program. You don't need to declare the variable's type beforehand; Python automatically infers the data type based on the assigned value.

### How to Assign a Variable

The assignment process uses the equals sign (=). The variable name goes on the left, and the value you're assigning goes on the right.

```python
# The variable 'age' is assigned the integer value 30.
age = 30

# The variable 'name' is assigned the string value "Alice".
name = "Alice"

# The variable 'is_student' is assigned the boolean value True.
is_student = True
```
You can also assign the same value to multiple variables at once, which is a convenient shortcut.

```python
x = y = z = 100
# All three variables (x, y, and z) now have the value 100.
```
### Reassigning Variables
Variables are "mutable," which means you can change the value assigned to them at any time.
```python
# Initial assignment
score = 95

# Reassigning the variable 'score' to a new value
score = 100

print(score)  # Output: 100
```

### Best Practices for Variable Names
**Be descriptive:** Use names that clearly indicate the variable's purpose. For example, use user_name instead of un.

**Use snake_case:** For multi-word variable names, use lowercase letters and separate words with underscores. For example, total_price.

**Follow Python's naming conventions:** Variable names cannot start with a number and can only contain alphanumeric characters and underscores. Also, they are case-sensitive (my_variable is different from My_Variable).

**Avoid using reserved keywords:** Don't use names that are already part of Python's syntax, such as if, else, for, while, class, or def.

some examples of variable assignment with different data types to illustrate the concept.

```python
# Numeric Assignment
# Integer
num_students = 25

# Float
average_score = 88.5

# String Assignment

# Single quotes
first_name = 'John'

# Double quotes
last_name = "Doe"

# You can combine strings using the '+' operator
full_name = first_name + " " + last_name
print(full_name) # Output: John Doe

# List Assignment

# Assigning a list of strings
fruits = ["apple", "banana", "cherry"]

# Accessing an element in the list
print(fruits[0]) # Output: apple
# Dictionary Assignment

# Assigning a dictionary
user_profile = {
    "username": "coder_2024",
    "email": "coder@example.com"
}

# Accessing a value using its key
print(user_profile["email"]) # Output

```
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





















