# Classes
We've come a long way with our ability to code in Python and learning some of the basics in population growth models. Now it's time to learn about one of Python's strongest features for implementing an individual-based model - Classes!

Classes provide a means of bundling data and functionality together. Creating a new class creates a new type of object, allowing new instances of that type to be made. Each class instance can have attributes attached to it for maintaining its state. Class instances can also have methods (defined by its class) for modifying its state.

Python classes provide all the standard features of Object Oriented Programming and the class inheritance mechanism allows you to easily pass along attributes and functions specific to a class to another class. Objects can contain arbitrary amounts and kinds of data. 

Let's first look at some simple examples:

```python
class Dog:
    kind = 'canine'         # class variable shared by all instances
    def __init__(self, name):
        self.name = name    # instance variable unique to each instance
```
In this example, we define a class type of Dog. A dog has an attribute, in this case is 'kind' and the kind is canine. Note the use of self here. Later we will call the name, but first we need to define it.

We also use the ```def __init__(self,name):``` to define the name of the dog. To define the name, we simply call:

```python
d = Dog('Fido')
e = Dog('Buddy')
```
Now we have defined two dogs with two different names, but they also have the same kind.
```python
d.kind                  # shared by all dogs
#'canine'
d.name
#'Fido'
e.kind                  # shared by all dogs
#'canine'
e.name
#'Buddy'
```
## Class exercise
Let's take a moment to write a class together for an animal, but in this case, we can have two types of animals: cats and dogs.

```python
class Organism:
    def __init__(self, species):
        self.type = species
    def do_something(self):
        if self.type == 'dog':
            print('I am a dog')
        elif self.type == 'cat':
            print('I am a cat')

a = Organism('cat')
a.do_something()
```
How might this be helpful?

Our functions can also adjust attributes of the class.
```python
class Dog:
    def __init__(self, name):
        self.name = name
        self.tricks = []    # creates a new empty list for each dog
    def add_trick(self, trick):
        self.tricks.append(trick)
 ```
 Keep in mind that if tricks were defined prior to the init method, then every class type would have that attribute. Now we can add tricks to a specific individual:
 ```python
d = Dog('Fido')
e = Dog('Buddy')
d.add_trick('roll over')
e.add_trick('play dead')
d.tricks
e.tricks
```
## Integrating classes into your individual-based models
So how are we going to use classes? So far, we have been using a grid-based method of zeroes and ones to simulate an individual based model. Now, we have the tools necessary to create individuals across a landscape that differ in attributes. One of the most important attributes that we have dealt with so far is location. So how would we deal with an individual that reproduces to produce another individual at another location? Let's try something like this!
```python
class Individual:
    def __init__(self, location):
        self.x = location[0]
        self.y = location[1]
    def reproduce(self,new_location):
        return Individual(new_location)
```
Now we can define our first individual and have them produce another:
```python
a = Individual([1,2])
b = a.reproduce([3,4])
```
We can also consider sharing a list of population among all organisms!
```python
class Individual:
    population = [] #new line that defines population
    def __init__(self, location):
        self.x = location[0]
        self.y = location[1]
        self.population.append(self) #new line
    def reproduce(self,new_location):
        return Individual(new_location)
```
Now we can define our individuals again, and notice that we are automaticlly keeping track of every individual in the population. And now we call population.
```python
a = Individual([1,2])
b = a.reproduce([3,4])

a.population
b.population
```
# Exercise
Integrate the reproduction code that you have into a classes framework and execute a population growth model using individuals. Once that's done, give each individual a random probability associated with reproduction to see how some individuals might contribute more to population growth than others.
