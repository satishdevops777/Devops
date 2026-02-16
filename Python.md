# Python  

## 📘 1. Python Data Types

Python has several built-in data types, which are used to classify and manage different kinds of data. Understanding these types is crucial for writing effective code. Here are some of the most common ones.

### Numeric Types 🔢

Numeric types are used to store numerical values.

- **int**: Represents integers, which are whole numbers (positive, negative, or zero) without a decimal point.  
  Example: `age = 30`, `temperature = -5`

- **float**: Represents floating-point numbers, which are numbers with a decimal point.  
  Example: `pi = 3.14`, `price = 19.99`
```python
x = 5           # int
y = 3.14        # float
print(type(x))  # <class 'int'>
print(type(y))  # <class 'float'>
```

- **complex**: Represents complex numbers, which have a real and an imaginary part, written with 'j' or 'J' as the imaginary unit.  
  Example: `c = 5 + 2j`


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

string[start : stop : step]
start → not given → start from beginning
stop → not given → go till end
step → -1

text = "hello"

print(text[::-1])    # olleh
print(text[::2])     # hlo  (every 2nd letter)
print(text[1:4])     # ell

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

{variable:width.precisionf}

```

### 🔹 f-strings (Python 3.6+)
```python
name = 'rockstar'
print(f'My name is {name}')  # My name is rockstar
```

---

## 📘 5. File I/O in Python

File I/O (Input/Output) in Python refers to reading from and writing to files on your computer's storage. It's a fundamental concept that allows programs to interact with external data sources. The process involves three main steps: opening a file, performing operations (reading or writing), and closing the file.

### 1. Opening a File 📂
To work with a file, you first need to open it. The open() function is used for this purpose. It takes two primary arguments: the file path and the mode.

```Python

file = open("example.txt", "w")
```
The second argument, the mode, specifies what you intend to do with the file.

**'r' (Read):** Opens a file for reading. The file must exist. This is the default mode.

**'w' (Write):** Opens a file for writing. It creates a new file if one doesn't exist, or overwrites the contents of an existing file. Be careful with this mode!

**'a' (Append):** Opens a file for appending. It creates a new file if one doesn't exist, but if it does, it adds new content to the end of the file without overwriting.

**'x' (Create):** Creates a new file. If the file already exists, the operation fails with a FileExistsError.

**'t' (Text):** Opens in text mode. This is the default.

**'b' (Binary):** Opens in binary mode, for working with non-text files like images or executables.

You can combine modes, for example, 'rb' for reading a binary file.

### 2. Reading and Writing Files ✍️
Once a file is open, you can perform I/O operations.

**Writing to a File**
You use the .write() method to write data to an open file. It's important to note that you must provide the data as a string.

```Python

# Open the file in write mode
file = open("notes.txt", "w")

# Write a single line
file.write("Hello, this is my first line.\n")

# Write another line
file.write("This is the second line.")

# Don't forget to close the file!
file.close()
```
**Reading from a File**
There are a few common ways to read data from a file:

**.read():** Reads the entire contents of the file as a single string.

**.readline():** Reads a single line from the file.

**.readlines():** Reads all lines from the file and returns them as a list of strings.

Here’s an example using these methods:

```Python

# Open the file in read mode
file = open("notes.txt", "r")

# Read the entire content
content = file.read()
print(content)

# To read line by line, you can use a for loop (most common method)
# This is a very efficient way to handle large files.
for line in file:
    print(line)

# Don't forget to close the file!
file.close()
```
### 3. Closing a File 🔒
After you're done with a file, it's crucial to close it using the .close() method. This frees up system resources and ensures that all changes are saved. Failing to close a file can lead to data loss or other issues.

***Best Practice: The with Statement** ✅
A more reliable and recommended way to handle files is using the with statement. This approach automatically handles closing the file, even if errors occur. It's cleaner and safer than manually calling .close().

```Python

# Writing to a file using 'with'
with open("shopping_list.txt", "w") as f:
    f.write("Milk\n")
    f.write("Bread\n")
    f.write("Eggs\n")

# Reading from the same file using 'with'
with open("shopping_list.txt", "r") as f:
    for item in f:
        print(item.strip()) # .strip() removes leading/trailing whitespace, including the newline character

```

## 📘 6.Comparison Operators in Python
Comparison operators are used to compare two values and evaluate them to a Boolean result: True or False. These operators are fundamental for making decisions in code, such as in conditional statements and loops.

**Basic Comparison Operators**
Operator	Name	Example	Result
==	Equal	5 == 5	True
!=	Not equal	5 != 3	True
>	Greater than	5 > 3	True
<	Less than	5 < 3	False
>=	Greater than or equal to	5 >= 5	True
<=	Less than or equal to	5 <= 3	False

Export to Sheets
Examples in Code
Here are some practical examples of how comparison operators work.

**Using Integers and Floats**
Comparison operators can be used with both integers and floats.

```Python

x = 10
y = 20
z = 10.0

print(x == y)   # False
print(x < y)    # True
print(x >= z)   # True (10 is equal to 10.0)
print(x != z)   # False (10 is not unequal to 10.0)
```
**Using Strings**
Strings can also be compared. The comparison is based on lexicographical order (alphabetical order).

```Python

str1 = "apple"
str2 = "banana"

print(str1 == str2)    # False
print(str1 < str2)     # True ('a' comes before 'b')
print("cat" > "car")   # True ('t' comes after 'r')
```
Chaining Comparison Operators
You can chain multiple comparison operators together for a more compact expression. This is a unique feature of Python.

```Python

x = 5

print(1 < x < 10)  # True (This is equivalent to: (1 < x) and (x < 10))
print(1 > x < 10)  # False (1 > 5 is False)
```
**Identity vs. Equality**
It is important to distinguish between equality (==) and identity (is).

== checks if the values of two objects are equal.

is checks if two variables refer to the exact same object in memory.

```Python

list1 = [1, 2, 3]
list2 = [1, 2, 3]
list3 = list1

print(list1 == list2)  # True (The values are equal)
print(list1 is list2)  # False (They are separate objects in memory)
print(list1 is list3)  # True (list3 is a reference to the same object as list1)

```



## 📘 7. Conditional Statements
Conditional statements let your program make decisions based on specific conditions. The primary structure uses if, elif (else if), and else.

```Python

if 3 > 2:
    print('Its true')

# The `if-else` structure
hungry = False
if hungry:
    print('Feed me')
else:
    print('I am not hungry')

# The `if-elif-else` structure
loc = 'School'
if loc == 'School':
    print(f'{loc} is mine')
elif loc == 'hostel':
    print('I own the hostel')
else:
    print('nothing')
```

## 📘 8. Loops
Loops are used to repeatedly execute a block of code.

**For Loop**
The for loop iterates over a sequence (like a list, string, or range).

```Python

mylist = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
for num in mylist:
    print(num)

# Using a for loop with a conditional statement
for num in mylist:
    if num % 2 == 0:
        print(f'even num: {num}')
    else:
        print(f'Odd number: {num}')

# Summing items in a list (Note: The user's example had a typo, here's the corrected version)
list_sum = 0
for num in mylist:
    list_sum = list_sum + num
    print(list_sum)

# Iterating through a string
mystring = 'Hello World'
for letter in mystring:
    print('Cool!')

# Iterating through a tuple
tup = (1, 2, 3)
for item in tup:
    print(item)

# Tuple unpacking with a for loop
mylist = [(1, 3), (5, 7), (8, 9)]
print(len(mylist))
for (a, b) in mylist:
    print(a)

# Iterating through a dictionary
dic = {'k1': 2, 'k2': 3, 'k3': 5}
for key, value in dic.items():
    print(key, value)
```
**While Loop**
While loops will continue to execute a block of code while some condition remains true.

Syntax:

```Python

while some_boolean_condition:
    # Do something
# A while loop can also have an optional else block.


while some_boolean_condition:
    # Do something
else:
    # Do something


x = 0
while x < 5:
    print(f'The current value of x is {x}')
    x = x + 1  # or x += 1
else:
    print("X is not less than 5")
```
**Loop Control: break, continue, pass**
**break:** Breaks out of the current closest enclosing loop.
Example: break
```Python

mystr = 'sammy'
for letter in mystr:
    if letter == 'a':
        break
    print(letter)

x = 0
while x < 10:
    if x == 6:
        break
    print(x)
    x += 1
```
**continue:** Goes to the top of the closest enclosing loop.
Example: continue

```Python

mystr = 'sammy'
for letter in mystr:
    if letter == 'a':
        continue
    print(letter)
```
**pass:** Does nothing at all.

Example: pass

```Python

x = [1, 2, 3]
for item in x:
    # with pass we can avoid syntax error for an empty loop
    pass
print('end of my script')
```


## 📘 9. Useful Operators in Python
Python has many built-in functions and operators that simplify common tasks.

**range()**
The range() function generates a sequence of numbers.

```Python

