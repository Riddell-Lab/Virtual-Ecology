# Age-Structured Population Growth

## General notation

Our models so far have dealt with simple organisms, such as bacteria. But for most plants and animals, we know that the birth and death rates depend on the age of the individual.

**Discussion topic:** How does the age of the individual influence your study species?

For today's exercise, we are going to learn how to calculate *r* for a population in which birth and death rates depend on the age of the organism. First, let's review some general notation.

We use the notation *x* in parentheses to refer to the age of the individual, which we will use years of age for the rest of class. Newborns are classified as 0 a birth, 0.05 at 6 months and 1 at a year. This is different from age class, in which an organism is considered in age class 1 from birth until its next birthday. If the ages in a population range from 0 to k, the age classes range from 1 to k. *f(5)* indicates individuals of age 5, whereas *f<sub>5</sub>* indicates individuals in the fifth age class (between ages 4 and 5). Notice that age varies within age classes, but we treat them as equivalent.

## The fecundity schedule (*b(x)*)
The fecundity schedule consists of the average number of female offspring born per unit time to an individual female at a particular age. It is specifically a column of values represented by *b(x)* or *m(x)*. For example, if *b(6)* = 3, a female of age 6 will give birth to an average of 3 female offspring. The schedule is essentially a fecundity rate for females; thus we are only really counting females, which may or may not be reasonable given your study species. Let's review the Table 3.1. At which age class does fecundity peak?

*x* | *S(x)* | *b(x)* | *l(x)* | *g(x)* | *l(x)b(x)* | *l(x)b(x)x* | initial estimate *e<sup>-rx</sup>l(x)b(x)* | corrected estimate *e<sup>-rx</sup>l(x)b(x)*
------------ | ------------- | ------------ | ------------- | ------------ | ------------- | ------------ | ------------- | -------------
0 | 500 | 0 | 1.0 | 0.8 | 0.0 | 0.0 | 0.0 | 0.0
1 | 400 | 2 | 0.8 | 0.5 | 1.6 | 1.6 | 0.780 | 0.736
2 | 200 | 3 | 0.4 | 0.25 | 1.2 | 2.4 | 0.285 | 0.254
3 | 50 | 1 | 0.1 | 0.0 | 0.1 | 0.3 | 0.012 | 0.010
4 | 0 | 0 | 0.0 | --- | 0.0 | 0.0 | 0.0 | 0.0
 |  |  |  | |  | R<sub>0</sub> = 2.9 | Σ = 4.3 | Σ = 1.077 | Σ = 1.000


Variable | Estimate
------------ | -------------
*G* | 1.483
*r* estimated | 0.718
Correction added to *r* | 0.058
*r* (Euler) | 0.776

**Discussion Topic:** Is your species semelparous or iteroparous (or monocarpic or polycarpic for plants)? What would the fecundity schedule look like?

## The survivorship schedule (*l(x)*)
Of course, individuals need not only reproduce - they must survive! So how do we measured the survivorship of a population? Imagine that we follow a **cohort** of individuals that were all born at the same time. We follow them from birth until they all die, keeping track of the number of individuals that have survived at the start of each new year. This pattern can be represented as a column of numbers *S(x)*, which is a column of numbers represented the number of individuals in each age class.

To convert to a survivorship schedule (*l(x)*), we express the proportion of the original cohort that survives to age x. For example, the first entry in the table for *l(x)* is *l(0)*, which represents the survivorship of the cohort to birth. Since all individuals in the cohort have "survived" to the start, the value is always 1.0 (l(0) = S(0)/S(0) = 500/500 = 1.0). Similarly, since none of the individuals make it to the final age class, the survirorship is always 0.0 (0/500). Notice that 80% of the original cohort survives to age 1 but only 10% survive to age 3. 

Always take care to express these values with respect to the original size of the cohort.

## The survival probability (*g(x)*)
The survivorship schedule gives the probability of surviving from birth to age x, but what about comparing different ages directly? For that we need to use the survival probability, which we calculate using:

*g(x)* = *l(x+1)/l(x)*

By looking at the table we can see 80% of individuals survive from age 0 to 1 and 50% survive from age 1 to 2. In nature, we commonly have three types of survivorship curves?

