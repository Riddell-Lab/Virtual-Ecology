# Logisitic Population Growth
The populations that we have been modeling have been pretty unrealistic so far. Today, we are going to add a bit more realism by relaxing the assumption that resources are not limiting. Of course they are!

In previous models, we assumed that rates of birth and death were *density-independent*, but we know that both of these rates can depend on the overall population size. As populations get bigger, for instance, they tend to consume more resources, leaving fewer resources for further population growth.

To derive a population growth model that incorporates resource limitation, we begin with a form we are familiar with:

(*dN*/*dt*) = (*b'* - *d'*)N

but now we modify the birth and death rates so they are *density-dependent* and reflect crowding. The simplest formula for a decreasing birth rate is a straight line:

*b'* = b - aN

In this equation N is the population size, *b'* is the per capita birth rate, and b and a are constants. We can see from this equation that larger popuation sizes coincide with lower birth rates. We can similarly define the per capita death rate as:

*d'* = d + cN

which is defined similarly. We can now combine these three equations to get:

(*dN*/*dt*) = ((b - aN)-(d + cN))N

Then, through a series of rearrangments:

(*dN*/*dt*) = *rN*(1-((a+c)/(b-d))*N*

Because a, c, b, and d are all constants, we can define K:

K = (b-d)/(a+c)

The constant K is used for more than just mathematical convenience - it's the carrying capacity! (cue dramatic music)

Substituting K back into the equation gives:

(*dN*/*dt*) = *rN*(1-(N/K))

The logistic population growth model was introduced to ecology in 1838 by P.-F. Verhulst, and still now forms the basis for many models in ecology. The term (1-(N/K)) represents the **unused portion of the carrying capacity**. Suppose K = 100 and N = 7, the unused portion of the carrying capacity is (1-(7/100)) = 0.93. Thus the population will grow at roughly 93% of the growth rate of an exponentially increasing population (*rN*(0.93)). In contrast, if the population is close to K (N = 98), then the unused carrying capacity is then 0.02, so the population will grow at just 2% of the exponential growth rate.

If the population should ever exceed the carrying capacity, the term in the parentheses becomes negative, which means the growth rate is less than zero, and the population declines towards K. The population will also stop growing when N is equal to the carrying capacity.

We can also use the rules of calculus to integrate the logistic growth equation with respect to time and express population size as a function time:

N<sub>t</sub> = K/(1+[(K-N<sub>0</sub>)/N<sub>0</sub>]*e*<sup>-*rt*</sup>)

## Discussion topic
In an individual-based model, how would be simulate the effect of crowding or resource limitation so that they influence population growth?

## Exercise
We are going to recreate Fig. 2.2 from Gotelli, which illustrates the logistic population growth curve under two scenarios: N<sub>0</sub> = 200 and 5. Write a function that illustrates population size (N) over 100 time steps using the expression for logistic population growth. The intrinsic rate of increase is 0.10 and the carrying capacity is 100. Both curves associated with the different initial population sizes are displayed on the same figure.

Hint:
- write a function that takes the amount of time the population will grow and initial population size as a parameter.
- The initial population size is really going to be two elements, since we have two of them. Consider making this a list.
- In the function, write a for loop that iterates over the time sequence and calculates the new population size and appends the value to your new population list.
- You'll also need a nested for loop that runs over the number of values you have for *N<sub>0</sub>* as well.
- In the function, write a list that contains the same number of lists as the list containing the initial population sizes (perhaps with a list comprehension).

## Extra credit
Let's now recreate the plots for Figure 2.3. Write a function that illustrates the relationship between population size and population growth for logistic growth and exponential growth.

## Solution
```python
from math import *
import matplotlib.pyplot as plt

def logistic_growth(initial_size,time_steps,carrying_capacity,r):
    population = [[] for x in initial_size]
    for i in range(len(initial_size)):
        population[i].append(initial_size[i])
    for m in range(len(population)):
        for time in range(1,time_steps):
            population[m].append(carrying_capacity/(1+(((carrying_capacity-population[m][0])/population[m][0])*exp(-r*time))))
    for z in range(len(population)):
        plt.plot(population[z])
    plt.show()
    return population
    
logistic_growth([200,5],100,100,0.1)
```
Here's another solution by Emily using arrays:

```python
import numpy as np
import matplotlib.pyplot as plt
from collections import namedtuple

LogData = namedtuple("LogData", ("time", "pop"))

def log_pop(init_pop, k, r, time_steps):
    time = np.arange(0, time_steps+1, 1) #set an array
    pop = k / (1 + (((k - init_pop) / init_pop) * np.exp(-r * time))) #array maths
    return LogData(time, pop) #output tuple

# set parameters
INIT_POP = np.array([[200], [5]]) # initial population set as an array
K = 100 # carrying capacity
R = 0.10 # intrinsic rate of increase
TIME_STEPS = 100

# run the function with the set parameters
data = log_pop(INIT_POP, K, R, TIME_STEPS)

# plot the values
plt.style.use("grayscale")
fig, ax = plt.subplots(figsize=(10, 10), facecolor="w")
ax.set_xlabel("Time (t)", fontsize=14)
ax.set_ylabel("Population size (N)", fontsize=14)
for i in range(len(data.pop)): # for loop to plot your many populations
    ax.plot(data.time, data.pop[i], "k") #able to use tuple outputs directly
ax.set_xlim(0,100)
ax.set_ylim(0)

# text box to show r value
text = f"r = {R:.2f}" #round r to two decimal points

ax.annotate(text, xy=(1, 1), xytext=(-15, -15), fontsize=14,
            xycoords='axes fraction', textcoords='offset points',
            bbox=dict(facecolor='white', edgecolor = 'none'),
            horizontalalignment='right', verticalalignment='top')

# save figure
#plt.savefig("gottelli_log_growth.png")

plt.show()
```