# range(stop) - generates numbers from 0 up to stop-1
for num in range(10):
    print(num)

# range(start, stop)
for num in range(3, 10):
    print(num)

# range(start, stop, step)
for num in range(0, 10, 2):
    print(num)

# Converting range to a list
print(list(range(0, 11, 2)))
```
**enumerate()**
The enumerate() function provides both an index and a value during a loop.

```Python

index_count = 0
for letter in 'abcde':
    print('At index {} the letter is {}'.format(index_count, letter))
    index_count += 1

# The same task using enumerate
word = 'abcde'
for index, letter in enumerate(word):
    print(index)
    print(letter)
    print('\n')
```
**zip()**
The zip() function combines multiple iterables into an iterator of tuples.

```Python

mylist1 = [1, 2, 3]
mylist2 = ['a', 'b', 'c']
for item in zip(mylist1, mylist2):
    print(item)

# If lists are of different lengths, zip stops at the shortest one
mylist1 = [1, 2, 3, 5, 6]
mylist2 = ['a', 'b', 'c']
for item in zip(mylist1, mylist2):
    print(item)
```
**in Operator (Membership)**
The in operator checks if an item is in a sequence.

```Python

print('x' in ['x', 'y', 'z'])
print('a' in 'a word')
print('mykey' in {'mykey': 345})
```
**min() and max()**
These functions find the smallest and largest values in an iterable.

```Python
mylist1 = [1, 2, 3, 5, 6]
print(min(mylist1))
print(max(mylist1))
```
**random Module**
The random module is used for generating random values.

```Python

from random import shuffle, randint

mylist = [1, 2, 3, 4, 5, 6, 7, 8, 9]
shuffle(mylist)
print(mylist) # Prints a shuffled version of the list

my_num = randint(0, 1000)
print(my_num) # Prints a random integer between 0 and 1000
```
**input()**
The input() function prompts the user for a value and returns it as a string.

```Python
result = input('Enter a number here: ')
print(result)
```
## 📘 10. List Comprehensions
List comprehensions are a unique and concise way to quickly create a list in Python. They are often more readable and efficient than a traditional for loop.

```Python

# Traditional approach
mystring = 'Hello'
mylist = []
for letter in mystring:
    mylist.append(letter)

# List comprehension equivalent
mylist = [letter for letter in mystring]

# Creating a list of squares
mylist = [num**2 for num in range(0, 11)]

# Creating a list with a conditional filter
mylist = [num for num in range(0, 11) if num % 2 == 0]

# Converting Celsius to Fahrenheit
celcius = [0, 10, 20, 30]
fahr = [(9/5) * temp + 32 for temp in celcius]
print(fahr)

# Nested List Comprehensions
mylist = [x * y for x in [2, 4, 6] for y in [100, 200, 300]]
```

## 📘 11. Methods and Functions
**def Keyword**
Functions are blocks of reusable code created using the def keyword. Good function names use snake casing (lowercase letters with underscores).

```Python

def name_of_function():
    """
    Docstring: This is where the function's explanation goes.
    """
    print("Hello")
name_of_function()
```
The return keyword is typically used to send a result back from the function, which allows you to assign the output to a variable.

```Python

# Function that just prints
def print_result(a, b):
    print(a + b)
print_result(10, 20) # Output: 30

# Function that returns a value
def return_result(a, b):
    return a + b
result = return_result(10, 20)
print(result) # Output: 30
```
**Functions with Logic**
You can define functions that perform a specific task, like checking if a number is even.

```Python

def even_check(number):
    return number % 2 == 0
print(even_check(20)) # True
```
Returning a Value from a List
Here are two examples of functions that process a list, showing different return behaviors.

Example 1: Check if any number is even

```Python

def even_list(num_list):
    for num in num_list:
        if num % 2 == 0:
            return True # Returns True as soon as an even number is found
    return False # Only returns False if the loop completes without finding an even number
print(even_list([1, 5, 3])) # False
```
Example 2: Return all even numbers in a list

```Python
def even_list(num_list):
    even_numbers = []
    for num in num_list:
        if num % 2 == 0:
            even_numbers.append(num)
    return even_numbers
print(even_list([1, 9, 6, 4])) # [6, 4]
```

## 📘 12. What are *args and **kwargs?

- **args** → lets you pass any number of positional arguments to a function (stored as a tuple).
- **kwargs** → lets you pass any number of keyword arguments to a function (stored as a dictionary).

Think of them as a way to make functions flexible without hardcoding the number of inputs.
- Example: args
```python
def install_packages(*args):
    for pkg in args:
        print(f"Installing {pkg}...")

install_packages("nginx", "docker", "git")
```
- Example: kwargs
```python
def deploy_service(**kwargs):
    for key, value in kwargs.items():
        print(f"{key} => {value}")

deploy_service(app="web", version="1.0.2", replicas=3)
```

- *args and **kwargs together
```python
def configure_server(*args, **kwargs):
    print("Packages to install:", args)
    print("Configuration settings:", kwargs)

configure_server("nginx", "docker", user="admin", port=8080)
```
## 📘 13. Displaying Informtion

```python
a = 100
print(a)
```
### Accepting User Input
- Input function always return a str type we can convert if we want to
```python
result = input("Plese enter a value: ")
print(result)
type(result)
```

### Validating User Input
```python
def user_choice():
    choice = 'Wrong'
    while choice.isdigit() == False:
        choice = input("Please enter a number (0-10): ")
        if choice.isdigit() == False:
            print("Sorry that is not a digit")
    return int(choice)
user_choice()
```
## 📘 14. Modulus and packages
**1. What is a Module in Python?**
- A module is simply a Python file (.py) that contains code (functions, classes, variables) you can reuse.

Think:
- A single .py file = 1 module.

Example — Creating and Importing a Module
📁 server_utils.py
```python
# server_utils.py
def start_server(name):
    print(f"Starting server: {name}")

def stop_server(name):
    print(f"Stopping server: {name}")
```
📁 main.py
```python
# main.py
import server_utils

server_utils.start_server("nginx")
server_utils.stop_server("nginx")
```
Output:
```yaml
Starting server: nginx
Stopping server: nginx
```

**2. What is a Package in Python?**
- A package is a collection of modules inside a folder that contains a special file __init__.py.

Think:
- Folder with multiple .py files (modules) + __init__.py = package.
Example — Creating and Using a Package
📁 Folder Structure:
```markdown
devops_tools/
    __init__.py
    aws_utils.py
    k8s_utils.py
main.py
```

📁 aws_utils.py

```python
def list_s3_buckets():
    print("Listing S3 buckets...")

def create_s3_bucket(name):
    print(f"Creating S3 bucket: {name}")
```
📁 k8s_utils.py
```python
def list_pods():
    print("Listing Kubernetes pods...")

def deploy_pod(name):
    print(f"Deploying pod: {name}")
```

📁 main.py
```python
from devops_tools import aws_utils, k8s_utils

aws_utils.list_s3_buckets()
k8s_utils.list_pods()
```
Output:

```nginx
Listing S3 buckets...
Listing Kubernetes pods...
```
**3. Key Differences**
| **Module**                             | **Package**                                             |
| -------------------------------------- | ------------------------------------------------------- |
| Single `.py` file                      | Folder with multiple `.py` files                        |
| Used for smaller, related functions    | Used for organizing large projects                      |
| Imported directly (`import file_name`) | Imported via folder path (`from package import module`) |

**4. Why DevOps Engineers Care**
- Modules → Organize automation scripts for different tasks (EC2, S3, CI/CD).
- Packages → Create reusable DevOps toolkits for teams.
- Keeps large automation projects structured.

**5. Real DevOps Example**
You might have:
```markdown
infra_automation/
    __init__.py
    aws.py
    docker.py
    kubernetes.py
```
- ```aws.py``` → Functions for EC2, S3
- ```docker.py``` → Functions for building & pushing images
- ```kubernetes.py``` → Functions for deployments & services
You import only what you need:
```python
from infra_automation import aws, docker
aws.create_ec2_instance("t2.micro")
docker.build_image("myapp")
```

## 📘 15. __name__ == "__main__"

**1. What does __name__ do?**
- Every Python file has a built-in variable called __name__.
- If you run a Python file directly, Python sets __name__ = "__main__".
- If you import that file into another script, __name__ will be set to the module’s name (filename without .py).

**2. Why use if __name__ == "__main__":?**
This lets you write code that:
- Runs only when the file is executed directly.
- Does not run when the file is imported as a module.

**3. Example — Without __main__**
📁 server.py
```python
print("Starting server...")

def start():
    print("Server is running.")
```
📁 main.py
```python
import server
print("Doing other tasks...")
```
Output:
```nginx
Starting server...       # <-- Runs even though we just imported
Doing other tasks...
```
❌ This is bad because importing the file starts the server automatically.

**4. Example — With if __name__ == "__main__":**
📁 server.py
```python
def start():
    print("Server is running.")

if __name__ == "__main__":
    print("Starting server...")
    start()
