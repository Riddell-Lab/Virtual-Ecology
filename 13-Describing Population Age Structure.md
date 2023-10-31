# Describing Population Age Structure
Now that we have a program that can calculate *r* from fecundity and survivorship schedules, we also want to know the number of individuals in each age class of the population, which means we shift our notation from ages to age classes.

We will use *n<sub>i</sub>(t)* to indicate the number of individuals at time *t* in age class *i*. For example, *n<sub>1</sub>(3)* = 50 would indicate there are 50 individuals in the first age class at the third time step. Because there are *k* age classes in the population, the age structure at time *t* consists of a vector of abundances, which we indicate as:

```
         [n1(t)]
         [n2(t)]
n(t) =   [  .  ]
         [  .  ]
         [nk(t)]
```
         
For example, the vector for population in Table 3.1 after five years might be:
```
         [600]
         [270]
n(5) =   [100]
         [50]
```
For the next stage, we need to calculate survival probabilities (P<sub>i</sub>) and fertilities (F<sub>i</sub>) for each age class, which are related to the *l(x)* and *b(x)* schedules.

## Calculating survival probabilities and fecundities
We are going to assume a birth-pulse model with a postbreeding census, which means that individuals give birth on the day they enter the new age class and individuals are counted each year just after they breed.

The probability that an individual in age class *i* survives to the next age class is:

*P<sub>i</sub>* = *l(i)/l(i-1)*

Then it's easy to calculate the change in the number of individuals in a particular age class from one time period to the next using:

*n<sub>i+1</sub>(*t* + 1)* = *P<sub>i</sub>* *n<sub>i</sub>(*t*)

To calculate fertility of an age class, use:

*F<sub>i</sub>* = *b(i)P<sub>i</sub>*

The probability acts to discount any individuals that do not survive until they reproduce.

Once *F<sub>i</sub>* is known for each age class, we multiply these fertilities be the numner of individuals in each age class. This product is then summed over all age classes to calculate the number of new offspring:

*n<sub>1</sub>*(t+1) = Σ*F<sub>i</sub>n<sub>i</sub>*(t)

So we can now calculate the number of individuals in each age class for a single time step:

n<sub>1</sub>(t+1) = F<sub>1</sub>n<sub>1</sub>(*t*) + F<sub>2</sub>n<sub>2</sub>(*t*) + F<sub>3</sub>n<sub>3</sub>(*t*) + F<sub>4</sub>n<sub>4</sub>(*t*)

n<sub>2</sub>(t+1) = P<sub>1</sub>n<sub>1</sub>(*t*)

n<sub>3</sub>(t+1) = P<sub>2</sub>n<sub>2</sub>(*t*)

n<sub>4</sub>(t+1) = P<sub>3</sub>n<sub>3</sub>(*t*)

# The Leslie Matrix
We can represent the growth of an age-structured population in matrix form, using the Leslie Matrix (named after biologist Patrick Leslie). The Leslie Matrix as the following form:
```
    [F1 F2 F3 F4]
A = [P1 0  0  0 ]
    [0 P2  0  0 ]
    [0 0  P3  0 ]
```
Each column is the age class at time *t* and each row is the age class at time t+1. The zeroes are there because individuals cannot remain in the same age class from one year to the next.

We can now describe population growth as a simple matrix multiplication:

n(t+1) = An(t)

the population vector in the next time step equals the Leslie Matrix (A) multipled by the current population vector.

## Exercise
Write a program that recreates Figure 3.3 in Gotelli. There are two scenarios, one in which there are 200 individuals in the first age class and no individuals in age classes two, three, or four, and the second scenario has 50 individuals in each age class. Estimate population growth for 9 time steps. I would advise using the dot() function in NumPy for matrix alegebra. This problem will likely make you rely more on the NumPy library and dealing with arrays - which is great practice for starting the IBM portion of the class. Feel free to add the leslie matrix as a parameter or for extra credit, you can generate the leslie matrix from l(x) and b(x).

Here is the Leslie Matrix based on values from Table 3.1
```
    [1.6 1.5 0.25 0.0]
A = [0.8 0  0  0 ]
    [0 0.5  0  0 ]
    [0 0  0.25  0 ]
```

Hint: I would start by defining your leslie matrix and initial population vector. Then consider using the dot function in NumPy to calculate the next step of the population growth cycle. Once you have the basics here, you can write your function that takes the number of time steps and initial size of the population.

Here is another way to create a Leslie matrix:
```python
from scipy.linalg import leslie

leslie([1.6, 1.5, 0.25, 0],[0.8, 0.5, 0.25])
```

## Solution
```python
import numpy as np
import math
import matplotlib.pyplot as plt
from scipy.linalg import leslie

def leslie_sim(l_x,b_x,initial_size,steps):
    P_i = []
    print(b_x)
    for i in range(len(l_x)):
        if i+1 > len(l_x)-1:
            break
        else:
            P_i.append(l_x[i+1]/l_x[i])
    P_i = np.array(P_i)
    b_x = np.array(b_x)
    b_x = np.delete(b_x,0)
    F_i = np.multiply(P_i,b_x)
    P_i = np.delete(P_i,-1)
    leslie_matrix = leslie(F_i,P_i)    
    population = np.array(initial_size)
    age_classes = np.split(population,len(population))
    for i in range(1,steps):
        n_t_plus_1 = np.dot(leslie_matrix,population)
        population = np.array(n_t_plus_1)
        n_t_plus_1.resize((1, 4))
        age_classes = np.concatenate((age_classes,n_t_plus_1.T), axis=1)
    age_classes = np.log(np.array(age_classes))
    age_classes[np.isneginf(age_classes)]=0
    for m in range(len(age_classes)):
        plt.plot(age_classes[m])
    plt.show()

# assign variables
survival = [1.0,0.8,0.4,0.1,0]
fecundity = [0,2,3,1,0]
leslie_sim(survival, fecundity, [50,50,50,50],10)
leslie_sim(survival, fecundity, [200,0,0,0],10)
```
    


