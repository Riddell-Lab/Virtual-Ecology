# Species interactions
Species interactions play really important roles in many ecological processes, including population dynamics. By this point in your career, you've probably heard of many models that have been developed to deal with species interactions, like the Lotke-Volterra equations.

Today I am going to demonstrate a few different types of species interactions that you can use or steal for ideas.

Below is a spatially explicit model of competition. The simulation focuses only on two individuals, with each individual beginning at the exact same location. The model therefore beings with a competitive interaction in which there is a certain probability that one individual wins and another loses. If one loses, it moves 3-fold the distance of a normal movement and is displaced. Since they begin at the same location, the simulation always begins with a competitive interaction.

```python
import numpy as np
import copy
import matplotlib.pyplot as plt
import random as rd
import math as math

STEFAN_BOLTZMANN = 5.670373*10**(-8)

class Individual:
    def __init__(self,name,Mb,X,Y,minT,maxT,windspeed):
        self.name = name
        self.mass = Mb #mass (g)
        self.loc_x = X
        self.loc_y = Y
        self.E_S = 0.97 #emittance
        self.D = self.mass/1000.0 #characteristic dimension (m)
        self.length = self.D * 7.0 #the animal is 7 times longer than it is thick
        self.surface_area = 2*math.pi*self.D*self.length+2*math.pi*self.D**2 #surface area of cyclinder
        self.volume = math.pi*(self.D**2)*self.length
        self.body_water = 0.7 * self.volume
        self.hydration_history = []
        self.cwl = 2.20 * 10**-7
        self.boundary_conductance = 1.4*0.135*math.sqrt(windspeed/self.D)
        self.Tb = 10
        self.Te = 10
        self.minT = minT
        self.maxT = maxT
        self.energy_budget = 0.0
        self.energy_history = []
        self.Te_history = []
        self.Tb_history = []
        self.coordinates_x = [X]
        self.coordinates_y = [Y]
        self.orientation = rd.randint(0,359)
    
    def move(self):
        x = self.loc_x
        y = self.loc_y
        if x > 8 or x < 2 or y > 8 or y < 2:
            angle = math.degrees(np.random.vonmises(3000,2.0))
        else:
            angle = math.degrees(np.random.vonmises(0.,5000.0))
        self.orientation += angle
        distance = np.random.beta(4.0,1.0)
        move_x = math.cos(self.orientation)*distance
        move_y = math.sin(self.orientation)*distance
        if move_x + self.loc_x > 10 or move_x + self.loc_x < 0 or move_y + self.loc_y < 0 or move_y + self.loc_y > 10:
            self.move()
            return None
        self.loc_x += move_x
        self.loc_y += move_y
        self.coordinates_x.append(x)
        self.coordinates_y.append(y)
        return None
    
    def temperature(self,hour):#function that estimates ground temperatures
        average_T = ((self.minT + self.maxT)/2.0)
        A_0 = ((self.maxT - self.minT)/2.0)
        temperature = (average_T + A_0 * math.exp(-0./0.08)*math.sin(((math.pi/12)*(hour-8))-0./0.08))
        return temperature
        
    def radiative_conductance(self,temperature):
        return (4.*STEFAN_BOLTZMANN*(temperature+273.15)**3.)/29.3

    def operative_temperature(self,temperature,R_abs):
        R_abs = R_abs * self.loc_x
        self.Te = temperature + (((R_abs)-(self.E_S*STEFAN_BOLTZMANN*((273.5 + temperature)**4)))/(29.3*(self.boundary_conductance+self.radiative_conductance(temperature))))
        self.Te_history.append(self.Te)
        return(self.Te)
    
    def body_temperature(self,temperature,R_abs,initial_Tb):
        if self.Te >= initial_Tb:
            tau = math.exp(0.72+0.36*math.log(self.mass))
            self.Tb = (math.exp(-1./tau) * (initial_Tb - self.Te) + self.Te)
            self.Tb_history.append(self.Tb)
        if self.Te < initial_Tb:
            tau = math.exp(0.42+0.44*math.log(self.mass))
            self.Tb = (math.exp(-1./tau) * (initial_Tb - self.Te) + self.Te)
            self.Tb_history.append(self.Tb)
            
    def energy_analysis(self):
        energetic_costs = (10**((0.036*self.Tb)+0.57*math.log(self.mass)-1.95))*20.1
        energetic_intake = ((((((0.015*(self.Tb**3))-(0.81*(self.Tb**2))+(12.76*self.Tb)-43.06)*self.mass)/24.)*4.184))
        self.energy_budget += energetic_intake - energetic_costs
        self.energy_history.append(self.energy_budget)
        water_loss = (1.4665e-7*math.exp(0.081061*self.Tb))*self.surface_area
        water_intake = (energetic_intake/22000.0)*2.33 #add water from food per joule
        self.body_water += water_intake - water_loss
        self.hydration_history.append(self.body_water)
        
    def compete(self,competitor_x, competitor_y,time):
        if abs(self.loc_x - competitor_x) < 1.0 and abs(self.loc_y - competitor_y) < 1.0:
            if np.random.uniform(1,0) < 0.2:
                print(str(self.name)+' wins at time '+ str(time))
            else:
                print(str(self.name)+' lose at time '+ str(time))
                x = self.loc_x
                y = self.loc_y
                if x > 8 or x < 2 or y > 8 or y < 2:
                    angle = math.degrees(np.random.vonmises(3000,2.0))
                else:
                    angle = math.degrees(np.random.vonmises(0.,5000.0))
                self.orientation += angle
                distance = np.random.beta(100.0,0.1) * 3
                move_x = math.cos(self.orientation)*distance
                move_y = math.sin(self.orientation)*distance
                if move_x + self.loc_x > 10 or move_x + self.loc_x < 0 or move_y + self.loc_y < 0 or move_y + self.loc_y > 10:
                    self.move()
                    return None
                self.loc_x += move_x
                self.loc_y += move_y
                self.coordinates_x[-1] = self.loc_x
                self.coordinates_y[-1] = self.loc_y
                return None

a = Individual('a',1,5,5,10,40,0.1)
b = Individual('b',1,5,5,10,40,0.1)

for i in range(10):
    a.compete(b.loc_x,b.loc_y,i)
    b.compete(a.loc_x,a.loc_y,i)
    a.move()
    b.move()

colors = range(0,11)
plt.scatter(a.coordinates_x,a.coordinates_y, s=200, c=colors, cmap='gray')
plt.scatter(b.coordinates_x,b.coordinates_y, s=200, c=colors, cmap='OrRd')
plt.xlim(0, 10)
plt.ylim(0, 10)
```
But this is just with two individuals - not very impressive. What if we want to simulate competition between an entire population of reproducing individuals? We could try something like the model below. In the simulation below, reproduction is based on how much energy you have. Every individual is born with enough energy to reproduce once. The only way to get more energy to reproduce is to compete with other individuals. If you win, you get enough energy to reproduce (10 units). If you lose, you lose energy that you could allocated towards reproduction (5 units). 

