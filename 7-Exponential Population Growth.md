# Exponential Population Growth

Today we begin to develop population growth models that help us understand how populations grow and the assumptions that are made to describe various patterns of population growth.

In ecology, we are often interested in the processes that contribute to population growth so that we can explain the size of the population. Populations increase from a combination of births and deaths as well as when new individuals arrive (immigration) and when resident individuals depart (emigration). Each of these operate at different spatial scales, with births and deaths happening locally and immigration and emigration occuring with other populations.

Thus, population growth can be descirbed as:

*N<sub>t+1</sub>* = *N<sub>t</sub>* + *B* - *D* + *I* - *E*

where *N<sub>t+1</sub>* is the population size in the next time period, *N<sub>t</sub>* is the current population size, *B* is births, *D* is deaths, *I* is immigration, and *E* is emigration. To simplify things, we are going to assume that the population is **closed** such that there is no movement between any populations. This assumption is often not true in nature but is mathematically convenient.

We will also assume that population growth is **continuous**, meaning that each time step is infinitely small and can be described by a smooth curve. This model also allows us to model population growth rate (*dN*/*dt*) with a continuous differential equation.

To derive an equation of population growth, we need to think about how *rates* of birth and death vary. One of the most important factors is population size. A large population will produce more progeny per unit time than a small population. So we can describe *B* and *D* rates using:

*B = bN* and *D = dN* 

where *b* and *d* are the **instantaneous birth** and **death rates** respectively and *N* is the population size. These rates can also be density dependent but let's assume they are not for now. By substiting and rearranging, we get:

(*dN/dt*) = (*b-d*)*N*

Let *b-d* equal the constant *r*, the **instantaneous rate of increase** or sometimes called the **intrinsic rate of increase**. The value of *r* determines whether a population increases exponentially (*r* > 0), stays the same (*r* = 0), or decreases (*r* < 0). The units of *r* are individuals per individual per unit time, thus *r* measures the per capita growth rate, which is simply the difference between *b* and *d*. Thus we have our differential equation:

(*dN/dt*) = *rN*

This equation tells us the population growth rate, not the population size. However, if the equation is integrated with respect to time, the result can be used to project or predict population growth:

*N<sub>t</sub>* = N<sub>0</sub>*e*<sup>*rt*</sup>

where N<sub>0</sub> is the intitial population size, *N<sub>t</sub>* is the population size at time *t*, and *e* is a constant, the base of the natural logarithm (e ~ 2.718). Now let's put it to work!

## Exercise
We are going to recreate Figure 1.1 in Gotelli. The figure demonstrates the relationship between population size and time with five different values for the instantaneous rate of increase (*r* = 0.02, 0.01, 0.00, -0.003034, and -0.02) for a populution that starts with 100 individuals and grows for 100 time steps. Write a function that estimates population size across these five different values of *r*. Then plot the results on a figure with one column and two rows. The first top subplot shows the untransformed population size and in the bottom subplot, population size is log-transformed using the natural log.

Did you finish? The doubling time of a population describes the amount of time it takes for a population to double and is described by *t<sub>double</sub>* = ln(2)/*r*. Write a function that plots the relationship between doubling time and *r* for 20 populations with instantaneous rates of increase that randomly range from 300 to 0.000075 individuals per day.

## Topics for discussion

What are the assumptions of the model?

## Solution

```python
import numpy as np
from math import *
import matplotlib.pyplot as plt


def exp_growth(initial_size, time_steps):
    r = np.array([0.02, 0.01, 0.00, -0.003034, -0.02])
    population_size = [[initial_size],[initial_size],[initial_size],[initial_size],[initial_size]]
    time = np.arange(1,time_steps,1)
    for j in range(len(r)):
        for i in range(len(time)):
            next_step = population_size[j][0]*(exp((r[j]*time[i])))
            population_size[j].append(next_step)
    plt.plot(np.arange(0,100),results[0], 'k', np.arange(0,100),results[1], 'k', np.arange(0,100),results[2], 'k',np.arange(0,100),results[3], 'k',np.arange(0,100),results[4], 'k')
    plt.show()
    return population_size

exp_growth(100,100)
```