```
📁 main.py
```python
import server
print("Doing other tasks...")
```
***Case 1 — Run python server.py***
```nginx
Starting server...
Server is running.
```
***Case 2 — Run python main.py***
```nginx
Doing other tasks...
```
✅ Now the server doesn’t auto-run when imported.

**5. Why DevOps Engineers Use This**
- To make scripts that can be imported as a library or run as a standalone tool.
- Example: You might have aws_utils.py with functions, but only run them if the file is executed directly.
```python
# aws_utils.py
import boto3

def list_buckets():
    s3 = boto3.client('s3')
    for bucket in s3.list_buckets()['Buckets']:
        print(bucket['Name'])

if __name__ == "__main__":
    print("Listing buckets from CLI...")
    list_buckets()
```
You can run:
```bash
python aws_utils.py
```

- Lists buckets immediately.
- Or import it into another script:

```python
from aws_utils import list_buckets
list_buckets()
```
- No unwanted extra output.

## 📘 16. Errors and exception handling
- Errors are bound to happen in code.
- We can use error handling to attempt to plan for possible errors
- We can use error handling to let the script continue with other code, even if there is an error
- We use three keywords for this:
-- try: This is the block of code to be attempted (May lead to an error)
-- except: Block of code will execute in case there is an error in try block
-- finally: A final block of code to be executed, regardless of an error

```python
def add():
    try:
        n1 = int(input("Please enter a number: "))
        n2 = int(input("Please enter a number: "))
        result = n1 + n2
    except ValueError:
        print("Invalid input! Please enter numeric values.")
    else:
        print("Addition went well")
        print("Result:", result)

add()
```
```python
try:
    f = open ('testfile','r')
    f.write("Write a test file")
except TypeError:
    print("There was a type error!")
except OSError:
    print('Hey you have an OS Error')
finally:
    print("I always run")
```
'''
**Unit Testing**
- pylint: This is a library that looks at your code and reports back possible issues
- unittest: This built-in library will allow to test your own programs and check you are getting desired outputs.
```python
# Install pylint
pip install pylint
```


---
---
---


# Python  

## 📘 1. Python Data Types

Python has several built-in data types, which are used to classify and manage different kinds of data. Understanding these types is crucial for writing effective code. Here are some of the most common ones.

### Numeric Types 🔢

Numeric types are used to store numerical values.

- **int**: Represents integers, which are whole numbers (positive, negative, or zero) without a decimal point.  
  Example: `age = 30`, `temperature = -5`

- **float**: Represents floating-point numbers, which are numbers with a decimal point.  
  Example: `pi = 3.14`, `price = 19.99`
```python
x = 5           # int
y = 3.14        # float
print(type(x))  # <class 'int'>
print(type(y))  # <class 'float'>
```

- **complex**: Represents complex numbers, which have a real and an imaginary part, written with 'j' or 'J' as the imaginary unit.  
  Example: `c = 5 + 2j`


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

## 📘 5. File I/O in Python

File I/O (Input/Output) in Python refers to reading from and writing to files on your computer's storage. It's a fundamental concept that allows programs to interact with external data sources. The process involves three main steps: opening a file, performing operations (reading or writing), and closing the file.

### 1. Opening a File 📂
To work with a file, you first need to open it. The open() function is used for this purpose. It takes two primary arguments: the file path and the mode.

```Python

file = open("example.txt", "w")
```
The second argument, the mode, specifies what you intend to do with the file.

**'r' (Read):** Opens a file for reading. The file must exist. This is the default mode.

**'w' (Write):** Opens a file for writing. It creates a new file if one doesn't exist, or overwrites the contents of an existing file. Be careful with this mode!

**'a' (Append):** Opens a file for appending. It creates a new file if one doesn't exist, but if it does, it adds new content to the end of the file without overwriting.

**'x' (Create):** Creates a new file. If the file already exists, the operation fails with a FileExistsError.

**'t' (Text):** Opens in text mode. This is the default.

**'b' (Binary):** Opens in binary mode, for working with non-text files like images or executables.

You can combine modes, for example, 'rb' for reading a binary file.

### 2. Reading and Writing Files ✍️
Once a file is open, you can perform I/O operations.

**Writing to a File**
You use the .write() method to write data to an open file. It's important to note that you must provide the data as a string.

```Python

# Open the file in write mode
file = open("notes.txt", "w")

# Write a single line
file.write("Hello, this is my first line.\n")

# Write another line
file.write("This is the second line.")

# Don't forget to close the file!
file.close()
```
**Reading from a File**
There are a few common ways to read data from a file:

**.read():** Reads the entire contents of the file as a single string.

**.readline():** Reads a single line from the file.

**.readlines():** Reads all lines from the file and returns them as a list of strings.

Here’s an example using these methods:

```Python

# Open the file in read mode
file = open("notes.txt", "r")

# Read the entire content
content = file.read()
print(content)

# To read line by line, you can use a for loop (most common method)
# This is a very efficient way to handle large files.
for line in file:
    print(line)

# Don't forget to close the file!
file.close()
```
### 3. Closing a File 🔒
After you're done with a file, it's crucial to close it using the .close() method. This frees up system resources and ensures that all changes are saved. Failing to close a file can lead to data loss or other issues.

***Best Practice: The with Statement** ✅
A more reliable and recommended way to handle files is using the with statement. This approach automatically handles closing the file, even if errors occur. It's cleaner and safer than manually calling .close().

```Python

# Writing to a file using 'with'
with open("shopping_list.txt", "w") as f:
    f.write("Milk\n")
    f.write("Bread\n")
    f.write("Eggs\n")

# Reading from the same file using 'with'
with open("shopping_list.txt", "r") as f:
    for item in f:
        print(item.strip()) # .strip() removes leading/trailing whitespace, including the newline character

```

## 📘 6.Comparison Operators in Python
Comparison operators are used to compare two values and evaluate them to a Boolean result: True or False. These operators are fundamental for making decisions in code, such as in conditional statements and loops.

**Basic Comparison Operators**
Operator	Name	Example	Result
==	Equal	5 == 5	True
!=	Not equal	5 != 3	True
>	Greater than	5 > 3	True
<	Less than	5 < 3	False
>=	Greater than or equal to	5 >= 5	True
<=	Less than or equal to	5 <= 3	False

Export to Sheets
Examples in Code
Here are some practical examples of how comparison operators work.

**Using Integers and Floats**
Comparison operators can be used with both integers and floats.

```Python

x = 10
y = 20
z = 10.0

print(x == y)   # False
print(x < y)    # True
print(x >= z)   # True (10 is equal to 10.0)
print(x != z)   # False (10 is not unequal to 10.0)
```
**Using Strings**
Strings can also be compared. The comparison is based on lexicographical order (alphabetical order).

```Python

str1 = "apple"
str2 = "banana"

print(str1 == str2)    # False
print(str1 < str2)     # True ('a' comes before 'b')
print("cat" > "car")   # True ('t' comes after 'r')
```
Chaining Comparison Operators
You can chain multiple comparison operators together for a more compact expression. This is a unique feature of Python.

```Python

x = 5

print(1 < x < 10)  # True (This is equivalent to: (1 < x) and (x < 10))
print(1 > x < 10)  # False (1 > 5 is False)
```
**Identity vs. Equality**
It is important to distinguish between equality (==) and identity (is).

== checks if the values of two objects are equal.

is checks if two variables refer to the exact same object in memory.

```Python

list1 = [1, 2, 3]
list2 = [1, 2, 3]
list3 = list1

print(list1 == list2)  # True (The values are equal)
print(list1 is list2)  # False (They are separate objects in memory)
print(list1 is list3)  # True (list3 is a reference to the same object as list1)

```



## 📘 7. Conditional Statements
Conditional statements let your program make decisions based on specific conditions. The primary structure uses if, elif (else if), and else.

```Python

if 3 > 2:
    print('Its true')

# The `if-else` structure
hungry = False
if hungry:
    print('Feed me')
else:
    print('I am not hungry')

# The `if-elif-else` structure
loc = 'School'
if loc == 'School':
    print(f'{loc} is mine')
elif loc == 'hostel':
    print('I own the hostel')
else:
    print('nothing')
```

## 📘 8. Loops
Loops are used to repeatedly execute a block of code.

**For Loop**
The for loop iterates over a sequence (like a list, string, or range).

```Python

mylist = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
for num in mylist:
    print(num)

# Using a for loop with a conditional statement
for num in mylist:
    if num % 2 == 0:
        print(f'even num: {num}')
    else:
        print(f'Odd number: {num}')

# Summing items in a list (Note: The user's example had a typo, here's the corrected version)
list_sum = 0
for num in mylist:
    list_sum = list_sum + num
    print(list_sum)

# Iterating through a string
mystring = 'Hello World'
for letter in mystring:
    print('Cool!')

# Iterating through a tuple
tup = (1, 2, 3)
for item in tup:
    print(item)

