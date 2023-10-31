# Time Lags

As with all of the models we have used so far, logistic growth models make some unrealistic assumptions. They assume that when an individual is added to the population they immediately reduce the population growth. But we know that organisms reproduce seasonally in some cases, so the effect of population growth might not be seen until the subsequent season. With trees for instance, density-dependent reproduction does not occur until 50 years after the seedling stage. Individuals do not immediately adjust their growth and reproduction, it takes time to affect the population dynamics.

## Discussion topic
How do time lags affect population growth of your study species?

How can time lags be incorporated into the model? Suppose there is a time lag of length τ between the change in population size and its effect on population growth rate. Consequently the growth rate of the population at time t (dN/dt) is controlled by its size at time t - τ in the past. This means that the population can overshoot the carrying capacity!

To model the behavior, we use the following equation:

(*dN*/*dt*) = *rN*(1-(N<sub>t-τ</sub>/K))

The behavior of the **delay differential equation** depends on two factors: (1) the length of the time lag (τ) and (2) the response time of the population which is inversely proportional to *r*. Populations with fast growth rates have short response times and vice versa.

The ratio of time lag τ to response time, or *rτ*, controls population growth. If *rτ* is small (0 < *rτ* < 0.368), the population inceases smoothly to the carrying capacity. If *rτ* is medium (0.368 < *rτ* < 1.570), then the population first overshoots, then undershoots the carrying capacity followed by damped oscillations. These eventually diminish with time until K is reached. If *rτ* is large (*rτ* < 1.570), the population enters a **stable limit cycle** with periodically rising and falling about K, but never settling on it.

## Exercise
Create a function that predicts population size based upon the delay differential equation for population growth. Model three scenarios that coincide with small, medium, and large ratios between time lag and response time (*rτ*). Plot each simulation on top of each other. There are no rules for initial population size or the number of time steps, but they should display the general patterns in Figure 2.5 of Gotelli. Let's start with a growth rate of 0.1, an initial size of 5, a carrying capacity of 100, and the number of time steps at 200. But feel free to play around with the parameters.

Hint:
- Write a function that takes inputs for initial population size, time steps, carrying capacity, and growth rate.
- Define your population list based on the initial popualtion size.
- Write a for loop that iterates through the time steps.
- Use the formula to calculate the growth rate at each time step.
    - Note that if there is a lag, you may need an if statement to deal with the time lag (how are you going to base the growth rate on a population size that does not exist?)
- Add the change in population to the list to track the total population over time.
- Plot the lists.

## Periodic variation in time lags

Now what happens if the carrying capacity also varies with time? We know that the carrying capacity may fluctuate over a given time period because of a change in resources or conditions for reproduction. The cyclic function can be described with a cosine function:

K<sub>t</sub> = k<sub>0</sub> + k<sub>1</sub>[cos(2πt/c)]

where K<sub>t</sub> is the carrying capacity at time t, k<sub>0</sub> is the mean carrying capacity, k<sub>1</sub> is the amplitude of the cycle, and c is the length of the cycle. As time increases, the cosine term in the parentheses varies cyclically from -1 to 1. Thus, during a single cycle length c, the carrying capacity varies from k<sub>0</sub>-k<sub>1</sub> to k<sub>0</sub>+k<sub>1</sub>.

## Discussion topic

How does carrying capacity change in your population? And how does that influence how you will model the effect of changes in carrying capacity in relation to how the population grows?

## Exercise
Write a function that simulates periodic variation in carrying capacity over 100 years under two scenarios: one with a small population growth rate (r = 0.10) and another when population growth rate is larger (r=0.50). The initial population size is 100 and the carrying capacity is 100. The amplitude and length of the cycle are similar between populations with large and small growth rates.

## Hint
If you use your function from the previous exercise, you can incorporate both time lags AND carrying capacity. Just simply define carrying capacity prior to making any calculations of growth rate.

## Solutions
Time lag exercise
```python
import matplotlib.pyplot as plt
import numpy as np

def lags(r,K,N_0,steps):
    time = np.arange(1,steps,0.1)
    response_time = [0.2,1.2,1.6]
    population = [[N_0] for m in response_time]
    for i in range(len(response_time)):
        for j in range(len(time)):
            tau = int(response_time[i]/r)
            if j <= tau:
                rate = r*population[i][-1]*(1-(population[i][-1]/K))
            else:
                rate = r*population[i][-1]*(1-(population[i][tau*-1]/K))
            population[i].append(population[i][-1]+rate)
    for m in range(len(population)):
        plt.plot(population[m])
    plt.show()
    #return population, time

lags(0.001,100,5,10000)
```
Another solution that combines both time lags and a variable carrying capacity
```python
import matplotlib.pyplot as plt
import math

def lags(r,K,N_0,steps,k_1,c):
    population = [N_0]
    for i in range(steps):
        Ks = K+k_1*(math.cos((2*math.pi*i)/c))
        if i <= 20:
            rate = r*population[-1]*(1-(population[-1]/Ks))
        else:
            rate = r*population[-1]*(1-(population[-15]/Ks))
        population.append(population[-1]+rate)
    plt.plot(population)
    plt.show()

lags(0.1,100,1,1000,50,100)
```
