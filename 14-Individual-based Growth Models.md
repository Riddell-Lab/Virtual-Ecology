# Individual-based Growth Models

We made it! We are finally going to start creating our first individual-based models. First, we are going to develop our individual-based population growth model to simulate some of the population growth models we have become oh-so-familiar with.

# The environment
An individual-based model doesn't necessarily need to be spatially-explicit, but we are going to start with a spatially-explicit model. That means we need to have some way of creating a virtual space. Some of you have already begun creating your own virtual space, and we can use some of the modules that we have come to know over the last few weeks.

NumPy example
```python
import numpy as np

env = np.zeros(100)
env.reshape(10,10)
```
There are many other ways of creating an environment. Feel free to use another method if you prefer.

The goal today will be to simulate population growth similar to a bacterial colony growing. 

## Exercise
Now that we have our 'space', we can start simulating growth. For this exercise, write a program that simulates population growth in your space. Start with one individual in your space that is capable of producing a second individual in a location adjacent to the parents' location. Each individual can reproduce and each individual will also have a certain probability of dying. First, simulate population growth such that the growth will be exponential. Once you have finished this simulation, write a progam that simulates logistic population growth. For each simulation, plot the population size as a function of time.

If you'd like to create a gif from your arrays, try using the PIL module. First let's create an individual that can randomly teleport across your landscape.
```python
from PIL import Image as im
import random
import matplotlib.pyplot as plt

for i in range(10):
    env = np.zeros(100, np.uint8)
    random_number = random.randint(1,99)
    env[random_number] = 1.0
    env = np.reshape(env, (10, 10))
    plt.pcolor(env, cmap = 'binary' )
    plt.savefig('test'+str(i)+'.png')
```
And then create a gif using:
```python
import imageio
import glob
from pathlib import Path

p = Path('')
filenames = list(p.glob('test?.png'))

with imageio.get_writer('mygif.gif', mode='I') as writer:
    for filename in filenames:
        image = imageio.imread(filename)
        writer.append_data(image)
```
And then we can add the gif to our Jupyter Notebook using Markdown with:
```
![SegmentLocal](mygif.gif "segment")
```
# Solutions
Here is a solution by Ryan:
```python
import random
import matplotlib.pyplot as plt
import imageio
import glob
from pathlib import Path

def listGen(col, row):
    arr = [[0 for i in range(col)] for j in range(row)] #generate array with 'row' lists and 'col' elements per list w/ value 0
    arr[random.randrange(0,row)][random.randrange(0,col)] = 1 #randomly assign 1 element to a value of 1
    for row in arr: # Prints each list within the list as a line, so lists become rows and elements are columns
        print(row)
    print('\n')
    return arr


def move1space(arr):
    for i in range(len(arr)): # Identifies coordinates of the starting position
        for j in range(len(arr[i])):
            if arr[i][j] == 1:
                startY = i
                startX = j
                check = False
                while not check: #re-rolls move if the move would put the '1' outside of the array
                    move = random.randint(1,4)
                    if 0 < startY < len(arr)-1 and 0 < startX < len(arr[0])-1: #not against an edge
                        check = True
                    elif startY == 0 and move == 1: # Don't move up if you're at the top
                        continue
                    elif startY == len(arr)-1 and move == 3: #Don't move down if you're on the bottom
                        continue
                    elif startX == 0 and move == 4: #Don't move left if you're on the left edge
                        continue
                    elif startX == len(arr[0])-1 and move == 2: # Don't move right if you're on the right edge
                        continue
                    else: # If none of the 'continue' conditions are caught, the move is valid
                        check = True

                if move == 1: #Move up
                    arr[startY-1][startX] = 1
                elif move == 2: # Move right
                    arr[startY][startX+1] = 1
                elif move == 3: #Move down
                    arr[startY+1][startX] = 1
                elif move == 4: #Move left
                    arr[startY][startX-1] = 1

    for i in arr:
        print(i)
    print('\n')


def placeAndMove(col, row, num): #Generates landscape and starting position, then moves the subject a desired number of times. Each step is printed.
    x = listGen(col, row)
    for i in range(num):
        move1space(x)
        plt.pcolor(x, cmap='binary')
        plt.savefig('test' + str(i) + '.png')
    p = Path('')
    filenames = list(p.glob('test?.png'))

    with imageio.get_writer('mygif.gif', mode='I') as writer:
        for filename in filenames:
            image = imageio.imread(filename)
            writer.append_data(image)

placeAndMove(10,10,8)
```
