## Environmental stochasticity

So far, we have learned deterministic models of population growth. If we were to start over with the same set of conditions, we would always end up with the same answer. Nothing is left to chance and everything is left to the inputs.

To model stochasticity, we need to know more about the mean and variance for a particular input.

Imagine that a population is growing exponentially with a mean *r* and a variance in *r* (&sigma;<sup>2</sup><sub>r</sub>). We weill use this model to predict mean population size and the variance in population size.

We already know the formula for the mean population size. Gotelli provides the following formula for variance in population size:

&sigma;<sup>2</sup><sub>N<sub>t</sub></sub> = N<sup>2</sup><sub>0</sub> *e*<sup>2rt</sup>(*e*<sup>&sigma;<sup>2</sup><sub>r</sub>t</sup> - 1)

where &sigma;<sup>2</sup><sub>N<sub>t</sub></sub> is the variance in population size and &sigma;<sup>2</sup><sub>r</sub> is the variance in *r*. The equation tells us many things about how populations grow in stochastic environments. Much like the stock market or weather, the further we predict population growth through time, the less certain we become about the value. Also, the variance of N<sub>t</sub> is proportional to both the mean and variance of *r*. And if the variance of *r* is zero, then the equation collapses to zero and there is no stochasticity.

In some instances, the variability can be so large that it drives the population towards extinction, If the variance in r is greater than twice the average of r, the population will almost certainly collapse.

## Exercise

Now we are going to simulate stochastic population growth based on the equations provided by Gotelli. The exercise should create something that looks like Figure 1.3 in Chapter 1, but remember, it's stochastic so it won't look identical. Write a function that predicts population growth with environmental stochasticity. The initial size of the population should be 20 individuals and the population should reproduce for 20 years with continuous and stochastic population growth between time steps. The population exhibits an average *r* of 0.05 and a variance of *r* of 0.001.

Here's a hint:
* Define a function that takes an initial population size and the number of years the population will reproduce.
* Make a list that will hold your population size and another that will hold your population variance.
* Use a for loop that calculates the average population growth and variance in population growth. I would consider using something like arange() from NumPy because it can iterate over float values (think small time steps within the 20 years of the simulation).
* Once that's calculated, we can then pull from a normal distribution based on the mean and variance. I would recommend something like the random.normal() function in NumPy. Note you may need to transform variance into standard deviation in order to sample the full distribution of randomness.
* Append randomly generated values from the random function at each time step that correspond with the average and variance in the population size.
* Then plot the stochastic population.

# Demographic stochasticity
When an individual reproduces, they generally do not create a fraction of an individual (though some coral may bend some of these rules). Demographic stochasticity arises from the sequential patterns of birth and death, which are driven by their respective relative probabilities. Let's say births are twice as likely as deaths. In that case, we might expect the following sequence: BBDBBDBBDBBDBBD...and so on. But in reality, these events don't happen in sequence. They might occur like: BBBBDDBBDBBDDDBBBBBB. In a model of demographic stochasticity, the probability of a birth or death depends on the relative magnitudes of *b* and *d*:

P(birth) = b/(b + d)

P(death) = d/(b + d)

For instance, suppose for a population of chimpanzees, *b* = 0.55 births/(individual * year) and that *d* = 0.50 deaths/(individual * year). This yields an *r* of 0.05 individuals/(individual * year), with a doubling time of 13.86 years. Using the equations above, the probability of birth is 0.524 and the probability of death is 0.476. Note that these probabilities must add to 1.0 because the only events that can occur in this population are births and deaths.

Generally the population should increase over time, but depending on the initial population size, a chance run of deaths may bring the population close to extinction.

## Exercise

Write a program that simulates population growth with demographic stochasticity. Each population begins with 20 individuals, *b* = 0.55 births/(individual * year) and *d* = 0.50 deaths/(individual * year). Run the simulation four times to recreate Figure 1.4. For extra credit, develop a sensitivity analysis to determine the minimum population size at which extinction will become almost certain.