# Tuple unpacking with a for loop
mylist = [(1, 3), (5, 7), (8, 9)]
print(len(mylist))
for (a, b) in mylist:
    print(a)

# Iterating through a dictionary
dic = {'k1': 2, 'k2': 3, 'k3': 5}
for key, value in dic.items():
    print(key, value)
```
**While Loop**
While loops will continue to execute a block of code while some condition remains true.

Syntax:

```Python

while some_boolean_condition:
    # Do something
# A while loop can also have an optional else block.


while some_boolean_condition:
    # Do something
else:
    # Do something


x = 0
while x < 5:
    print(f'The current value of x is {x}')
    x = x + 1  # or x += 1
else:
    print("X is not less than 5")
```
**Loop Control: break, continue, pass**
**break:** Breaks out of the current closest enclosing loop.
Example: break
```Python

mystr = 'sammy'
for letter in mystr:
    if letter == 'a':
        break
    print(letter)

x = 0
while x < 10:
    if x == 6:
        break
    print(x)
    x += 1
```
**continue:** Goes to the top of the closest enclosing loop.
Example: continue

```Python

mystr = 'sammy'
for letter in mystr:
    if letter == 'a':
        continue
    print(letter)
```
**pass:** Does nothing at all.

Example: pass

```Python

x = [1, 2, 3]
for item in x:
    # with pass we can avoid syntax error for an empty loop
    pass
print('end of my script')
```


## 📘 9. Useful Operators in Python
Python has many built-in functions and operators that simplify common tasks.

**range()**
The range() function generates a sequence of numbers.

```Python

# range(stop) - generates numbers from 0 up to stop-1
for num in range(10):
    print(num)

# range(start, stop)
for num in range(3, 10):
    print(num)

# range(start, stop, step)
for num in range(0, 10, 2):
    print(num)

# Converting range to a list
print(list(range(0, 11, 2)))
```
**enumerate()**
The enumerate() function provides both an index and a value during a loop.

```Python

index_count = 0
for letter in 'abcde':
    print('At index {} the letter is {}'.format(index_count, letter))
    index_count += 1

# The same task using enumerate
word = 'abcde'
for index, letter in enumerate(word):
    print(index)
    print(letter)
    print('\n')
```
**zip()**
The zip() function combines multiple iterables into an iterator of tuples.

```Python

mylist1 = [1, 2, 3]
mylist2 = ['a', 'b', 'c']
for item in zip(mylist1, mylist2):
    print(item)

# If lists are of different lengths, zip stops at the shortest one
mylist1 = [1, 2, 3, 5, 6]
mylist2 = ['a', 'b', 'c']
for item in zip(mylist1, mylist2):
    print(item)
```
**in Operator (Membership)**
The in operator checks if an item is in a sequence.

```Python

print('x' in ['x', 'y', 'z'])
print('a' in 'a word')
print('mykey' in {'mykey': 345})
```
**min() and max()**
These functions find the smallest and largest values in an iterable.

```Python
mylist1 = [1, 2, 3, 5, 6]
print(min(mylist1))
print(max(mylist1))
```
**random Module**
The random module is used for generating random values.

```Python

from random import shuffle, randint

mylist = [1, 2, 3, 4, 5, 6, 7, 8, 9]
shuffle(mylist)
print(mylist) # Prints a shuffled version of the list

my_num = randint(0, 1000)
print(my_num) # Prints a random integer between 0 and 1000
```
**input()**
The input() function prompts the user for a value and returns it as a string.

```Python
result = input('Enter a number here: ')
print(result)
```
## 📘 10. List Comprehensions
List comprehensions are a unique and concise way to quickly create a list in Python. They are often more readable and efficient than a traditional for loop.

```Python

# Traditional approach
mystring = 'Hello'
mylist = []
for letter in mystring:
    mylist.append(letter)

# List comprehension equivalent
mylist = [letter for letter in mystring]

# Creating a list of squares
mylist = [num**2 for num in range(0, 11)]

# Creating a list with a conditional filter
mylist = [num for num in range(0, 11) if num % 2 == 0]

# Converting Celsius to Fahrenheit
celcius = [0, 10, 20, 30]
fahr = [(9/5) * temp + 32 for temp in celcius]
print(fahr)

# Nested List Comprehensions
mylist = [x * y for x in [2, 4, 6] for y in [100, 200, 300]]
```

## 📘 11. Methods and Functions
**def Keyword**
Functions are blocks of reusable code created using the def keyword. Good function names use snake casing (lowercase letters with underscores).

```Python

def name_of_function():
    """
    Docstring: This is where the function's explanation goes.
    """
    print("Hello")
name_of_function()
```
The return keyword is typically used to send a result back from the function, which allows you to assign the output to a variable.

```Python

# Function that just prints
def print_result(a, b):
    print(a + b)
print_result(10, 20) # Output: 30

# Function that returns a value
def return_result(a, b):
    return a + b
result = return_result(10, 20)
print(result) # Output: 30
```
**Functions with Logic**
You can define functions that perform a specific task, like checking if a number is even.

```Python

def even_check(number):
    return number % 2 == 0
print(even_check(20)) # True
```
Returning a Value from a List
Here are two examples of functions that process a list, showing different return behaviors.

Example 1: Check if any number is even

```Python

def even_list(num_list):
    for num in num_list:
        if num % 2 == 0:
            return True # Returns True as soon as an even number is found
    return False # Only returns False if the loop completes without finding an even number
print(even_list([1, 5, 3])) # False
```
Example 2: Return all even numbers in a list

```Python
def even_list(num_list):
    even_numbers = []
    for num in num_list:
        if num % 2 == 0:
            even_numbers.append(num)
    return even_numbers
print(even_list([1, 9, 6, 4])) # [6, 4]
```

## 📘 12. What are *args and **kwargs?

- **args** → lets you pass any number of positional arguments to a function (stored as a tuple).
- **kwargs** → lets you pass any number of keyword arguments to a function (stored as a dictionary).

Think of them as a way to make functions flexible without hardcoding the number of inputs.
- Example: args
```python
def install_packages(*args):
    for pkg in args:
        print(f"Installing {pkg}...")

install_packages("nginx", "docker", "git")
```
- Example: kwargs
```python
def deploy_service(**kwargs):
    for key, value in kwargs.items():
        print(f"{key} => {value}")

deploy_service(app="web", version="1.0.2", replicas=3)
```

- *args and **kwargs together
```python
def configure_server(*args, **kwargs):
    print("Packages to install:", args)
    print("Configuration settings:", kwargs)

configure_server("nginx", "docker", user="admin", port=8080)
```
## 📘 13. Displaying Informtion

```python
a = 100
print(a)
```
### Accepting User Input
- Input function always return a str type we can convert if we want to
```python
result = input("Plese enter a value: ")
print(result)
type(result)
```

### Validating User Input
```python
def user_choice():
    choice = 'Wrong'
    while choice.isdigit() == False:
        choice = input("Please enter a number (0-10): ")
        if choice.isdigit() == False:
            print("Sorry that is not a digit")
    return int(choice)
user_choice()
```
## 📘 14. Modulus and packages
**1. What is a Module in Python?**
- A module is simply a Python file (.py) that contains code (functions, classes, variables) you can reuse.

Think:
- A single .py file = 1 module.

Example — Creating and Importing a Module
📁 server_utils.py
```python
# server_utils.py
def start_server(name):
    print(f"Starting server: {name}")

def stop_server(name):
    print(f"Stopping server: {name}")
```
📁 main.py
```python
# main.py
import server_utils

server_utils.start_server("nginx")
server_utils.stop_server("nginx")
```
Output:
```yaml
Starting server: nginx
Stopping server: nginx
```

**2. What is a Package in Python?**
- A package is a collection of modules inside a folder that contains a special file __init__.py.

Think:
- Folder with multiple .py files (modules) + __init__.py = package.
Example — Creating and Using a Package
📁 Folder Structure:
```markdown
devops_tools/
    __init__.py
    aws_utils.py
    k8s_utils.py
main.py
```

📁 aws_utils.py

```python
def list_s3_buckets():
    print("Listing S3 buckets...")

def create_s3_bucket(name):
    print(f"Creating S3 bucket: {name}")
```
📁 k8s_utils.py
```python
def list_pods():
    print("Listing Kubernetes pods...")

def deploy_pod(name):
    print(f"Deploying pod: {name}")
```

📁 main.py
```python
from devops_tools import aws_utils, k8s_utils

aws_utils.list_s3_buckets()
k8s_utils.list_pods()
```
Output:

```nginx
Listing S3 buckets...
Listing Kubernetes pods...
```
**3. Key Differences**
| **Module**                             | **Package**                                             |
| -------------------------------------- | ------------------------------------------------------- |
| Single `.py` file                      | Folder with multiple `.py` files                        |
| Used for smaller, related functions    | Used for organizing large projects                      |
| Imported directly (`import file_name`) | Imported via folder path (`from package import module`) |

**4. Why DevOps Engineers Care**
- Modules → Organize automation scripts for different tasks (EC2, S3, CI/CD).
- Packages → Create reusable DevOps toolkits for teams.
- Keeps large automation projects structured.

**5. Real DevOps Example**
You might have:
```markdown
infra_automation/
    __init__.py
    aws.py
    docker.py
    kubernetes.py