```python
import numpy as np
import copy
import matplotlib.pyplot as plt
import random as rd
import math as math
import gc

class Individual:
    population = [] #new line that defines population
    def __init__(self, role, X, Y):
        self.species = role
        self.loc_x = X
        self.loc_y = Y
        self.orientation = rd.randint(0,359)
        self.population.append(self)
        self.name = str(rd.randint(1000,10000))
        self.coordinates_x = [X]
        self.coordinates_y = [Y]
        self.energy_balance = 10.0
        
    def reproduce(self):
        if self.energy_balance >= 10.0:
            new_species = copy.copy(self.species)
            new_x = copy.copy(self.loc_x)
            new_y = copy.copy(self.loc_y)
            new_orientation = copy.copy(self.orientation)
            if new_x > 8 or new_x < 2 or new_y > 8 or new_y < 2:
                angle = math.degrees(np.random.vonmises(3000,2.0))
            else:
                angle = math.degrees(np.random.vonmises(0.,5000.0))
            new_orientation += angle
            distance = np.random.beta(4.0,1.0)
            move_x = math.cos(new_orientation)*distance
            move_y = math.sin(new_orientation)*distance
            if move_x + new_x > 10 or move_x + new_x < 0 or move_y + new_y < 0 or move_y + new_y > 10:
                self.reproduce()
                return None
            new_x += move_x
            new_y += move_y
            self.energy_balance -= 10
            return Individual(new_species,new_x,new_y)
        else:
            pass
            
    
    def move(self):
        x = self.loc_x
        y = self.loc_y
        if x > 8 or x < 2 or y > 8 or y < 2:
            angle = math.degrees(np.random.vonmises(3000,2.0))
        else:
            angle = math.degrees(np.random.vonmises(0.,5000.0))
        self.orientation += angle
        distance = np.random.beta(4.0,1.0)
        move_x = math.cos(self.orientation)*distance
        move_y = math.sin(self.orientation)*distance
        if move_x + self.loc_x > 10 or move_x + self.loc_x < 0 or move_y + self.loc_y < 0 or move_y + self.loc_y > 10:
            self.move()
            return None
        self.loc_x += move_x
        self.loc_y += move_y
        self.coordinates_x.append(x)
        self.coordinates_y.append(y)
        return None
    
    def compete(self,time):
        for i in range(len(self.population)):
            if self.population[i].name == self.name:
                pass
            else:
                if abs(self.loc_x - self.population[i].loc_x) < 1.0 and abs(self.loc_y - self.population[i].loc_y) < 1.0:
                    if np.random.uniform(1,0) < 0.2:
                        print(str(self.name)+' wins at time '+ str(time))
                        self.energy_balance += 10.0
                    else:
                        print(str(self.name)+' lose at time '+ str(time))
                        self.energy_balance -= 5.0
                        x = self.loc_x
                        y = self.loc_y
                        if x > 8 or x < 2 or y > 8 or y < 2:
                            angle = math.degrees(np.random.vonmises(3000,2.0))
                        else:
                            angle = math.degrees(np.random.vonmises(0.,5000.0))
                        self.orientation += angle
                        distance = np.random.beta(100.0,0.1) * 3
                        move_x = math.cos(self.orientation)*distance
                        move_y = math.sin(self.orientation)*distance
                        if move_x + self.loc_x > 10 or move_x + self.loc_x < 0 or move_y + self.loc_y < 0 or move_y + self.loc_y > 10:
                            self.move()
                            return None
                        self.loc_x += move_x
                        self.loc_y += move_y
                        self.coordinates_x[-1] = self.loc_x
                        self.coordinates_y[-1] = self.loc_y
                else:
                    pass
            return None

a = Individual('prey',5,5)
population_size = []

for i in range(50):
    print('at time ' + str(i) + ', population size is ' + str(len(a.population)))
    for j in range(len(a.population)):
        a.population[j].reproduce()
        a.population[j].move()
        a.population[j].compete(i)
        if a.population[j].energy_balance < 0.0:
            print('at time ' + str(i) + ', individual ' + str(a.population[j].name)+' died.')
            a.population.pop(j)
    population_size.append(len(a.population))

plt.plot(population_size)
```
And what about predators and prey? We might divise a similar kind of model. Now I create a predator and prey, every time the prey moves, it gains energy (like foraging). It also runs the risk of being depredated. The predator has a 50% chance of catching the prey if its within one space of the predator, and it then gains half the amount of energy needed to reproduce. The predator also loses energy every time it moves.