**Discussion Topic**: What type of survivorship does your study species exhibit? What about humans?

## Calculating net reproductive rate (*R<sub>0</sub>*)
To estimate *r* from the *l(x)* and *b(x)* schedules, we have to calculate the net reproductive rate and generation time. The net reproductive rate is defined as the mean number of female offspring produced per female over her lifetime. To compute *R<sub>0</sub>* multiply each value of *l(x)* by the corresponding value of *b(x)* and sum these products across all ages.

*R<sub>0</sub>* = Σ*l(x)* *b(x)*

Notice that if none of the individuals died until the final age class, then *l(x)* would be 1.0 for each age class expect for the last and the equation would simply add up the lifetime production of offspring. If *R<sub>0</sub>* is greater than 1.0, there is a net surplus of offspring, and if it's less than one, then the population cannot replace itself due to high mortality.

## Generation time (*G*)
Generation time is defined as the average age of the parents of all the offspring produced by a single cohort and is calculated as:

G = Σ*l(x)* *b(x)*x/Σ*l(x)* *b(x)*

The units of *l(x)* and *b(x)* cancel in the numerator, leaving us with an answer in units of time (x). In the Table above, the generation time is 1.483

## Calculating the intrinsic rate of increase (*r*)

For an approximation of *r* we can use:

*r* ~ ln(R<sub>0</sub>)/G

However, this is only an approximation, though it's usually within 10% of the true value. To obtain the exact value, you must solve for the following equation:

1 = Σ *e<sup>-rx</sup>l(x)b(x)*

which is adapted from the **Euler equation**. In this equation, the sum notation applies to all age class from x = 0 to k. Because we know the *l(x)* and *b(x)* schedules, the only unknow value is *r*. The only way to solve for *r* is to plug in different values for *r* and adjust the amount upwards or downwards until you converge on an answer around 1.0. 

## Exercise
Write a program that iteratively solves for *r* by increasing or decreasing the value of *r* based on the Euler equation presented above. Use the table above for values to calculate *r* so that you can check your answer.

## Solution
```python
import math

def age_structure(l_x,b_x):
    R_0 = 0
    G_num = 0
    G_denom = 0
    for i in range(len(l_x)):
        R_0 += l_x[i]*b_x[i]
        G_num += l_x[i]*b_x[i]*i
        G_denom += l_x[i]*b_x[i]
    G = G_num/G_denom
    r = math.log(R_0)/G
    euler = 0
    for j in range(len(l_x)):
        euler += math.exp(-r*j)*l_x[j]*b_x[j]
    while abs(1 - euler) > 0.001:
        if 1 - euler < 0.0:
            r += 0.001
        else:
            r -= 0.001
        euler = 0
        for m in range(len(l_x)):
            euler += math.exp(-r*m)*l_x[m]*b_x[m]
    return r

l_x = [1.0,0.8,0.4,0.1,0.0]
b_x = [0,2,3,1,0]
age_structure(l_x,b_x)
```
Here's another solution using arrays by Audrey:
```python
# estimate r for age-structured population
# using Euler's formula - iterate to convergence
# 1 = Σ e^(-rx)*lx*bx
import numpy as np

def calc_r(r, x, lxbx):
    rx = np.multiply(r,x)
    approx = np.sum(np.multiply(np.exp(-rx),lxbx))
    return(approx)

def est_r(lx, bx):
    x = range(len(lx))
    lxbx = np.multiply(lx,bx)
    R0 = np.sum(lxbx)
    G = np.sum(np.multiply(lxbx,x)) / R0
    r = [np.log(R0) / G]
   
    euler = calc_r(r, x, lxbx)
    while euler < 0.998 or euler > 1.002:
        if euler > 1.001:
            r_new = r[-1] + 0.001
        elif euler < 0.999:
            r_new = r[-1] - 0.001
        r.append(r_new)
        euler = calc_r(r_new, x, lxbx)
    print("est r =", np.round(r[-1], 3), "\nR0 = ", np.round(R0, 3), "\nG = ", np.round(G,3), "\nconvergence value: ", euler)
 
lx = [1.0,0.8,0.4,0.1,0.0]
bx = [0,2,3,1,0]
```