Here's a hint:
* Define a function that takes the initial population size and the number of years of reproduction.
* In this simulation, you are going to simulate population growth multiple times, so you probably want some sort of list of lists. You could use a list comprehension or take a shot at arrays.
* Then you want the population to experience a birth or death based on rates provided. The choices() function from the random library is a great option because it uses weights.
* Then write a for loop that iterates through the time sequence, randomly adding or subtracting a birth or death rate through time.
* Plot the result.

## Solutions

Environmental stochasticity
```python
import numpy as np

def env_stochastic(initial,steps):
    pop = [initial]
    pop_var = []
    r = [0.05]
    r_var = [0.0001]
    for i in np.arange(1,steps,0.1):
        pop.append(pop[0]*exp(r[0]*i))
        pop_var.append((pop[0]**2)*exp(2*r[0]*i)*(exp(r_var[0]*i)-1))
    pop_stochastic = []
    for j in range(len(pop_var)):
        val = np.random.normal(pop[j],3*np.sqrt(pop_var[j]),1)
        pop_stochastic.append(val)
    plt.plot(pop_stochastic)
    plt.show()
    return pop_stochastic

env_stochastic(20,20)
```
And here's another example by Emily
```python
import numpy as np
import matplotlib.pyplot as plt
rng = np.random.default_rng()

def env_stochastic(init_pop, time, mean_r, var_r, stoch_timesteps):
    tot_time = np.arange(0, time + 1, 1 / stoch_timesteps)
    pop_mu = init_pop * np.exp(mean_r * tot_time)
    pop_var = ((init_pop ** 2) * np.exp(2 * mean_r * tot_time) * (np.exp(var_r * tot_time) - 1))
    stoch_pop = rng.normal(pop_mu, 3 * np.sqrt(pop_var))    
    plt.plot(tot_time, stoch_pop, "-k", alpha = 0.3)
    plt.plot(tot_time, pop_mu, "k", label = "Mean Population Size")

INIT_POP = 20
TIME = 20
MEAN_R = 0.05
VAR_R = 0.0001
STOCH_TIMESTEPS = 30
FILE_NAME = "gottelli_env_stochastic.png"

plt.figure(figsize=(10, 10))
plt.xlabel("Time (t)")
plt.ylabel("Population size (N)")

env_stochastic(INIT_POP, TIME, MEAN_R, VAR_R, STOCH_TIMESTEPS)

plt.legend(loc = "upper left", fontsize = "x-large")
plt.show()
#plt.savefig(FILE_NAME)
```

Demographic Stochasticity
```python
from random import choices
import matplotlib.pyplot as plt

def demo_stoch(initial_size,steps,sims):
    population = [[initial_size] for m in range(sims)]
    b_or_d = [1, -1]
    weights = [0.54, 0.46]
    for j in range(len(population)):
        for i in range(steps):
            population[j].append(population[j][-1]+choices(b_or_d, weights)[0])
    for z in range(len(population)):
        plt.plot(population[z])
    plt.show()
    return population

demo_stoch(20,50,5)
```
And here's another example by Emily that uses arrays
```python
import numpy as np
import matplotlib.pyplot as plt
rng = np.random.default_rng()

def dem_stochastic(b, d, init_pop, steps):
    tot_b_d = np.arange(0, steps + 1, 1)
    birth_prob = b / (b + d)
    death_prob = d / (b + d)
    pop_birth = 1
    pop_death = -1
    b_or_d = rng.choice([pop_birth, pop_death], steps, p = (birth_prob, death_prob))
    # numpy's cumulative summation function:
    population = np.zeros((steps + 1, 1))
    population[0] = init_pop
    population[1:, :] = b_or_d[:, None]
    population = population.cumsum(axis = 0) # axis = 0, telling it to add across the row
    plt.plot(tot_b_d, population)
   
B = 0.55
D = 0.5
INIT_POP = 20
STEPS = 50
DRAWS = 4
FILE_NAME = "gottelli_dem_stochastic.png"

plt.figure(figsize=(10, 10))
plt.xlabel("Population births and deaths")
plt.ylabel("Population size (N)")

for i in range(DRAWS):
    dem_stochastic(B, D, INIT_POP, STEPS)

plt.show()
#plt.savefig(FILE_NAME)
```
