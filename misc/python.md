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
// Some code
```

### Importing Modules

```python
// Some code
```

### Advanced Strings

```python
// Some code
```

### Dictionaries

```python
// Some code
```

### Sockets

```python
// Some code
```

## Scripts

### Port Scanner

```python
// Some codepy
```
