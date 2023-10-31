 # Functions
 A function is like a miniprogram in a program. Let's create our first function using the define function:
```python
def hello():
    print('Howdy')

hello()
hello()
```
What did you learn? The main purpose of functions is to group code such that you don't need to copy and paste certain segments of code over and over again to perform the same operation. You always want to avoid duplicated/copying code in case you ever want to update your code. Also, if there's a type or error in the copied code, you have to find each time you used it. With a function, you only need to edit once.

## def statements with parameters

```python
def hello(name):
    print('Howdy, '+name)

hello('Jerry')
```
Parameters are variables that contain arguments. When a function is called with arguments, the arguments are stored in the parameters. The first time the hello function is called, it is passed the argument 'Jerry'. The program execution enters the function, and the parameter name is automatically set to Jerry, which is what gets printed by the print statement.

One special thing to note about parameters is that the value stored in a parameter is forgotten when the function returns. For example if you added print(name) after hello('Jerry') in the previous program, the program would give you a name error because there is no variable named name.

This is similar to how a program's variables are forgotten when the program terminates.

To review, the *define* function creates the function, the hello() statement *calls* the function while *passing* the name 'Jerry' to the function. Anything passed to the function is known as an *argument* (in this case 'Jerry') and variables that have arguments assigned to them are called *parameters*.

# The return function
When we think about the len() function, we often use it to evaluate the length of a list or string. In the following example:
```python
len('Jerry')
```
the function len() *returns* the number of letters in the name. This is called the *returned value* of a function. The return function consists of the return statement followed by whatever you want returned. Let's practice.

```python
def nameMe(name):
    if name == 'Jerry':
        return 'So glad to see you, Jerry!'
    else:
        return 'I have nothing to say to you.'
```        
## Exercise
Let's create a magic 8-ball. We remember these right? You shake it and it returns cryptic answers like 'Not a chance' and so on. Using the random module, write a function that returns a randomly selected string simulating a magic 8-ball. In addition, make each answer return the name of the user in each response (such as, 'Not a chance, Jerry'.

## Answer
```python
import random

def magic8ball(name):
    r = random.randint(0,4)
    if r == 0:
        return 'Not a chance, ' + name
    elif r == 1:
        return 'Do not do it, ' + name
    elif r == 2:
        return 'Seems risky, ' + name
    elif r == 3:
        return 'Absolutely, ' + name

magic8ball('Jerry')
```  
We can then assign the returned value to a variable. For instance:
```python
fortune = magic8ball('Jerry')
print(fortune)
```
Notice that printing fortune as many times as you want does not re-run the function. We can also use functions on a single equivalent line:
```python
print(magic8ball('Jerry'))
```
## Multiple parameters
Functions can also take multiple parameters. Make sure to keep track of the parameter order when executing your functions.
```python
def magic8ball(name,lucky):
    r = random.randint(0,4)
    if lucky == 'I am feeling lucky':
        if r == 0:
            return 'Not a chance, ' + name
        elif r == 1:
            return 'Do not do it, ' + name
        elif r == 2:
            return 'Seems risky, ' + name
        elif r == 3:
            return 'Absolutely, ' + name
    else:
        return 'You lose.'

a = magic8ball('Jerry','I am feeling lucky')

b = magic8ball('Jerry','I do not feel lucky')

c = magic8ball('Jerry','I am feeling lucky')

print(a,b,c,sep=', ')
```  
##Exercise
Write a function that uses two parameters. Each parameter is a random number, and if their sum is even, then the function returns 'the sum is even' and if the sum is odd, the function returns 'the sum is odd'.

## None
In python there is a value called ```None```, which represents the absence of a value. The non-value is the only value of the non-type data type.

Behind the scenes, python adds ```return none``` to the end of any function definition with no return statement. This is similar to how a while or for loop implicitly ends with a continue statement. Also if you use a return statement without a value that is just the return keyword by itself, then none is returned.

## Local vs Global Scope
Parameters and variables that are assigned in a called function are said to exist in that function's local scope. Variables that are assigned outside of all functions are said to exist in the global scope. The variable that exists in a local scope is called a local variable whereas a variable that exist in a global scope is called a global variable. A variable must be one of the other, it cannot be both.A local scope is created whenever a function is called. A global scope is created when your program begins. When a function returns the local scope is destroyed and these variables are forgotten. When your program terminates the global scope is destroyed and all of its variables are forgotten.