```
- ```aws.py``` → Functions for EC2, S3
- ```docker.py``` → Functions for building & pushing images
- ```kubernetes.py``` → Functions for deployments & services
You import only what you need:
```python
from infra_automation import aws, docker
aws.create_ec2_instance("t2.micro")
docker.build_image("myapp")
```

## 📘 15. __name__ == "__main__"

**1. What does __name__ do?**
- Every Python file has a built-in variable called __name__.
- If you run a Python file directly, Python sets __name__ = "__main__".
- If you import that file into another script, __name__ will be set to the module’s name (filename without .py).

**2. Why use if __name__ == "__main__":?**
This lets you write code that:
- Runs only when the file is executed directly.
- Does not run when the file is imported as a module.

**3. Example — Without __main__**
📁 server.py
```python
print("Starting server...")

def start():
    print("Server is running.")
```
📁 main.py
```python
import server
print("Doing other tasks...")
```
Output:
```nginx
Starting server...       # <-- Runs even though we just imported
Doing other tasks...
```
❌ This is bad because importing the file starts the server automatically.

**4. Example — With if __name__ == "__main__":**
📁 server.py
```python
def start():
    print("Server is running.")

if __name__ == "__main__":
    print("Starting server...")
    start()
```
📁 main.py
```python
import server
print("Doing other tasks...")
```
***Case 1 — Run python server.py***
```nginx
Starting server...
Server is running.
```
***Case 2 — Run python main.py***
```nginx
Doing other tasks...
```
✅ Now the server doesn’t auto-run when imported.

**5. Why DevOps Engineers Use This**
- To make scripts that can be imported as a library or run as a standalone tool.
- Example: You might have aws_utils.py with functions, but only run them if the file is executed directly.
```python
# aws_utils.py
import boto3

def list_buckets():
    s3 = boto3.client('s3')
    for bucket in s3.list_buckets()['Buckets']:
        print(bucket['Name'])

if __name__ == "__main__":
    print("Listing buckets from CLI...")
    list_buckets()
```
You can run:
```bash
python aws_utils.py
```

- Lists buckets immediately.
- Or import it into another script:

```python
from aws_utils import list_buckets
list_buckets()
```
- No unwanted extra output.

## 📘 16. Errors and exception handling
- Errors are bound to happen in code.
- We can use error handling to attempt to plan for possible errors
- We can use error handling to let the script continue with other code, even if there is an error
- We use three keywords for this:
-- try: This is the block of code to be attempted (May lead to an error)
-- except: Block of code will execute in case there is an error in try block
-- finally: A final block of code to be executed, regardless of an error

```python
def add():
    try:
        n1 = int(input("Please enter a number: "))
        n2 = int(input("Please enter a number: "))
        result = n1 + n2
    except ValueError:
        print("Invalid input! Please enter numeric values.")
    else:
        print("Addition went well")
        print("Result:", result)

add()
```
```python
try:
    f = open ('testfile','r')
    f.write("Write a test file")
except TypeError:
    print("There was a type error!")
except OSError:
    print('Hey you have an OS Error')
finally:
    print("I always run")
```
**Unit Testing**
- pylint: This is a library that looks at your code and reports back possible issues
- unittest: This built-in library will allow to test your own programs and check you are getting desired outputs.
```python
# Install pylint
pip install pylint
```
✅ Lambda Expressions
- Lambda is a way to write small anonymous functions in Python.
```python
# Traditional function
def square(num):
    return num ** 2

# Equivalent lambda expression
square = lambda num: num ** 2
```
✅ map(function, iterable)
- The map() function applies a given function to each item in an iterable (like a list) and returns a map object (which is an iterator).

Example with a named function:
```python
def square(num):
    return num ** 2

my_nums = [1, 2, 3, 4, 5]
for item in map(square, my_nums):
    print(item)
```
✅ filter(function, iterable)
- The filter() function filters items out of an iterable if they don't meet a condition defined by the function.
```python
my_nums = [1, 2, 3, 4, 5, 6]

# Filter even numbers using lambda
evens = list(filter(lambda num: num % 2 == 0, my_nums))
print(evens)  # Output: [2, 4, 6]
```
✅ Nested Statements and Scope (LEGB Rule)
- Python uses the LEGB rule to resolve variable names.
  -- L: Local — Names assigned within a function (including lambda)
  -- E: Enclosing — Names in the local scope of any enclosing functions
  -- G: Global — Names assigned at the top-level of a module or declared global
  -- B: Built-in — Names preassigned in Python (e.g., len, sum, etc.)
```python
x = "global"

def outer():
    x = "enclosing"
    
    def inner():
        x = "local"
        print(x)  # local

    inner()
    print(x)  # enclosing

outer()
print(x)  # global
```
- If a variable is not found in Local, Python checks Enclosing, then Global, then Built-in.

## 📘 17. Decprators and Generators

✅ Decorators
🔍 What does "decorate" mean?
- In Python, decorating a function means adding extra functionality to it without modifying the original function's code.
- A decorator is just a function that takes another function as input and returns a new function with added behavior.

✨ Example Breakdown
```python
def new_decorator(original_func):
    def wrap_func():
        print('Some extra code, before the original function')
        original_func()
        print('Some extra code, after the original function')
    return wrap_func
```
- original_func: The function to be decorated.
- wrap_func: The new function that wraps original_func with extra functionality.

✅ Using the decorator manually
```python
def func_needs_decorator():
    print("I want to be decorated!")

decorated_func = new_decorator(func_needs_decorator)
decorated_func()
```

✅ Using the @ decorator syntax (shortcut)
```python
@new_decorator
def func_needs_decorator():
    print("I want to be decorated!")

func_needs_decorator()
```
This is equivalent to:

```python
func_needs_decorator = new_decorator(func_needs_decorator)
```
✅ Generators
- Generators allow you to generate values lazily (one at a time, on demand), using the yield keyword instead of return.

🔁 Normal function vs Generator
### Regular function
```python
def create_cubes(n):
    result = []
    for x in range(n):
        result.append(x**3)
    return result
```

Returns the full list at once (memory-intensive for large n).
```python
# Generator version
def create_cubes(n):
    for x in range(n):
        yield x**3
```

Yields one value at a time, saving memory.
```python
print(list(create_cubes(10)))  # [0, 1, 8, 27, 64, ...]
```
📈 Fibonacci Generator Example
```python
def gen_fibon(n):
    a = 1
    b = 1
    for _ in range(n):
        yield a
        a, b = b, a + b

for num in gen_fibon(10):
    print(num)
```

Prints first 10 Fibonacci numbers.

✅ iter() Function
- iter() turns an iterable (like a list, tuple, string) into an iterator (which you can use with next()).

Example:
```python
my_list = [1, 2, 3]
my_iter = iter(my_list)
print(next(my_iter))  # 1
print(next(my_iter))  # 2
print(next(my_iter))  # 3
# print(next(my_iter))  # Raises StopIteration
```
✅ Summary Table

| Feature       | Key Concept                               | Keyword Used |
| ------------- | ----------------------------------------- | ------------ |
| **Decorator** | Add functionality to existing functions   | `@decorator` |
| **Generator** | Produce items lazily (one at a time)      | `yield`      |
| **Iterator**  | Step through iterable data using `next()` | `iter()`     |

## 📘 18. Python Web Scraping 
- requests to fetch webpage content
- BeautifulSoup to parse and extract HTML data
```bash
pip install requests beautifulsoup4 lxml
```
```python
import requests
result = requests.get("https://www.google.com")
print(result.text)  # Raw HTML from the page
```

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(result.text, "lxml")
print(soup.select('title')[0].getText())  # Extract the page title
```

### Element Selectors:
```python
soup.select('div'): All <div> tags
soup.select('#id'): Elements with a specific id
soup.select('.class'): Elements with a specific class
soup.select('div span'): Any <span> inside a <div>
soup.select('div > span'): Direct children only
```

🔹 Example: Scraping a simple page title
```python
# 1. Import the required libraries
import requests                      # To make HTTP requests
from bs4 import BeautifulSoup        # To parse and extract content from HTML

# 2. Define the URL you want to scrape
url = "https://www.example.com"

# 3. Make a GET request to fetch the HTML content of the page
response = requests.get(url)        # Sends an HTTP GET request
print(response.status_code)         # Check if request was successful (200 = OK)

# 4. Parse the HTML using BeautifulSoup
soup = BeautifulSoup(response.text, "lxml")  # Parse the HTML with lxml parser

# 5. Extract the <title> tag text
title_tag = soup.select_one("title")  # Selects the first <title> element
print("Page Title:", title_tag.getText())    # Extracts and prints the text inside <title>
```

