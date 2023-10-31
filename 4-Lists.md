# Lists
A list is a value that contains multiple values in an ordered sequence. The term *list value* refers to the list itself, which can be stored as a variable or passed to a function. A list looks like:
```python
['cat','elephant','dog']
```
Lists are indicated by the presence of brackets. Values inside the list are called *items*, which are separated by commas.
```python
spam = ['cat','elephant','dog']
```
No spam is assigned a list value which contains multiple values, in this case strings.

## Indexing
In the above example, the Python code ```spam[0]``` would evaluate to ```'cat'```.

## Exercise
Create your own list and use a for loop to print each value in the list. What happens if you index beyond the length of the list? Can indexes be floats?

## Lists of lists
We can also create lists of lists. For instance:
```python
spam = [['cat','elephant','dog'],['alligator','lizard','snake']]
```
Now indexing changes a little bit, but is still relatively straightforward. Copy and paste the following code to explore how indexing works in a list of lists.
```python
spam[0]
spam[0][0]
spam[1][2]
spam[-1]
```
What did you learn? What does the first index do? And the second? How do negative indices work?

## Slices
We can also use *slices* to return several values from a list. For instance.
```
spam = ['cat','elephant','dog','mouse']
spam[1]
spam[1:3]
spam[1:4]
spam[0:-1]
spam[0:]
spam[:2]
spam[:0]
spam[:]
```
What did you learn?

## Changing values with indices
We can also use indices to change species values within a list - or even add to a list!

Enter each one of these separately!
```
spam = ['cat','elephant','dog','mouse']
spam[1] = 'zebra'
spam[2:] = 'horse'
spam[2:] = 2
spam[-1] = 2
```
What did you learn?

## List concatenation and replication
Lists can be concatenated and replicated much in the same way strings can.
```
[1,2,3] + [4,5,6]
[1,2,3] * 3
spam = ['cat','elephant','dog','mouse']
spam = spam + [1,2.5,3]
```
As you have already realized, lists can be combinations of strings, integers, and floats.

## Deleting values from a list
Deleting a value from a list is pretty simple.
```python
spam = [1, 2.5, 3]
del spam[1]
```
You can also use the del function on a variable and it will be erased from memory.

## Append and for loops
We can also use another function called *append* to add a value to the end of a variable.
```python
spam = []
for i in range(6):
    spam.append(i)
spam
```
We can also use the for statement on a list.
```python
spam = [400,1,734.2]
for i in spam:
    print(i)
```
In the above case, the index calls every  value from the list. But what if we want to iterate over the indices of the list?
```python
spam = [400,1,734.2]
for i in range(len(spam)):
    print(i)
```
## The in and not operators
What if we want to know whether a value exists within a list?
```python
spam = [400,1,734.2]
400 in spam
92 not in spam
```

## Multiple assignment trick (tuple unpacking)
This is a shortcut that allows you to assign multiple variables with the values in a list one line of code.

Instead of:
```python
spam = ['small','gray','loud']
size = spam[0]
color = spam[1]
disposition = spam[2]
```
You can write:
```python
spam = ['small','gray','loud']
size,color,disposition = spam
```

## Enumerate
We can also use the enumerate function to return both the index and item's index with a for loop:
```python
spam = ['small','gray','loud']
for index, item in enumerate(spam):
    print('Index ' + str(index) + ' in supplies is: ' + item)
```

## More random functions!
We are defintely going to use these functions a lot later, so become familiar with them.
If you want to randomly select an item from a a list, use the choice() function.
```python
import random
spam = ['small','gray','loud']
random.choice(spam)
```
We can also shuffle a list. Notice the list is permanently changed.
```python
import random
spam = ['small','gray','loud']
random.shuffle(spam)
```
## Exercise
Create a function that uses a parameter to create a list of random numbers. The random numbers should be generated between two values (could be parameters) and then the same function should also determine whether a user specified number (also a parameter) exists in the list of random numbers. If it does, then return the index of the random number or return a message that states the number does not exist.

## index() method
A method is like a function except it is distingueshed by the method part coming after the value or list, separated by a period. What if you want to find the index of a particular value? This might be helpful if you want to find an individual with the greatest body size or fitness.
```python
names = ['Sam','Ronaldo','Jen']
names.index('Sam')
```
Or if it were an integer/float:
```python
fitness = [3,5,6,9,1]
fitness.index(max(fitness))
```
## insert() method
Or what if we want to insert at a particular location?
```python
names = ['Sam','Ronaldo','Jen']
names.insert(1,'Rachel')
names
```

## remove() method
```python
names = ['Sam','Ronaldo','Jen']
names.remove('Sam')
names
```
What happens with this program?
```python
names = ['Sam','Ronaldo','Jen','Sam']
names.remove('Sam')
names
```
The del function is helpful when you know the index of the value you want to remove. The remove function is helpful when you don't.

## sort() method
Lists of numbers and values can be sorted numerically or alphabetically.
```python
spam = [2,6,3,1,28,9]
spam.sort()
spam
```
Try sorting a list of names. What if you try to sort a mixed list of strings and integers? Is sorting case sensitive?

We can also sort in reverse.
```python
spam = [2,6,3,1,28,9]
spam.sort(reverse = True)
spam
```
If you want to treat all strings as lowercase, you can use:
```python
spam = [a,A,z,C,X]
spam.sort(key = str.lower)
spam
```
## reverse() method
This is quite self-explanatory
```python
spam = [2,6,3,1,28,9]
spam.reverse()
spam
```

## Differences and similarities between lists and strings
Just like lists, strings can also be indexed.
```python
name = 'Zophie'
name[0]
```
```python
name = 'Zophie'
for i in name:
    print('****' + i + '****')
```
However, strings are immutable, whereas lists are mutable.
```python
name = 'Zophie a cat'
name[7] = 'the'
```
Rather you need to use
```python
name = 'Zophie a cat'
newName = name[0:7] + 'the' + name[8:12]
newName
```

## With lists, indentation doesn't matter
```python
name = ['Eric',
'Jerry',
        'Zophie',
        'Carl']
name
```
You can use this to make really long lists more readable.

## Tuples
Tuples are almost identical to lists except that they are immutable. They are helpful if you want to indicate to a reader that the sequence of values is not supposed to change.
```python
eggs = ('hello', 42, 0.5)
type(eggs)

eggs[1] = 99
```
What did you learn?

To convert between lists and tuples, you can use the list() and tuple() functions.

## Exercise
Write a function that creates a list of zeroes as long as a number defined by a parameter, then randomly changes one of the zeros to a 1. Once this works, then create another function that takes the list as a parameter, and randomly moves the 1 to another location. You can bias the movement if you would like.

## Solutions
### Exercise 1
```python
def randomNumz(stop,query_num,x):
    Numz_list = []
    Indices = []
    for i in range(x):
        Numz_list.append(random.randrange(0,stop))
    for i in range(len(Numz_list)):
        if Numz_list[i] == query_num:
            Indices.append(i)
    return ['I found your number at the following location(s): ' + str(Indices),Numz_list]
```
### Exercise 2
```python
import random

def population(how_big):
    habitat = [0] * how_big
    ran = random.randrange(0,how_big)
    habitat[ran] = 1
    return habitat

def move(population):
    
population(10)
```
