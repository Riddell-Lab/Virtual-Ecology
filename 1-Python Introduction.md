# Python

Python is a programming language. The name python comes from this real British comedy group Monty Python. As with any language, it has syntax rules for writing what is considered valid Python code in the Python interpreter software that reads source code written in Python language and performs its instructions. Your code (or any computer program) is a set of instructions that instruct the CPU to perform a defined task.

Python is a general purpose language, meaning it can be used to create a variety of different programs and isn’t specialized for any specific problems. This versatility, along with its beginner-friendliness, has made it one of the most-used programming languages today. Python is commonly used for developing websites and software, task automation, data analysis, and data visualization. 

We are using Jupyter, Spyder, and PyCharm as an integrated development environment (IDE), which is software for building applications that combines common developer tools into a single GUI.

## Why use Python?

* It has a simple syntax that mimics natural language, so it’s easier to read and understand. This makes it quicker to build projects, and faster to improve on them.
* It’s versatile. Python can be used for many different tasks, from web development to machine learning.
* It’s beginner friendly, making it popular for entry-level coders.
* It’s open source, which means it’s free to use and distribute, even for commercial purposes.
* Python’s archive of modules and libraries—bundles of code that third-party users have created to expand Python’s capabilities—is vast and growing.
* Python has a large and active community that contributes to Python’s pool of modules and libraries, and acts as a helpful resource for other programmers. The vast support community means that if coders run into a stumbling block, finding a solution is relatively easy; somebody is bound to have run into the same problem before.

 A programming paradigm is a way of categorizing a programming language depending on its features. Two such paradigms are structured and object oriented programming. Python supports both types of paradigms! Structured programming allows developing a program using a set of modules or functions, while object oriented programming allows constructing a program using a set of objects and their interactions.

Writing efficient Python code often means writing memory-efficient code. The good thing about Python is that everything in Python is an object. This means that Dynamic Memory Allocation underlies Python Memory Management. When objects are no longer needed, the Python Memory Manager will automatically reclaim memory from them. Memory can be freed and reused when not required. It is important to understand that the management of the Python heap is performed by the interpreter itself and that the user has no control over it.

## What's the difference between Python and R?

The main distinction between the two languages is in their approach to data science. Both open source programming languages are supported by large communities, continuously extending their libraries and tools. But while R is mainly used for statistical analysis, Python provides a more general approach to data wrangling.

* Data collection: Python supports all kinds of data formats, from comma-separated value (CSV) files to JSON sourced from the web. In contrast, R is designed for data analysts to import data from Excel, CSV and text files.
* Data exploration: In Python, you can explore data with Pandas, the data analysis library for Python. You’re able to filter, sort and display data in a matter of seconds. R, on the other hand, is optimized for statistical analysis. With R, you’re able to build probability distributions, apply different statistical tests, and use standard machine learning and data mining techniques.
* Data modeling: Python has standard libraries for data modeling, including Numpy for numerical modeling analysis, SciPy for scientific computing and calculations and scikit-learn for machine learning algorithms. For specific modeling analysis in R, you’ll sometimes have to rely on packages outside of R’s core functionality. But the specific set of packages known as the Tidyverse make it easy to import, manipulate, visualize and report on data.
* Data visualization: While visualization is not a strength in Python, you can use the Matplotlib library for generating basic graphs and charts. However, R was built to demonstrate the results of statistical analysis, with the base graphics module allowing you to easily create basic charts and plots.

## Getting started

We're going to begin the exercise using Jupyter Notebooks. They’re great for showcasing your work. You can see both the code and the results. It’s easy to use other people’s work as a starting point. You can run cell by cell to better get an understanding of what the code does. Jupyter Notebooks are a spin-off project from the IPython project, which used to have an IPython Notebook project itself. The name, Jupyter, comes from the core supported programming languages that it supports: Julia, Python, and R.

* Open Anaconda Navigator
* Open Jupyter Notebook
* Create new notebook
* Review basic operations of notebook
  * Naming notebook
  * Running cells, print('Hello World!')
  * Run through Menus: File, Edit, View, etc.
  * Adding rich content
  * Homepage: View what's running

## Entering expressions
Operatior | Operation
------------ | -------------
** | Exponent
% | Modulus/remainder
// | Integer division/floored quotient
/, *, +, - | Division, Multiplication, Subtraction, Addition

## Exercise

Take a few minutes to explore order of operations and how spaces influence computation. Explore all of the operators to understand what they do and what makes them fail. Also feel free to throw in some parentheses.

The order of operations (or precedence) follows as ** first, then *, /, //, and % next from left to right, and then + and - are last. Parentheses can be used to override these rules. Spaces don't matter between operators (sidenote, they do for indentation!) but a single space is convention.

## Data types
Data Type | Example
------------ | -------------
Integers | -1, 0, 1, 2, 345
Floating-point numbers | -1.23, 3.45672, 954.521
Strings | 'bob', 'a', 'Hello!'

## Exercise
Take a few minutes to understand how to concatenate: use the + operator to add two strings together. Then try adding a string with an integer. Then type multiplying a string by an integer. Then try multiplying two strings. Then try multipling a string by a float point.

## Storing values in variables
A variable holds a particular data type in memory. Values are stored in variables within the assignment statement, which consists of a variable name, an = (called an assignment operator), and the value to be stored. Use names that are informative!

## Helpful functions
str(), int(), and float(). These functions are particularly helpful when you want to print a number within text. Take a minute to learn how these functions work.

## Exercise
Take a few minutes to assign a value to a variable. You can start with the letter 'x' as your variable name but then explore other variable names. 
* What happens if you choose a different letter? 
* What happens when you put a space or a dash within the variable name? 
* What happens if you put a number first in a variable name?
* What happens if you add two variables together?
* What happens if you overwrite a variable?
* What happens when you use an assignment operator on your variable (try integers, floats, and strings as your variables)?
* Are variable names case sEnSiTiVe?

Next, let's create a program with some common functions:
```python
print('Hello World!')
print('What is your name?')
my_name = input()
print('Nice to meet you, ' + my_name)
print('There are ' + str(len(my_name)) + ' characters in your name!')
```
Which three new functions did you use and what do they do?

## Exercise
Take a few minutes to create your own program that asks how old you are and also tells you how old you will be in 2025.



