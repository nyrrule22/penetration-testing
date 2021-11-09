# Python

## Basics

### Strings

```python
# Print String (Can use double or single quotes)
print("Hello, world!")
print('Hello, world!')
print("""This string runs
    multiple lines!""")
print("This string is " + "awesome!")  # Concatenation
# Add a new line
print('\n')
```

### Math

```python
print(50 + 50)  # Add
print(50 - 50)  # Subtract
print(50 * 50)  # Multiply
print(50 / 50)  # Divide
print(50 // 6)  # Divide, with no remainder
print(50 + 50 - 50 * 50 / 50)  # PEMDAS
print(50 ** 2)  # Exponents
print(50 % 6)  # Modulus
```

### Variables and Methods

```python
# Creating a variable called quote
quote = "All is fair in love and war."
# Print out the value of the quite variable
print(quote)
print(quote.upper())  # Print value as uppercase using upper() method
print(quote.lower())  # Print value as uppercase using lower() method
print(quote.title())  # Print value as uppercase using title() method
print(len(quote))  # Print the length of the value of quote
# Building upon this
name = "Erich"  # String
age = 33  # Int
gpa = 4.0  # Float
# Use str() method to conver Int to a String
print("My name is " + name + " and I am " + str(age) + " years old.")
```

### Functions

```python
# Example 1
def who_am_i():
    name = "Erich"
    age = 33
    print("My name is " + name + " and I am " + str(age) + " years old.")
who_am_i()
# Example 2 - Using a parameter
def add_one_hundred(num):
    print(num + 100)
add_one_hundred(22)
# Example 3 - Multiple parameters
def add(x, y):
    print(x + y)
add(7,7)
# Example 4 - Using return
def multiply(x, y):
    return x * y
print(multiple(7,7))
# Example 5
def square_root(x):
    print(x ** .5)
square_root(64)
# Example 6
def new_line():
    print('\n')
new_line()
```

### Boolean Expressions

```python
bool1 = True  # Will equal to True
bool2 = 3*3 == 9  # Will equal to True
bool3 = False  # Will equal to False
bool4 = 3*3 != 9  # Will equal to False
print(bool1,bool2,bool3,bool4)  # True True False False
print(type(bool1))  # <class 'bool'>
```

### Relational and Boolean Operators

```python
greater_than = 7 > 6  # Will be True
less_than 5 < 7  # Will be True
greater_than_equal_to = 7>= 7  # Will be True
less_than_equal_to = 7 <= 7  # Will be True
test_and = (7 > 5) and (5 < 7)  # Will be True
test_and2 = (7 > 5) and (5 > 7)  # Will be False
test_or = (7 > 5) or (5 > 7)  # Will be True
test_not = not True  # Will be True
```

### Conditional Statements

```python
# Example 1
def drink(money):
    if money >= 2:
        return "You can purchase the drink"
    else:
        return "No drink for you!"
print(drink(3))  # Will return first condition
print(drink(1))  # Will return second condition
# Example 2
def alcohol(age, money):
    if (age >= 21 and (money >= 5):
        return "We are getting a drink!"
    elif (age >=21) and (money < 5):
        return "Come back with more money."
    elif (age < 21) and (oney >= 5):
        return "Nice try, kid."
    else:
        return "You are too poor and too young."
print(alcohol(21,5))  # Will return first condition
print(alcohol(21,4))  # Will return second condition
print(alcohol(19,5))  # Will return third condition
print(alcohol(19,4))  # Will return fourth condition
```

### Lists

```python
# Mutible and uses square brackets
movies = ["When Harry Met Sally", "Than Hangover", "The Perks of Being a Wallflower", "The Exorcist"]
print(movies[1])  # Will return "The Hangover"
print(movies[1])  # will return "When Harry Met Sally"
print(movies[1:3])  # Will return "Than Hangover" and "The Perks of Being a Wallflower"
print(movies[1:])  # Get the item in index 1 and the rest of the list
print(movies[:1])  # Get the items before the item in index 1
print(movies[-1])  # Get the last item in the list
print(len(movies))  # Print the length (item count) of the list
print(movies.append("Jaws"))  # Append and item to the list
print(movies.pop())  # Removes the last item of the list
print(movies.pop[0])  # Remove the first (a specified ) item from the list
```

### Tuples

```python
# Immutable and uses paretheses
grades = ("a", "b", "c", "d", "f")
print(grades[1])  # Will print out "b"
```

### Looping

```python
# For Loops
veggies = ["cucumber", "spinach", "cabbage"]
for v in veggies:
   print(v)
# While Loops
i = 1
while i < 10:
    print(i)
    i += 1
```

### Importing Modules

```python
import sys  # Import module
from datetime import datetime  # Import part of a module
import datetime as dt  # Import module and giving it an alias
print(sys.version)
print(datetime.now())
print(dt.now())
```

### Advanced Strings

```python
my_name = "Erich"
print(my_name[0])  # Print first letter of my_name
sentence = "This is a sentence."
print(sentence[:4])  # Print first 4 letters
print(sentence.split())  # Print out each word/index based on delimter (space)
print(sentence_split = sentence.split()
print(sentence_join = ' '.join(sentence_split))
quote = "He said, 'give me all your money'"  # Using quotes in a string
quote2 = "He said, \"give me all your money\""  # Escaping same quotes
too_much_space = "        hello        "
print(too_much_space.strip())  # Print string removing extra space
print("A" in "Apple")  # Will return True
print("A" in "apple")  # Will return False
letter = "A"
word = "Apple"
print(letter.lower() in word.lower())
movie = "The Hangover"
print(f"My favorite movie is {movie}.")
```

### Dictionaries

```python
# Key/Value pairs and uses curly braces
drinks = {"White Russian": 7, "Old Fashioned": 10, "Lemon Drop": 8}
drinks["White Russian"] = 8  # Update existing key value in drinks
drinks.get("White Russian")  # Get the value of a specified key
employees = {"Finance": ["bob", "Linda", "Tina"], "IT": ["Gene", "Louise", "Teddy"], "HR": ["Jimmy Jr.", "Mort"]}
employees["Legal"] = ["Mr. Frond"]  # Add new key/value pair to employees
employees.update({"Sales": ["Andie", "Ollie"]})  # Add new key/value pairs to employees
```

### Sockets

```python
# Mini 5 line script using sockets
import socket
HOST = '127.0.0.1'
PORT = 7777
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST, PORT))
# Listen on 7777 using netcat
nc -lvnp 7777
# Execute the socket 5 lines script to connect to netcat
```

## Scripts

### Port Scanner

```python
#!/bin/python
import sys
import socket
from datetime import datetime
# Define our target
if len(sys.argv) == 2:
    target = socket.gethostbyname(sys.argv[1])  # Translate hostname to IPv4
else:
    print("Invalid amount of arguments.")
    print("Syntax: python3 scanner.py <ip>"
# Add pretty banner
print("-" * 50)
print("Scanning target " + target)
print("Time started: " + str(datetime.now()))
print("-" * 50)
try:
    for port in range(21,88):
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        socket.setdefaulttimeout(1)
        result = s.connect_ex((target,port))  # Returns an error indicator
        if result == 0:
            print(f"Port {port} is open.")
        s.close()
except KeyboardInterrupt:
    print("\nExiting program.")
    sys.exit()
except socket.gaierror:
    print("Hostname could not be resolved.")
    sys.exit()
except socket.error:
    print("Could not connect to server.")
    sys.exit()
```
