# Operative Temperature
Today, we are going to learn about a fairly specific yet broadly relevant phenomenon: the concept of operative temperature. Operative temperature is defined as the equivalent blackbody temperature that provides the same head load (or cold stress) as is present in the natural environment of the animal. It combines air temperature and radiation in a single equivalent temperature. The operative temperature is mathematically the air temperature plus or minus some temperature increment which depends on the absorbed radiation, wind speed, characteristic dimension of the animal, and temperature.

In a room or metabolic chamber (aka a blackbody cavity), the absorbed radiation is equal to the emittance at air temperature so operative temperature is equivalent to air temperature. But in nature, organisms can be exposed to solar radiation, shade, and high wind speeds. All of which can influence how much radiation is absorbed. In direct sunlight, the operative temperature can be much hotter than air temperature.

We intuitively know that operative temperature is important. And so do animals.


<p align="center">
  <img width="300" height="450" src="https://i.redd.it/3awufio7wqkz.jpg">
</p>

The methods are calculating operative temperature in natural environments can be remarkably complex, especially if you want to estimate the amount of solar radiation absorbed throughout the day. For today, we are just going to play around with radiation absorbed, and not estimate the actual amount of radiation absorbed from the sun based on the location on the globe, day of year, and time of day. If you want to do that, you will need to dig much deeper into Campbell and Norman.

```python
import numpy as np
import copy
import matplotlib.pyplot as plt
import random as rd
import math as math

STEFAN_BOLTZMANN = 5.670373*10**(-8)

class Individual:
    def __init__(self,Mb,X,Y,minT,maxT):
        self.mass = Mb #mass (g)
        self.loc_x = X
        self.loc_y = Y
        self.E_S = 0.97 #emittance
        self.D = 0.01 #characteristic dimension (m)
        self.boundary_conductance = 1.4*0.135*math.sqrt(.1/self.D)
        self.Tb = 10
        self.Te = 10
        self.minT = minT
        self.maxT = maxT
        self.Te_history = []
        self.Tb_history = []
        self.coordinates_x = [X]
        self.coordinates_y = [Y]
        
    def move(self):
        orientation = rd.randint(0,359)
        x = self.loc_x
        y = self.loc_y
        angle = math.degrees(np.random.vonmises(0.,5000.0))
        orientation += angle
        distance = np.random.beta(4.0,1.0)
        move_x = math.cos(orientation)*distance
        move_y = math.sin(orientation)*distance
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

a = Individual(100,5,5,10,30)

for i in range(24):
    a.move()
    a.operative_temperature(a.temperature(i),100)
    a.body_temperature(a.temperature(i),100,a.Tb)

colors = range(0,25)
time = range(0,24)
plt.figure(figsize=(12, 6))
plt.subplot(121)
plt.scatter(a.coordinates_x,a.coordinates_y, s=200, c=colors, cmap='gray')
plt.xlim(0, 10)
plt.ylim(0, 10)
plt.subplot(122)
plt.plot(time,a.Te_history)
plt.plot(time,a.Tb_history)
plt.show()
```
The following simulation shows a couple things. First, as the individual moves through space, temperature fluctuates and the individual experiences greater radiation based on where they are located on the grid. We also notice that body temperatures do not match operative temperatures. Nor should they!

Operative temperatures describe the instantaneous head load, but it takes time for the animal to heat up or cool down. If we change the mass of the animal, we can see how these dynamics change through time.
