# Flow control

Today we are going to learn flow control statements, which decide which python instructions to execute under certain conditions. First, remember the help() function. These flow control statements correspond to symbols in flow charts which can be helpful to understand the sequence of events in a computer program.

<p align="center">
  <img width="450" height="300" src="https://automatetheboringstuff.com/images/000105.jpg">
</p>

First, we will talk about using equivalences and point out an interesting feature of Python. Enter the following into your notebook:
```python
47 == '47'
```
What did you learn? What happens when you compare a float to a string or an integer?

## Boolean data
Boolean data types only have two values: ```True``` and ```False```. Boolean is capitalized because the data type is named after mathematician George Boole. When entered as Python code, the Boolean values ```True``` and ```False``` lack the quotes you place around strings and they always start with the capital T or F. Let's explore some basics.

```python
spam = True
spam
type(spam)
true
True = 2+2
```
What did you learn?

Boolean values are used in expressions and can be stored in variables. If you don't use proper case (```True``` and ```False```) for varible names, you will receive an error message.

## Comparison operators
Operator | Meaning
------------ | -------------
== | Equal to
!= | Not equal to
<, > | Less than, Greater than
<=, >= | Less than or equal to, Greater than or equal to

## Exercise
Take a few minutes to explore these new operators. Be sure to try with combinations of integers, floats, and strings. Can you use one Boolean to evaluate another?

## Boolean operators
The three Boolean operators (```and```, ```or```, and ```not```) are used to compare Boolean values. They evaluate expressions down to a Boolean value. Let's explore these operators in detail, starting with the *and* operator.

The ```and``` and ```or``` operators always take two Boolean values (or expressions), so they're considered binary operators. The ```and``` operator evaluates an expression to ```True``` if both Boolean values are ```True```.

```python
True and True
True and False
False and False
```

The ```or``` operator evaluates an expression to ```True``` if either of the Boolean values is ```True```.

```python
False or True
False or False
```

The *not* operator operates only on one Boolean value or expression. It simply evaluates to the opposite Boolean value.
```python
not True
not not True
```
## Mixing Boolean and Comparison Operators
Predict the response!
```python
(4 < 5) and (5 < 6)
(1 == 2) or (2 == 4/2)
Jerry = 100
(5>=0.5) and (99 > Jerry - 2)
2 + 2 == 4 and not 2 + 2 == 5 and 2 * 2 == 2 + 2
```

## Elements of flow control
A flow control statement decides what to do based on whether its *condition* is True or False. A condition is just a more specific name in the context of flow control statements. Lines of code can then be grouped together in *blocks*, which are orgaized based on indentation. Blocks result in large sections of code never being executed based on certain conditions. There are three rules for blocks: 1) they begin when the indentation increases, 2) they can contain other blocks, 3) blocks end when the indentation decreases to zero or to a containing block's indentation.

The *if()* statement is one of the most common flow control statements in Python. It simply states 'if a condition is True, then execute the code in the clause.' Each if statement consists of four elements: the if keyword, a Boolean condition, a colon, and an indented block of code on the next line.

```python
name = 'Jerry'
password = 'tuna'
if name == 'Jerry':
  print('Hello, Jerry')
  if password == 'tuna':
    print('Access granted.')
  else:
    print('Access denied.')
```
The *else* statement can optionally follow the if statement and is executed only if the if statement is False. The else statement requires the keyword, a colon, and an indented block of code called the else clause.

The *elif* statement is used if you would like to add many possible clauses. The statement is an "else if" statement that always follows an if or another elif statement. It is another statement that is checked if the previous statement was False and consists of the elif keyword, a Boolean condition, a colon, and an indented block of code on the next line

## Exercise
Create a program that prompts the user for their age and returns whether the user is too young to vote, old enough to vote, and too young to retire. Change the order of your elif statements to determine whether order of the elif statements matters. What happens if an else statement is not included and the conditions are always False?