A local scope is created whenever a function is called. A global scope is created when your program begins. When a function returns the local scope is destroyed and these variables are forgotten. When your program terminates the global scope is destroyed and all of its variables are forgotten. Here are some rules.

Rule 1:
```python
def spam():
    eggs = 404
print(eggs)
```
What did you learn?

Rule 2:
```python
def spam():
    eggs = 404
    bacon()
    print(eggs)

def bacon():
    eggs = 101
```
What did you learn?

Rule 3: 
```python
def spam():
    print (toast)
toast = 512
spam()
```
Rule 4:
```python
eggs = 734

def spam():
    eggs = 404
    bacon()
    print(eggs)

def bacon():
    eggs = 101
    print(eggs)

spam()
print(eggs)
```
What did you learn?

Rule 1: Local variables cannot be used in the global scope

Rule 2: Local scopes cannot use variables in other local scopes

Rule 3: Global variables can be read from a local scope

Rule 4: Local and global variables can have the same name, though not recommended.

## The global statement
If you need to modify a global variable from within a function, use the global statement.

```python
eggs = 12
def spam():
    global eggs
    eggs = 567
    print(eggs)
spam()
```
So what does global do?

However if an assignment statement for a variable is not used in the function (at the local level) then the variable is automatically a global variable. For instance:
```python
hashbrowns = 13
def spam():
    print(hashbrowns)
spam()
```
You can see how this might get confusing and how you might think a variable is local when in fact it is global and you didn't want it to be. Also, the code within a function can only be local or global. You can't have the same variable be local and global in the same function.
```python
hashbrowns = 13
def spam():
    print(hashbrowns)
    hashbrowns = 34
spam()
```
It returns a nice error message.

## Another way of using random!
This function does not include the stop value and is an alternative to random.randint().

```python
import random
for i in range(10):
    print(random.randrange(0,4))
```

## Exception Handling
We never want our code to crash. And sometimes you run into a situation where you code will produce an error message. We don't want an error message - called an *exception* - we want our code to keep running until the end.
```python
def spam(divideBy):
    return 42/divideBy

print(spam(2))
print(spam(12))
print(spam(0))
print(spam(84))
```
Error can be handled with the ```try``` and ```except``` statements.

```python
def spam(divideBy):
    try:
        return 42/divideBy
    except ZeroDivisionError:
        print('Error: You cannot divide by zero.')

print(spam(2))
print(spam(12))
print(spam(0))
print(spam(84))
```
Other errors include NameError, SyntaxError, and TypeError. All of which we have seen so far.

## Exercise
Write a program that uses two functions. The first function randomly determines what you are having for breakfast and the second function uses what you are having for breakfast to determine how many laps to run around a track. For instance, if you are having granola for breakfast, you should run 3 laps around the track. The first function must call the second function.

If you finish this exercise, use the same randomly generated number to determine the proportion of a pizza you should eat for dinner. Be sure to account for any errors you might encounter with try or except statements.

## Solutions
Magic 8-ball
```python
import random

def magic8ball(name,lucky):
    r = random.randint(0,4)
    if lucky == 'I am feeling lucky':
        if r == 0:
            return 'Not a chance, ' + name
        elif r == 1:
            return 'Do not do it, ' + name
        elif r == 2:
            return 'Seems risky, ' + name
        elif r == 3:
            return 'Absolutely, ' + name
    else:
        return 'You lose.'

a = magic8ball('Jerry','I am feeling lucky')

b = magic8ball('Jerry','I do not feel lucky')

c = magic8ball('Jerry','I am feeling lucky')
print(a,b,c,sep=', ')
```
Exercise exercise!
```python
import random

def exerciseRegimen():
    Num = random.randrange(0,3)
    breakfast = 'empty'
    calories = 0.0
    if Num == 0:
        breakfast = 'eggs. '
        calories += 50
    elif Num == 1:
        breakfast = 'eggs and bacon. '
        calories += 100
    elif Num == 2:
        breakfast = 'eggs, bacon, and a waffle. '
        calories += 150
    exercise = howIntense(calories)
    return 'For breakfast, you will have ' + breakfast + 'That means you need to ' + exercise

def howIntense(calories):
    if calories <= 50:
        return 'run 2 laps.'
    elif calories > 50 and calories <= 100:
        return 'run 4 laps.'
    else:
        return 'run 6 laps.'
 ```
