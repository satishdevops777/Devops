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
