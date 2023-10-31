# Energy budgets
We are starting to get into the nitty gritty details of some IBMs. At this point it might be helpful to give additional traits to your taxa of interest. As a physiologist, I often think about energy balanace. There are many factors that contribute to the energy budget of an animal. Here is a figure illustrating the various components.

<p align="center">
  <img width="900" height="450" src="https://onlinelibrary.wiley.com/cms/asset/13d89985-108e-4576-9e9e-ca465bb21b87/ecog12495-fig-0002-m.jpg">
</p>

Today we are going to focus on a few of these, with the first being metabolic rate. But before we can get into metabolic rate or any metric of performance, we need to talk about performance curves. The performance curve expresses the environmental sensitivity of a particular trait, such as metabolic rate or water loss. The trait can vary with respect to any environmental variable, but most often we ask questions about the thermal performance curve (TPC). Ectotherms generally exhibit a asymmetrical 'hump' shape that is skewed to the left. You may also hear or read the term 'reaction norm', which is essentially the same thing.


<p align="center">
  <img width="450" height="450" src="https://www.researchgate.net/profile/Sascha-Krenek/publication/221716210/figure/fig1/AS:305632667291651@1449879914905/General-shape-of-a-thermal-performance-curve-Relationship-between-environmental.png">
</p>

There are several ways of approximate these relationships in your IBM. For metabolism, metabolic rates are driven by temperature and body size, mostly - this is true for both endotherms and ectotherms (though the shape of the TPC for endotherms is quite different). For ectotherms, we can assume that metabolism is modeled by an exponential relationship between temperature and metabolic rate. If we want to include body mass, we might include an equation that the following:

Metabolic rate = 10<sup>((a*T)+ b * log(Mb)-c))</sup>

where T is temperature and Mb is body mass. Also shown are the coefficients for the model (a, b, and c).

For additional functions, I would point you towards work by Angilletta titled "Estimating and comparing thermal performance curves" published in the Journal of Thermal Biology. Interestingly, he concludes that a quadratic function also approximates the relationship between temperature and performance pretty well. The philosophy here is very much a Goldilocks effect - performance is often maximized between two extremes and falls on either side of the maximum. This is also true for a function like energy intake (or underlying processes that capture probability, sprint speed, etc.).

We might also be interested in knowing how quickly an animal dehydrates. Answering this question requires a fairly deep understanding of environmental biophysics, but we can approximate the relationship, assuming the water loss rate is proportional to the rise in temperature. Of course, hydration also depends on the body size, or rather the surface area-to-volume ratio.

We might start with a cutaneous water loss rate, which is a function of the temperature. Then, we would multiply this value by the total surface area of the individual to estimate a total water loss rate.

We can then assume that the animal loses water at an exponential rate as temperatures increase. The assumption here is based on the fact that the drying power of the air rises exponentially with temperature.

```python
import numpy as np
import copy
import matplotlib.pyplot as plt
import random as rd
import math as math

STEFAN_BOLTZMANN = 5.670373*10**(-8)

class Individual:
    def __init__(self,Mb,X,Y,minT,maxT,windspeed):
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
            
    def energy_analysis(self):
        energetic_costs = (10**((0.036*self.Tb)+0.57*math.log(self.mass)-1.95))*20.1
        energetic_intake = ((((((0.015*(self.Tb**3))-(0.81*(self.Tb**2))+(12.76*self.Tb)-43.06)*self.mass)/24.)*4.184))
        self.energy_budget += energetic_intake - energetic_costs
        self.energy_history.append(self.energy_budget)
        water_loss = (1.4665e-7*math.exp(0.081061*self.Tb))*self.surface_area
        water_intake = (energetic_intake/22000.0)*2.33 #add water from food per joule
        self.body_water += water_intake - water_loss
        self.hydration_history.append(self.body_water)

a = Individual(4,5,5,10,40,0.1)

for i in range(24):
    a.move()
    a.operative_temperature(a.temperature(i),100)
    a.body_temperature(a.temperature(i),100,a.Tb)
    a.energy_analysis()

colors = range(0,25)
time = range(0,24)
plt.figure(figsize=(24, 6))
plt.subplot(141)
plt.scatter(a.coordinates_x,a.coordinates_y, s=200, c=colors, cmap='gray')
plt.xlim(0, 10)
plt.ylim(0, 10)
plt.subplot(142)
plt.plot(time,a.Te_history)
plt.plot(time,a.Tb_history)
plt.subplot(143)
plt.plot(time,a.energy_history)
plt.subplot(144)
plt.plot(time,a.hydration_history)
plt.show()
```