🔐 Passing Data (Form Inputs) and Credentials
- You might want to log in, search, or submit a form. You can do this by sending data with a POST request.
  
🔸 Example: Submitting form data with POST
  - Let’s say you’re submitting a login form.
```python
import requests

# URL to send the form POST request to (e.g., login form action URL)
login_url = "https://example.com/login"

# The form data (key names must match the form's input names)
payload = {
    'username': 'your_username',
    'password': 'your_password'
}

# Create a session object to persist cookies
session = requests.Session()

# Send a POST request with the form data
response = session.post(login_url, data=payload)

# Print response status to check if login was successful
print("Login response:", response.status_code)
```
🔒 If login is successful, the session object will hold the cookies. You can now scrape protected pages:
```python
protected_url = "https://example.com/dashboard"
dashboard = session.get(protected_url)

print("Dashboard status:", dashboard.status_code)
print(dashboard.text[:500])  # Print the first 500 characters of the page
```
🔍 CSS Selectors for Data Extraction
```python
soup.select('div')         # All <div> tags
soup.select('#main')       # Element with id="main"
soup.select('.container')  # Elements with class="container"
soup.select('div > p')     # <p> directly inside <div>
soup.select('div span')    # Any <span> inside a <div>
```
Use .getText() or .attrs to get data:
```python
soup.select_one('a').getText()         # Text inside the first <a>
soup.select_one('a')['href']           # href attribute of first <a>
```

🛡️ Tips to Avoid Blocking

- Always use custom headers (User-Agent)
- Add delays between requests (time.sleep())
- Rotate proxies/IP addresses for heavy scraping
- Respect robots.txt and terms of service

📁 Optional: Export to File
```python
with open("output.html", "w", encoding="utf-8") as f:
    f.write(response.text)
```
✅ Summary
| Task             | Tool                    | Description                    |
| ---------------- | ----------------------- | ------------------------------ |
| Fetch page       | `requests.get()`        | Gets raw HTML                  |
| Parse HTML       | `BeautifulSoup`         | Navigates HTML like a DOM      |
| Extract data     | `select()`, `getText()` | Grab tags, attributes          |
| Submit data      | `requests.post()`       | Send login/data                |
| Maintain session | `requests.Session()`    | Store cookies between requests |

## 📘 19. Working with Images

📦 1. Installing Pillow
  - To work with images, first install Pillow (a modern fork of PIL):
    ```python
    pip install pillow
    ```
    📘 Official Docs: pillow.readthedocs.io
    
🖼️ 2. Opening and Inspecting an Image
 ```python
    from PIL import Image  # Import Image class from Pillow
    
    # Open the image file
    mac = Image.open('example.jpg')  # Replace with your image path
    
    # Check the type
    print(type(mac))  # <class 'PIL.JpegImagePlugin.JpegImageFile'>
    
    # Show the image in default viewer
    mac.show()
    
    # Get basic properties
    print(mac.size)                # Output: (width, height)
    print(mac.filename)            # 'example.jpg'
    print(mac.format_description)  # e.g., 'JPEG (ISO 10918)'
 ```   
✂️ 3. Cropping an Image
 
 ```python
    # Define the crop box as a tuple (left, top, right, bottom)
    # (X, Y, W, H) → (left, upper, right, lower)
    cropped = mac.crop((0, 0, 100, 100))  # Crop top-left 100x100 px
    cropped.show()
 ```   
 💡 Note: The crop box is not (X, Y, Width, Height), but rather a box defined by top-left and bottom-right coordinates.

📌 4. Pasting One Image into Another
```python
# Crop a portion from the image
piece = mac.crop((0, 0, 100, 100))

# Paste that piece into the original at position (0, 0)
mac.paste(im=piece, box=(0, 0))  # Overwrites the top-left of the image
mac.show()
```
🔄 5. Rotating and Resizing
```python
# Rotate image by 90 degrees
rotated = mac.rotate(90)  # Counter-clockwise
rotated.show()

# Resize image to 300x500 pixels
resized = mac.resize((300, 500))
resized.show()
```
🎨 6. Color and Transparency (RGBA)
- RGBA stands for Red, Green, Blue, Alpha
- Alpha controls transparency (0 is fully transparent, 255 is fully opaque)

🔴 Adding Transparency to a Red Image
```python
# Create a red RGBA image (100x100 px)
red = Image.new("RGBA", (100, 100), color=(255, 0, 0, 255))  # Opaque red

# Adjust transparency
red.putalpha(128)  # Set alpha to 50% transparent
red.show()
```
🔵 Pasting Red onto Blue with Mask
```python
# Create a blue background
blue = Image.new("RGBA", (300, 300), color=(0, 0, 255, 255))

# Paste red onto blue using red as a mask (uses alpha channel)
blue.paste(im=red, box=(0, 0), mask=red)
blue.show()
```

💾 7. Saving Images
```python
# Save the modified image
blue.save("C:/Users/Downloads/modified.png")  # Use full path or relative
```
📌 Make sure the directory exists, or Python will throw a FileNotFoundError.

🧠 Summary Table:

| Task              | Function Used                    |
| ----------------- | -------------------------------- |
| Open Image        | `Image.open()`                   |
| Show Image        | `.show()`                        |
| Get Size          | `.size`                          |
| Crop Image        | `.crop((x1,y1,x2,y2))`           |
| Paste Image       | `.paste(im, box)`                |
| Rotate Image      | `.rotate(degrees)`               |
| Resize Image      | `.resize((w,h))`                 |
| Create RGBA Image | `Image.new("RGBA", size, color)` |
| Add Alpha         | `.putalpha(value)`               |
| Save Image        | `.save("path")`                  |

## 📘 20. Working with Emails

📧 Part 1: Sending Emails with Python
- Python uses the smtplib (Simple Mail Transfer Protocol library) to send emails.

🔐 Pre-requisites:
- A Gmail (or other) email account
- “Less secure app access” or App Password enabled if using Gmail with 2FA

✅ Step-by-step code and explanation:
```python
import smtplib  # Module to send emails using SMTP (Simple Mail Transfer Protocol)
import getpass  # Module to securely input passwords without showing them

# 1. Connect to Gmail's SMTP server at port 587
smtp_object = smtplib.SMTP('smtp.gmail.com', 587)  # Use port 587 for TLS encryption

# 2. Send the 'EHLO' (Extended Hello) to initiate the connection
smtp_object.ehlo()

# 3. Start TLS encryption
smtp_object.starttls()

# 4. Securely ask for login credentials
email = getpass.getpass("Enter your email: ")
password = getpass.getpass("Enter your password or app password: ")

# 5. Log in using your credentials
smtp_object.login(email, password)

# 6. Set sender and receiver email
from_address = email
to_address = input("Enter recipient's email: ")

# 7. Get email subject and body
subject = input("Enter the subject line: ")
message = input("Enter the body of the email: ")

# 8. Format message correctly
msg = "Subject: " + subject + "\n" + message  # Separate subject and body with a newline

# 9. Send the email
smtp_object.sendmail(from_address, to_address, msg)

# 10. End the SMTP session
smtp_object.quit()
```
💡 Notes:
- ehlo() tells the server we want to communicate using extended SMTP
- starttls() upgrades the connection to secure TLS
- Use App Passwords instead of your Gmail password if 2FA is on

📨 Part 2: Receiving Emails with Python

- To receive and read emails, use:
  -- imaplib → Connect to mail server
  -- email module → Parse message content

✅ Step-by-step code and explanation:
```python
import imaplib       # For connecting to IMAP mail servers
import getpass       # For secure password input
import email         # To parse email content

# 1. Connect to Gmail's IMAP server
mail = imaplib.IMAP4_SSL('imap.gmail.com')  # SSL connection for secure access

# 2. Login to the email account
email_user = getpass.getpass("Email: ")
email_pass = getpass.getpass("Password or App Password: ")
mail.login(email_user, email_pass)

# 3. List mailboxes
mail.list()  # Returns all folders like INBOX, SENT, etc.

# 4. Select the mailbox to use (INBOX in this case)
mail.select('inbox')  # You can select other folders too

# 5. Search for specific emails (example: all before Nov 2000 with specific subject)
typ, data = mail.search(None, 'BEFORE 01-NOV-2000', 'SUBJECT "NEW 8TEST PYTHON"')

# 6. Get email IDs
email_ids = data[0].split()  # Splits all matching email IDs into a list

# 7. Fetch the latest matching email (optional: you can loop over all)
latest_email_id = email_ids[-1]  # Get the last (most recent) email ID from search result
result, email_data = mail.fetch(latest_email_id, '(RFC822)')  # Fetch full email content

# 8. Parse the raw email content
raw_email = email_data[0][1]  # Get raw bytes of the email
raw_email_string = raw_email.decode('utf-8')  # Decode to readable string

# 9. Use email module to create a structured message object
email_message = email.message_from_string(raw_email_string)

# 10. Walk through each part (text, HTML, attachments, etc.)
for part in email_message.walk():
    if part.get_content_type() == 'text/plain':  # Only extract plain text part
        body = part.get_payload(decode=True)  # Decode content
        print("Email Body:\n", body.decode())  # Decode bytes to string and print
```
💡 Notes:

