# Discrete Population Growth

We made many assumptions to generate a model that describes exponential population growth. However, we know that this type of population growth doesn't really happen in nature. All populations are liminted by resources, and our model assumes that there are no limitations to resources. But we present this model because every population has the potential for exponential growth. The model recognizes the multiplicative nature of population growth and positive feedback that increases growth at an accelerating rate. This is particularly true when resources are temporarily unlimited, such as seen with insect pests and some invasive species.

In the last class, we mentioned that time was viewed a continuous variable, such that at any one time point, we could predict the population size. Of course, populations do not usually grow continuously. Organisms have certain times of year when they reproduce, leading to discrete moments of population growth. These populations are said to have *non-overlapping generations*, and they are modeled with a **discrete difference equation** rather than a **continuous differential equation**.

Suppose the population increases (or decreases) each year by a constant proportion r<sub>d</sub>, the **discrete growth factor**. The population size next year would be:

N<sub>t+1</sub> = N<sub>t</sub> + r<sub>d</sub>N<sub>t</sub>

Combining terms gives:

N<sub>t+1</sub> = N<sub>t</sub>(1 + r<sub>d</sub>)

Let 1 + r<sub>d</sub> = &lambda;, the **finite rate of increase**. Then:

N<sub>t+1</sub> = &lambda;N<sub>t</sub>

&lambda; is always a positive number (unlike r in the previous lecture) and it measures the proportional change in population size from one year to the next. Thus &lambda; is the ratio of the population size during the next time period to the populations size for the current time period.

The general solution to this recursion equation is:

N<sub>t</sub> = &lambda;<sup>t</sup>N<sub>0</sub>

The equation is analgous to the equation that we used to model exponential population growth. Modeling how the population would look with a discrete model is not straightforward. But imagine that births are pulsed once per year and that deaths occur continuously throughout the year. The population curve would look like jagged saw blade, with a sharp vertical increase from births each spring, followed by a gradual decrease from deaths during the rest of the year.

Population growth would be modeled in discrete time steps, but deaths would occur continuously and proportionally to population size. Essentially the predictions for discrete and continuous models of exponential growth are qualitatively similar but later we will discuss how discrete models behave differently when we incorporate resource limitation.

## Exercise

Let's recreate Figure 1.2 on discrete population growth. In the example, births are pulsed at the beginning of the year and deaths occur continuously. The population reproduces twelve times once per year. The finite rate of increase is 1.2.

Hint: At this point, we don't have many options for modeling discrete and continuous population growth, nor does Gotelli really tell you how to model it. It's purpose here is to present the discrete equation. But we have the ability to make a figure that looks somewhat like Figure 1.2.

## Solution
Discrete simulation
```python
from math import *
import matplotlib.pyplot as plt

def discrete(initial_pop,years):
    pop = [initial_pop]
    for i in range(1,years):
        pop.append((1.2**i)*pop[0])
    pop_disc = []
    for j in range(len(pop)):
        pop_disc.append(pop[j])
        for k in range(1,366):
            pop_disc.append(pop[j]*exp(-0.002*k))
    plt.plot(pop_disc)
    plt.show()

discrete(2000,12)
```
Here another example that uses list comprehension:

```python
from math import *
import matplotlib.pyplot as plt

def discrete(initial_pop,years,r,lambdas):
    pop = [[initial_pop] for i in range(len(r))]
    for m in range(len(pop)):
        for i in range(1,years):
            pop[m].append((lambdas[m]**i)*pop[m][0])
    pop_disc = [[] for i in range(len(r))]
    for p in range(len(pop)):
        for j in range(len(pop[0])):
            pop_disc[p].append(pop[p][j])
            for k in range(1,365):
                pop_disc[p].append(pop[p][j]*exp(r[p]*k))
    plt.plot(pop_disc[0])
    plt.plot(pop_disc[1])
    plt.show()
    return pop_disc

a = discrete(2000,12,[-0.001,-0.002],[1.2,1.2])
```

Here's another example from Emily using arrays:

```python
import numpy as np
import matplotlib.pyplot as plt

def discrete_pop(init_pop, lam, birth_time, death_time, death_rate, file_name):
    time_birth = np.arange(0, birth_time + 1, 1)
    pop = ((lam ** time_birth) * init_pop) # array math
    disc_time = np.arange(0, 1, 1 / death_time)
    pop_discrete = pop[:, None] * np.exp(death_rate * disc_time) # None adds new axis to numpy array, avoids unequal lengths error if you used original pop array axis
    pop_discrete = pop_discrete.flatten()
    plt.figure(figsize=(10,10))    
    plt.plot(np.arange(0, birth_time + 1, 1 / death_time), pop_discrete, "k")
    plt.plot(pop, "or")
    plt.xlabel("Time (t)")
    plt.ylabel("Population size (N)")
    #plt.savefig(file_name)
    plt.show()

INIT_POP = 2000
LAM = 1.2
BIRTH_TIME = 12
DEATH_TIME = 365
DEATH_RATE = -0.4
FILE_NAME = "gottelli_discrete_growth.png"

discrete_pop(INIT_POP, LAM, BIRTH_TIME, DEATH_TIME, DEATH_RATE, FILE_NAME)
```