## Exercise
Write a program using the if statement that determines whether integers, floats, or strings are considered True or False. For instance:

```
NumOfGuests = input()
if NumOfGuests:
    print('Make sure you have even food for everyone.')
```
Change the number of guests to see if you numbers have Boolean values. What if you aren't having anybody over?

## The while loop
The ```while``` loop helps you execute a block of code over and over again until a condition is met. The code will be executed as long as the while condition is True. The ```while``` statement consists of the while keyword, a Boolean condition, a colon, and an indented block of code on the next line. Let's compare the if statement with the while statement.

```python
spam = 0
if spam < 5:
    print('Hello, world.')
    spam = spam + 1
```
And with the while statement
```python
spam = 0
while spam < 5:
    print('Hello, world.')
    spam = spam + 1
```
What did you learn?

Here's a fun ```while``` loop.

```python
name = ''
while name != 'your name':
    print('Please type your name.')
    name = input()
print('Thank you!')
```
Break statements are helpful for exiting infinite while loops - which frequently act as bugs in programs.
```python
while True:
    print('Please type your name.')
    name = input()
    if name == 'your name':
        break
print('Thank you!')
```
If name was not 'your name' in the loop above, you will need to use control + c or restart the kernel. The ```continue``` function is also helpful for jumping back to the start of the loop. Let's look at another infinite loop.

```python
while True:
    print("What is your name?")
    name = input()
    if name != 'Joe':
        continue
    print('Hi Joe, what is the password?')
    password = input()
    if password == 'swordfish':
        break
print('Access granted')
```

The ```pass``` function can also be helpful. It essentially says 'do nothing.'

## Exercise
Change the continue function to pass. What happens?

## For loops
The ```while``` is great and all, but what if you want to loop through a specific number of steps? Your answer is the ```for``` loop (and the ```range()``` function). The for code looks something like: ```for i in range(5):```

The ```for``` loop includes the for keyword, a variable name, the in keyword, a call to the ```range``` method, a colon, and an indented block of code.
```python
for i in range(5):
    print('Jerry can count to ' + str(i))
```
## Exercise
Write a program that uses a ```for``` loop to calculate the sum of all numbers between 0 and 100. Then perform the same exercise using the while loop.

## Start, stop, and step with range()
The ```range()``` function is extremely helpful for counting at specific steps and intervals. For instance:
```python
for i in range(12,15):
    print(i)
```
Also, we can tell the ```range``` function how many steps to take between counting:
```python
for i in range(10,20,2):
    print(i)
```
We can also use ```range()``` to count backwards:
```python
for i in range(5,-1,-1):
    print(i)
```
## Exercise
Use a ```for``` loop to accept number from a user and calculate the sum of all number from 1 to a given number. Then, use a ```for``` loop to print every number divisible by 5 between 1 and a user-provided number.

## Importing modules
All python programs can call basic set of functions called *built-in functions* including the print(), input(), len() functions you've seen before. Python also comes with a set of modules called the *standard library*. Each module is a python program that contains a related group of functions that can be embedded into your programs. For example the math module has mathematics related functions, the random module has a random number related functions, and so on.

Importing a module has consists of the import keyword, the name of the module, and possible more modules separated by commas.
```python
import random
for i in range(1,10):
    print(random.randint(1,10))
```
Important note - do not name your Python programs afters modules. You can overwrite them.

If you don't want the random prefix (random.randint) then you say enter:

```python
from random import *
```
However a note of caution that doing so makes your code more difficult to read because you won't necessarily know where a function is coming from. You can also use:
```python
import random as ran
for i in range(1,10):
    print(ran.randint(1,10))
```
Another module is the sys module.
```python
import sys

while True:
    print('Type exit to exit')
    response = input()
    if response == 'exit':
        sys.exit()
```
## Exercise
Create a program that generates a random number between two values and gives the user a specific number of attempts to guess the number. Indicate whether the user's guess is too high or too low with each guess. If they guess the number, they win and if they can't, they lose.