| Step                          | Function                                     | Purpose |
| ----------------------------- | -------------------------------------------- | ------- |
| `imaplib.IMAP4_SSL()`         | Connect securely to IMAP server              |         |
| `login()`                     | Log in to email account                      |         |
| `list()`                      | List mail folders                            |         |
| `select('inbox')`             | Select inbox                                 |         |
| `search()`                    | Filter emails by date, sender, subject, etc. |         |
| `fetch()`                     | Retrieve specific email by ID                |         |
| `email.message_from_string()` | Converts raw email to a Python object        |         |
| `get_payload()`               | Extracts the body/content of the message     |         |

🔐 Security Tips

- Use app passwords, not your actual Gmail password.
- Never hardcode credentials in your script.
- Use getpass to hide passwords in CLI.
- If automating, consider using a secure secret store (like AWS Secrets Manager or .env files + python-dotenv).

🧠 Summary
| Task             | Library            | Description                                    |
| ---------------- | ------------------ | ---------------------------------------------- |
| Sending Emails   | `smtplib`          | Login, connect to SMTP server, send            |
| Receiving Emails | `imaplib`, `email` | Connect to mailbox, search and read            |
| Secure Inputs    | `getpass`          | Hides password in terminal                     |
| Message Parsing  | `email.message`    | Helps extract content like subject, body, etc. |

## 📘 21. PDFs and Emails

🔹 Working with CSV files
- CSV = Comma Separated Values → A plain text file where each row is a record, and values are separated by , or another delimiter (like ; or |).

### 1. Using Python’s csv module
- The csv module is built into Python.
```python
import csv

# Open CSV file
data = open('example.csv', encoding='utf-8')

# Create a CSV reader object
csv_data = csv.reader(data)

# Convert to list of lists
data_lines = list(csv_data)

# Print the first row (header)
print(data_lines[0])

# Print first 5 rows
for line in data_lines[:5]:
    print(line)

# Writing to a new CSV file
file_to_output = open('to_save_file.csv', mode='w', newline='')
csv_writer = csv.writer(file_to_output, delimiter=',')

# Writing multiple rows
csv_writer.writerows([
    ['1', '2', '3'],
    ['4', '5', '6']
])

file_to_output.close()
```
✅ Explanation:
- csv.reader → Reads file row by row, splitting values by delimiter.
- list(csv_data) → Converts reader into a list of lists.
- csv.writer → Writes rows to new file.
- delimiter=',' → Defines separator (default is ,).

### 2. Using Pandas
- Pandas is a full data analysis library. It is faster and easier for large datasets.
```python
import pandas as pd

# Read CSV file
df = pd.read_csv("example.csv")

# Show first 5 rows
print(df.head())

# Select a column
print(df['Name'])

# Save DataFrame back to CSV
df.to_csv("output.csv", index=False)
```

✅ Explanation:
- pd.read_csv() → Loads CSV directly into a DataFrame (table-like object).
- df.head() → Shows first 5 rows.
- index=False → Prevents Pandas from writing row numbers.

### 3. Openpyxl (Excel)

For .xlsx (Excel) files.

```python
from openpyxl import Workbook, load_workbook

# Create new Excel file
wb = Workbook()
ws = wb.active
ws['A1'] = "Hello"
ws.append([1, 2, 3])
wb.save("example.xlsx")

# Read Excel file
wb = load_workbook("example.xlsx")
ws = wb.active
print(ws['A1'].value)
```

✅ Explanation:
- Workbook() → Creates a new Excel workbook.
- load_workbook() → Opens an existing Excel file.
- .append() → Adds a row.

### 4. Google Sheets API
- Lets you work with Google Sheets directly from Python.

Steps:
- Enable Google Sheets API in Google Cloud Console.
- Install gspread → pip install gspread oauth2client.
- Authenticate with service account JSON.

Example:
```python
import gspread
from oauth2client.service_account import ServiceAccountCredentials

# Define scope
scope = ["https://spreadsheets.google.com/feeds",
         "https://www.googleapis.com/auth/drive"]

# Authenticate
creds = ServiceAccountCredentials.from_json_keyfile_name("creds.json", scope)
client = gspread.authorize(creds)

# Open sheet
sheet = client.open("MySheet").sheet1  

# Get data
data = sheet.get_all_records()
print(data)

# Insert a row
sheet.insert_row(["Satish", "27", "India"], 2)
```
✅ Explanation:

- creds.json → Service account credentials.
- .sheet1 → Refers to the first sheet.
- .get_all_records() → Reads everything into list of dictionaries.

🔹 Working with PDF files
    - We use PyPDF2.

1. Reading a PDF
```python
import PyPDF2

# Open PDF file in read-binary mode
f = open("mypdf.pdf", "rb")

# Create reader object
pdf_reader = PyPDF2.PdfReader(f)

# Get number of pages
print(len(pdf_reader.pages))

# Extract text from first page
page_one = pdf_reader.pages[0]
print(page_one.extract_text())

f.close()
```
✅ Explanation:

- "rb" → read binary.
- .pages → list of pages.
- .extract_text() → extracts text.

2. Writing/Creating a PDF

```python
import PyPDF2

# Create a new PDF writer
pdf_writer = PyPDF2.PdfWriter()

# Add a blank page (width=200, height=200)
pdf_writer.add_blank_page(width=200, height=200)

# Save it
with open("newfile.pdf", "wb") as f:
    pdf_writer.write(f)
```
✅ Explanation:
- PdfWriter() → Used to create/modify PDFs.
- .add_blank_page() → Adds an empty page.
- .write(f) → Saves PDF.

🔑 Summary
- csv module → Basic reading/writing CSVs.
- Pandas → Powerful tabular data analysis.
- Openpyxl → Excel automation.
- Google Sheets API → Cloud spreadsheet manipulation.
- PyPDF2 → Read/write PDFs.

## 📘 22. Advanced python objects and data structures
🔹 Advanced Numbers in Python
- Numbers can be manipulated with built-in functions.
```python
print(hex(8))       # 0x8
print(bin(78))      # 0b1001110
print(pow(2, 4))    # 16 (same as 2**4)
print(abs(-3))      # 3
print(round(3.9))   # 4
print(round(3.9890, 2))  # 3.99
```
✅ Explanation:
- hex(8) → Converts number into hexadecimal string (0x8).
- bin(78) → Converts number into binary (0b1001110).
- pow(2,4) → Raises number to power → 2^4 = 16.
- abs(-3) → Returns absolute value → distance from zero.
- round(3.9) → Rounds to nearest integer → 4.
- round(3.9890,2) → Rounds to 2 decimals → 3.99.

🔹 Advanced Strings
- Strings have many formatting and query methods.
```python
- s = "Hello"

print(s.center(20, 'z'))  # zzzzzzzHellozzzzzzz
print(s.endswith('o'))    # True
```
✅ Explanation:
- s.center(20, 'z') → Puts string in the middle of width=20, fills empty space with 'z'.
- s.endswith('o') → Checks if string ends with 'o'. Returns True/False.

🔹 Advanced Sets
- Sets are unordered collections with no duplicates.
```python
s = {1, 2, 3}
s.add(4)
print(s)   # {1, 2, 3, 4}

s.clear()
print(s)   # set() → now empty

s = {1, 2, 4}
sc = s.copy()
print(sc)  # {1, 2, 4}

print(s.difference(sc))  # set() → no difference

s.discard(2)
print(s)   # {1, 4}

s1 = {1, 2, 3}
s2 = {2, 3, 4}
print(s1.intersection(s2))       # {2, 3}
print(s1.issubset(s2))           # False
print(s1.symmetric_difference(s2)) # {1, 4}
```
✅ Explanation:

- .add(x) → Adds element.
- .clear() → Empties set.
- .copy() → Creates a copy.
- .difference() → Elements only in first set.
- .discard(x) → Removes element if exists (no error if missing).
- .intersection() → Common elements.
- .issubset() → Checks if one set is fully contained in another.
- .symmetric_difference() → Elements in either set, but not both.

🔹 Advanced Dictionaries
- Dictionaries are key-value pairs. We can build them with comprehensions.
```python
# Squares from 0 to 9
squares = {x: x**2 for x in range(10)}
print(squares)
# {0:0, 1:1, 2:4, 3:9, ..., 9:81}

# Map keys ['a','b'] with values squared
mapping = {k: v**2 for k, v in zip(['a','b'], range(2))}
print(mapping)
# {'a':0, 'b':1}
```
✅ Explanation:
- {x: x**2 for x in range(10)} → Dictionary comprehension.
- zip(['a','b'], range(2)) → pairs ('a',0), ('b',1).

