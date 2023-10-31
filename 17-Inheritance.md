# Inheritance
During our last class, we introduced classes. By now, you have had time to begin 

Inheritance is a powerful feature in object oriented programming.It refers to defining a new class with little or no modification to an existing class. The new class is called derived (or child) class and the one from which it inherits is called the base (or parent) class.
Derived class inherits features from the base class where new features can be added to it. This results in re-usability of code.

```python
class Animal:
    kind = 'vertebrate'         # class variable shared by all instances
    def __init__(self, kind):
        self.species = kind     # instance variable unique to each instance
        
class Dog(Animal):
    def declare(self):
        print('I am an '+ str(self.species))
        
a = Dog('English wolfhound')
a.species
a.declare()
```

The built-in inheritance saves you some code by essentially copying the attributes and functions defined in the base class. The child class can also have specific functions that are only associated with the child class. Keep in mind that you can't add another __init__ to the child class.

```python
class Animal:
    kind = 'vertebrate'         # class variable shared by all instances
    def __init__(self, kind):
        self.species = kind # instance variable unique to each instance
        
class Dog(Animal):
    def declare(self):
        print('I am a(n) '+ str(self.species))
    def update_species(self,new_kind):
        self.species = new_kind
        
a = Dog('English wolfhound')
a.species
a.declare()
a.update_species('Golden doodle')
a.declare()
```
## Genetic algorithm
Now this is different than the inheritance that we more often think about as biologists. But now we have the tools to start thinking about how to implement a genetic algorithm.

Genetic algorithms are inspired by the evolutionary processes we are all fammiliar with, such as mutation, genetic crossover, natural selection, and drift. First, let's create an individual with a certain allelic combination that is passed on to their progeny. Below is an example of script of the mc1r gene that encodes pelage coloration in mice. In this example, we have an incredibly high mutation rate for each allele.

# Exercise
Develop a genetic algorithm that tracks allele frequencies in your population, assuming a diploid organism. Alleles should be inherited from parents with a certain probability of mutation from one allele to another. You can also try to add selection against or for certain genotypes by changing the probability of reproduction based on the genotype.

```python
import numpy as np
import copy
import matplotlib.pyplot as plt

class Individual:
    population = [] #new line that defines population
    def __init__(self, genotype,mutation_rate):
        self.mc1r = genotype
        self.population.append(self)
        self.mutation_rate = mutation_rate
    def reproduce(self):
        new_genotype = copy.copy(self.mc1r)
        if np.random.uniform(1,0) < self.mutation_rate:
            if self.mc1r[0] == 'a':
                new_genotype[0] = 'A'
            else:
                new_genotype[0] = 'a'
        if np.random.uniform(1,0) < self.mutation_rate:
            if self.mc1r[1] == 'a':
                new_genotype[1] = 'A'
            else:
                new_genotype[1] = 'a'    
        return Individual(new_genotype,self.mutation_rate)
           
a = Individual(['a','a'],0.05)
a.mc1r

def genetic_algorithm(time,individual):
    frequencies = [[],[],[]]
    for i in range(time):
        freq = allele_frequencies(individual)
        frequencies[0].append(freq[0])
        frequencies[1].append(freq[1])
        frequencies[2].append(freq[2])
        for j in range(len(individual.population)):
            individual.population[j].reproduce()
    colors = ['green','blue','orange']
    labels = ['homozygous a','homozygous A','heterozygous']
    for m in range(len(frequencies)):
        plt.plot(frequencies[m], color=colors[m], label=labels[m])
    plt.legend()
    plt.show()
    return frequencies

def allele_frequencies(individual):
    homozygous_a = 0
    homozygous_A = 0
    heterozygous = 0
    total = len(individual.population)
    for j in range(len(individual.population)):
        if individual.population[j].mc1r == ['a','a']:
            homozygous_a += 1
        elif individual.population[j].mc1r == ['A','A']:
            homozygous_A += 1
        else:
            heterozygous += 1
    return [homozygous_a/total,homozygous_A/total,heterozygous/total]

results = genetic_algorithm(20,a)
```