```python
import numpy as np
import copy
import matplotlib.pyplot as plt
import random as rd
import math as math
import gc
import statistics

class Individual:
    population = [] #new line that defines population
    def __init__(self, role, X, Y):
        self.species = role
        self.loc_x = X
        self.loc_y = Y
        self.orientation = rd.randint(0,359)
        self.population.append(self)
        self.name = str(rd.randint(1000,10000))
        self.coordinates_x = [X]
        self.coordinates_y = [Y]
        self.energy_balance = 10.0
        
    def reproduce(self):
        if self.species == 'prey':
            if self.energy_balance >= 10.0:
                new_species = copy.copy(self.species)
                new_x = copy.copy(self.loc_x)
                new_y = copy.copy(self.loc_y)
                new_orientation = copy.copy(self.orientation)
                if new_x > 8 or new_x < 2 or new_y > 8 or new_y < 2:
                    angle = math.degrees(np.random.vonmises(3000,2.0))
                else:
                    angle = math.degrees(np.random.vonmises(0.,5000.0))
                new_orientation += angle
                distance = np.random.beta(4.0,1.0)
                move_x = math.cos(new_orientation)*distance
                move_y = math.sin(new_orientation)*distance
                if move_x + new_x > 10 or move_x + new_x < 0 or move_y + new_y < 0 or move_y + new_y > 10:
                    self.reproduce()
                    return None
                new_x += move_x
                new_y += move_y
                self.energy_balance -= 2
                return Individual(new_species,new_x,new_y)
            else:
                pass
        elif self.species == 'predator':
            if self.energy_balance >= 10.0:
                new_species = copy.copy(self.species)
                new_x = copy.copy(self.loc_x)
                new_y = copy.copy(self.loc_y)
                new_orientation = copy.copy(self.orientation)
                if new_x > 8 or new_x < 2 or new_y > 8 or new_y < 2:
                    angle = math.degrees(np.random.vonmises(3000,2.0))
                else:
                    angle = math.degrees(np.random.vonmises(0.,5000.0))
                new_orientation += angle
                distance = np.random.beta(4.0,1.0)
                move_x = math.cos(new_orientation)*distance
                move_y = math.sin(new_orientation)*distance
                if move_x + new_x > 10 or move_x + new_x < 0 or move_y + new_y < 0 or move_y + new_y > 10:
                    self.reproduce()
                    return None
                new_x += move_x
                new_y += move_y
                self.energy_balance -= 3
                return Individual(new_species,new_x,new_y)
            else:
                pass
            
    
    def move(self):
        x = self.loc_x
        y = self.loc_y
        if x > 8 or x < 2 or y > 8 or y < 2:
            angle = math.degrees(np.random.vonmises(3000,2.0))
        else:
            angle = math.degrees(np.random.vonmises(0.,5000.0))
        self.orientation += angle
        distance = np.random.beta(4.0,1.0)
        move_x = math.cos(self.orientation)*distance
        move_y = math.sin(self.orientation)*distance
        if move_x + self.loc_x > 10 or move_x + self.loc_x < 0 or move_y + self.loc_y < 0 or move_y + self.loc_y > 10:
            self.move()
            return None
        self.loc_x += move_x
        self.loc_y += move_y
        self.coordinates_x.append(x)
        self.coordinates_y.append(y)
        if self.species == 'prey':
            self.energy_balance += 0.5
        elif self.species == 'predator':
            self.energy_balance -= 0.5
        return None
    
    def depredate(self,time):
        for i in range(len(self.population)):
            if self.population[i].name == self.name:
                pass
            if self.species == 'prey':
                pass
            if self.population[i].species == 'prey':  
                if abs(self.loc_x - self.population[i].loc_x) < 1.0 and abs(self.loc_y - self.population[i].loc_y) < 1.0:
                    if np.random.uniform(1,0) < 0.9:
                        #print(str(self.name)+' ate prey at time '+ str(time))
                        self.energy_balance += 10.0
                    else:
                        #print(str(self.name)+' failed to catch prey at time '+ str(time))
                        self.energy_balance -= 0.1
                        x = self.population[i].loc_x
                        y = self.population[i].loc_y
                        if x > 8 or x < 2 or y > 8 or y < 2:
                            angle = math.degrees(np.random.vonmises(3000,2.0))
                        else:
                            angle = math.degrees(np.random.vonmises(0.,5000.0))
                        self.population[i].orientation += angle
                        distance = np.random.beta(100.0,0.1) * 3
                        move_x = math.cos(self.orientation)*distance
                        move_y = math.sin(self.orientation)*distance
                        if move_x + self.loc_x > 10 or move_x + self.loc_x < 0 or move_y + self.loc_y < 0 or move_y + self.loc_y > 10:
                            self.move()
                            return None
                        self.population[i].loc_x += move_x
                        self.population[i].loc_y += move_y
                        self.population[i].coordinates_x[-1] = self.loc_x
                        self.population[i].coordinates_y[-1] = self.loc_y
                else:
                    pass
            else:
                pass
            return None

a = Individual('prey',5,5)
b = Individual('predator',8,8)
prey_size = []
predator_size = []
predator_energy_balance = []

for i in range(25):
    #print('at time ' + str(i) + ', population size is ' + str(len(a.population)))
    for j in range(len(a.population)):
        a.population[j].reproduce()
        a.population[j].move()
        a.population[j].depredate(i)
        if a.population[j].energy_balance < 0.0:
            #print('at time ' + str(i) + ', individual ' + str(a.population[j].name)+' died.')
            a.population.pop(j)
    counts = [0,0]
    predator_energy = []
    for k in range(len(a.population)):
        if a.population[k].species == 'prey':
            counts[0] += 1.0
        else:
            counts[1] += 1.0
            predator_energy.append(a.population[k].energy_balance)
    prey_size.append(counts[0])
    predator_size.append(counts[1])
    predator_energy_balance.append(statistics.mean(predator_energy))

time = range(0,10)
plt.figure(figsize=(18, 6))
plt.subplot(131)
plt.plot(prey_size)
plt.xlim(0, 10)
plt.ylim(0, 300)
plt.subplot(132)
plt.plot(predator_size)
plt.xlim(0, 10)
plt.ylim(0, 50)
plt.subplot(133)
plt.plot(predator_energy_balance)
plt.show()
```
But what if we want to use two different classes for predators and prey?