🔹 Advanced Lists
- Lists are ordered and mutable (can be modified).
```python
l = [1, 2, 3, 10]

l.append([4, 5])
print(l)   # [1, 2, 3, 10, [4, 5]]

print(l.index(10))   # 3 → position of element 10

l.insert(2, 'hey')
print(l)   # [1, 2, 'hey', 3, 10, [4,5]]

l.remove('hey')
print(l)   # [1, 2, 3, 10, [4, 5]]

l.reverse()
print(l)   # [[4,5], 10, 3, 2, 1]

popped = l.pop()
print(popped)  # 1
print(l)       # [[4,5], 10, 3, 2]

nums = [3, 1, 4, 2]
nums.sort()
print(nums)    # [1, 2, 3, 4]
```
✅ Explanation:
- .append([4,5]) → Adds list as single element at end.
- .index(x) → Finds position of x.
- .insert(pos, val) → Inserts value at given index.
- .remove(val) → Removes first occurrence of value.
- .reverse() → Reverses list order.
- .pop() → Removes last element and returns it.
- .sort() → Sorts list in ascending order.
📝 Summary Table

| Category | Example                       | Meaning            |
| -------- | ----------------------------- | ------------------ |
| Numbers  | `hex(8)`                      | Convert to hex     |
| Strings  | `"Hi".center(10,'-')`         | Center text        |
| Sets     | `s.intersection(s2)`          | Common elements    |
| Dict     | `{x: x**2 for x in range(5)}` | Dict comprehension |
| Lists    | `l.insert(2,'x')`             | Insert at index    |

## 📘 23. Introduction to GUIs in Python

✅ 1. Installation
- If widgets don’t load:
```python
pip install -U ipywidgets
jupyter nbextension enable --py widgetsnbextension
```
✅ 2. Basic Example with interact
```python
from ipywidgets import interact

def func(x):
    return x

interact(func, x=10)
```
🔎 Explanation:

- interact(func, x=10) → Creates a widget automatically:
- Since x=10 is an int, it creates a slider.
- When you move the slider, it calls func(x) with the new value.

✅ 3. Interactive Function with Multiple Inputs
```python
from ipywidgets import interact

def f(a, b):
    return a + b

interact(f, a=5, b=10)
```
🔎 Explanation:
- Both a and b get sliders.
- Adjusting them shows the sum live.

✅ 4. Using fixed to Lock a Value
```python
from ipywidgets import interact, fixed

def f(x, y):
    return x * y

interact(f, x=10, y=fixed(5))
```
🔎 Explanation:
- fixed(5) → Locks y=5.
- Only x has a slider.

✅ 5. Dropdown, Checkbox, Text Widgets
```python
import ipywidgets as widgets
from IPython.display import display

dropdown = widgets.Dropdown(
    options=['DevOps', 'Cloud', 'Python'],
    value='Python',
    description='Course:',
)

checkbox = widgets.Checkbox(
    value=True,
    description='Subscribe'
)

text = widgets.Text(
    value='Satish',
    description='Name:',
)

# Show them
display(dropdown, checkbox, text)
```
🔎 Explanation:

- Dropdown → Creates a dropdown list.
- Checkbox → Creates a checkbox.
- Text → Input box for text.

🔑 Summary
- interact(func, x=10) → Auto widget based on type.
- fixed() → Keep a parameter constant.
- widgets.Dropdown / Checkbox / Text → Manual widgets.
- display() → Shows widget/output inline in Jupyter.

## 📘 24. Modules

🔹 1. Collections Module
- The collections module provides specialized container datatypes beyond built-in lists, sets, dicts.

✅ Counter
- Counts occurrences of elements.
```python
from collections import Counter

mylist = [1,1,1,2,2,3,4,5,5,5,6,6,6,6]
count = Counter(mylist)
print(count)
# Output: Counter({6: 4, 1: 3, 5: 3, 2: 2, 3: 1, 4: 1})

print(count.most_common(2)) # Top 2 frequent
# [(6,4), (1,3)]
```
✅ defaultdict
- Like a normal dict, but provides a default value if the key doesn’t exist.
```python
from collections import defaultdict

d = defaultdict(lambda: 0)
d['a'] = 10
print(d['a']) # 10
print(d['b']) # 0 (default instead of KeyError)
```
✅ namedtuple
- A lightweight alternative to classes. Named indexes for tuples.
```python
from collections import namedtuple

Point = namedtuple('Point', 'x y')
pt = Point(10, 20)
print(pt.x, pt.y)  # 10 20
```

🔹 2. OS & Shutil Modules
- Used to interact with files, directories, and the operating system.
✅ os Module
```python
import os

print(os.getcwd())    # Current working directory
print(os.listdir())   # List of files in current dir

# Walk through directories
for folder, subfolders, files in os.walk(os.getcwd()):
    print("Looking at folder:", folder)
    print("Subfolders:", subfolders)
    print("Files:", files)
```
✅ File/Folder Operations
```python
os.unlink("file.txt")   # Delete file
os.rmdir("folder")      # Remove empty folder
```
✅ shutil Module
```python
import shutil

shutil.move("source.txt", "destination_folder/")
```
✅ send2trash
- Safer delete (moves file to Recycle Bin instead of permanent delete).
```python
import send2trash
send2trash.send2trash("file.txt")
```
🔹 3. Datetime Module
- Used for working with dates & times.
```python
from datetime import date, datetime

# Current date
today = date.today()
print(today, today.day, today.month, today.year)

# Current time
t = datetime.time(datetime.now())
print(t)  # e.g. 14:32:10

# Date difference
d1 = date(2021,11,3)
d2 = date(2020,11,3)
print((d1 - d2).days)  # 365
```
🔹 4. Math Module
- Mathematical functions/constants.
```python
import math
print(math.floor(4.35))  # 4
print(math.ceil(4.35))   # 5
print(math.pi)           # 3.14159...
print(math.e)            # 2.718...
print(math.log(math.e))  # 1.0
print(math.sin(10))      # Sine of 10 radians
print(math.degrees(math.pi/2)) # 90.0
```
🔹 5. Random Module
- Generates random numbers.
```python
import random

print(random.randint(0,100))  # Random int 0-100
random.seed(42)               # Reproducible results
mylist = [1,2,3,4,5]
random.shuffle(mylist)
print(mylist)
```
🔹 6. Python Debugger (pdb)
- Used to debug code line by line.
```python
import pdb

x = [1,2,3]
y = 2
z = 3
result = y+z
pdb.set_trace()   # Pause execution here
result1 = x+z     # Will cause error, good for debugging
```
✅ You can type commands like n (next), c (continue), p var (print variable).

🔹 7. Regular Expressions (re)
- For pattern matching in strings.

✅ Basic Search
```python
import re

text = "the agent phone number is 408-555-1234"

match = re.search(r'\d{3}-\d{3}-\d{4}', text)
print(match.group())  # 408-555-1234
```
✅ Regex Metacharacters
- \d → digit
- \w → word/letter/digit
- \s → whitespace
- \D → non-digit
- \W → non-alphanumeric
- \S → non-whitespace

✅ Quantifiers
- + → 1 or more
- * → 0 or more
- ? → 0 or 1
- {n} → exactly n times
- {n,m} → between n and m

✅ Examples
```python
re.findall(r'cat|dog', 'The dog is here')   # ['dog']
re.findall(r'at', 'the cat sat there')      # ['at','at']
re.findall(r'^\d', '1 is a number')         # ['1']
re.findall(r'[^\d]', 'abc123')              # ['a','b','c']
```
🔹 8. Time Module
- Used for performance measurement and delays.
```python
import time

start = time.time()
time.sleep(2)   # pause for 2 seconds
end = time.time()

print("Elapsed:", end-start, "seconds")
```
🔹 9. Zipping & Unzipping Files
✅ zipfile
```python
import zipfile

comp_file = zipfile.ZipFile('comp_file.zip','w')
comp_file.write('fileone.txt', compress_type=zipfile.ZIP_DEFLATED)
comp_file.close()
```
✅ shutil Archive Helpers
```python
import shutil

# Make zip archive
shutil.make_archive('example', 'zip', 'folder_to_compress')

# Extract archive
shutil.unpack_archive('example.zip', 'unzip_folder', 'zip')
```
🎯 Final Summary
| Module                       | Usage                                                           |
| ---------------------------- | --------------------------------------------------------------- |
| **collections**              | Specialized containers (`Counter`, `defaultdict`, `namedtuple`) |
| **os / shutil / send2trash** | File & directory operations, safe delete                        |
| **datetime**                 | Date, time, differences                                         |
| **math**                     | Math functions & constants                                      |
| **random**                   | Random number generation                                        |
| **pdb**                      | Debugging                                                       |
| **re**                       | Regular expressions for pattern matching                        |
| **time**                     | Measure execution time, delays                                  |
| **zipfile / shutil**         | Compressing & extracting archives                               |