```python
import numpy as np
import copy
import matplotlib.pyplot as plt
import random as rd
import math as math
import gc
import statistics

class Prey:
    population = [] #new line that defines population
    def __init__(self, color, X, Y):
        self.phenotype = color
        self.loc_x = X
        self.loc_y = Y
        self.orientation = rd.randint(0,359)
        self.population.append(self)
        self.name = str(rd.randint(1000,10000))
        self.coordinates_x = [X]
        self.coordinates_y = [Y]
        self.energy_balance = 10.0
        
    def move(self):
        x = self.loc_x
        y = self.loc_y
        if x > 8 or x < 2 or y > 8 or y < 2:
            angle = math.degrees(np.random.vonmises(3000,2.0))
        else:
            angle = math.degrees(np.random.vonmises(0.,5000.0))
        self.orientation += angle
        distance = np.random.beta(4.0,1.0)
        move_x = math.cos(self.orientation)*distance
        move_y = math.sin(self.orientation)*distance
        if move_x + self.loc_x > 10 or move_x + self.loc_x < 0 or move_y + self.loc_y < 0 or move_y + self.loc_y > 10:
            self.move()
            return None
        self.loc_x += move_x
        self.loc_y += move_y
        self.coordinates_x.append(x)
        self.coordinates_y.append(y)
        self.energy_balance += 0.5
        return None
    
    def reproduce(self):
        if self.energy_balance >= 10.0:
            if np.random.uniform(1,0) < 0.05:
                new_phenotype = 'blue'
            else:
                new_phenotype = copy.copy(self.phenotype)
            new_x = copy.copy(self.loc_x)
            new_y = copy.copy(self.loc_y)
            new_orientation = copy.copy(self.orientation)
            if new_x > 8 or new_x < 2 or new_y > 8 or new_y < 2:
                angle = math.degrees(np.random.vonmises(3000,2.0))
            else:
                angle = math.degrees(np.random.vonmises(0.,5000.0))
            new_orientation += angle
            distance = np.random.beta(4.0,1.0)
            move_x = math.cos(new_orientation)*distance
            move_y = math.sin(new_orientation)*distance
            if move_x + new_x > 10 or move_x + new_x < 0 or move_y + new_y < 0 or move_y + new_y > 10:
                self.reproduce()
                return None
            new_x += move_x
            new_y += move_y
            self.energy_balance -= 2
            return Prey(new_phenotype,new_x,new_y)
        else:
            pass
        
class Predator:
    population = [] #new line that defines population
    def __init__(self, preference, X, Y):
        self.preference = preference
        self.loc_x = X
        self.loc_y = Y
        self.orientation = rd.randint(0,359)
        self.population.append(self)
        self.name = str(rd.randint(1000,10000))
        self.coordinates_x = [X]
        self.coordinates_y = [Y]
        self.energy_balance = 10.0
        
    def move(self):
        x = self.loc_x
        y = self.loc_y
        if x > 8 or x < 2 or y > 8 or y < 2:
            angle = math.degrees(np.random.vonmises(3000,2.0))
        else:
            angle = math.degrees(np.random.vonmises(0.,5000.0))
        self.orientation += angle
        distance = np.random.beta(4.0,1.0)
        move_x = math.cos(self.orientation)*distance
        move_y = math.sin(self.orientation)*distance
        if move_x + self.loc_x > 10 or move_x + self.loc_x < 0 or move_y + self.loc_y < 0 or move_y + self.loc_y > 10:
            self.move()
            return None
        self.loc_x += move_x
        self.loc_y += move_y
        self.coordinates_x.append(x)
        self.coordinates_y.append(y)
        self.energy_balance -= 0.5
        return None
    
    def reproduce(self):
        if self.energy_balance >= 10.0:
            new_preference = copy.copy(self.preference)
            new_x = copy.copy(self.loc_x)
            new_y = copy.copy(self.loc_y)
            new_orientation = copy.copy(self.orientation)
            if new_x > 8 or new_x < 2 or new_y > 8 or new_y < 2:
                angle = math.degrees(np.random.vonmises(3000,2.0))
            else:
                angle = math.degrees(np.random.vonmises(0.,5000.0))
            new_orientation += angle
            distance = np.random.beta(4.0,1.0)
            move_x = math.cos(new_orientation)*distance
            move_y = math.sin(new_orientation)*distance
            if move_x + new_x > 10 or move_x + new_x < 0 or move_y + new_y < 0 or move_y + new_y > 10:
                self.reproduce()
                return None
            new_x += move_x
            new_y += move_y
            self.energy_balance -= 3
            return Predator(new_preference,new_x,new_y)
        else:
            pass
        
    def depredate(self,population,time):
        for i in range(len(population)): 
            if abs(self.loc_x - population[i].loc_x) < 1.0 and abs(self.loc_y - population[i].loc_y) < 1.0:
                if np.random.uniform(1,0) < 0.8:
                    #print(str(self.name)+' ate prey at time '+ str(time))
                    self.energy_balance += 50.0
                    population[i].energy_balance -= 100.0
                else:
                    #print(str(self.name)+' failed to catch prey at time '+ str(time))
                    #print(len(population))
                    #print(i)
                    self.energy_balance -= 1.0
                    x = population[i].loc_x
                    y = population[i].loc_y
                    if x > 8 or x < 2 or y > 8 or y < 2:
                        angle = math.degrees(np.random.vonmises(3000,2.0))
                    else:
                        angle = math.degrees(np.random.vonmises(0.,5000.0))
                    population[i].orientation += angle
                    distance = np.random.beta(100.0,0.1) * 3
                    move_x = math.cos(population[i].orientation)*distance
                    move_y = math.sin(population[i].orientation)*distance
                    if move_x + population[i].loc_x > 10 or move_x + population[i].loc_x < 0 or move_y + population[i].loc_y < 0 or move_y + population[i].loc_y > 10:
                        self.move()
                        return None
                    population[i].loc_x += move_x
                    population[i].loc_y += move_y
                    population[i].coordinates_x[-1] = population[i].loc_x
                    population[i].coordinates_y[-1] = population[i].loc_y
            else:
                pass
        return None


#Run Model
prey_population = Prey('orange',5,5)
predator_population = Predator('orange',8,8)
prey_size = []
predator_size = []
predator_energy_balance = []

for time in range(20):
    #print('at time ' + str(time) + ', prey population size is ' + str(len(prey_population.population)))
    #print('at time ' + str(time) + ', predator population size is ' + str(len(predator_population.population)))
    for j in range(len(prey_population.population)):
        prey_population.population[j].reproduce()
        prey_population.population[j].move()
    for k in range(len(predator_population.population)):
        predator_population.population[k].move()
        predator_population.population[k].reproduce()
        predator_population.population[k].depredate(prey_population.population,time)
    for each_predator in reversed(range(len(predator_population.population[:]))):
        if predator_population.population[each_predator].energy_balance < 0.0:
            predator_population.population.remove(predator_population.population[each_predator])    
    for each_prey in reversed(range(len(prey_population.population[:]))):
        if prey_population.population[each_prey].energy_balance < 0.0:
            prey_population.population.remove(prey_population.population[each_prey])
    prey_size.append(len(prey_population.population))
    predator_size.append(len(predator_population.population))

time = range(0,20)
plt.figure(figsize=(12, 6))
plt.subplot(121)
plt.plot(prey_size)
plt.xlim(0, 20)
plt.ylim(0, 300)
plt.subplot(122)
plt.plot(predator_size)
plt.xlim(0, 10)
plt.ylim(0, 50)
plt.show()

```
