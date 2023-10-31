# Environmental Variation
We now have the basic knowledge to develop IBMs of individuals moving and growing through space. But so far, our space has been relatively...boring. As organisms move through space, they encounter different environments, which can influence movement, dispersal, survival, reprduction, or any aspect of fitness.

We are going to focus today on temperature because it's one of the most influential environmental variables in nature. However, many of the techniques that we are going to use can be used for other variables, like vegetation, habitat types, precipitation, humidity, etc.

Temperature can be difficult to model because it varies across space and time. Moreover, because our planet rotates irregularly, we have seasons, at least at temperate latitudes. So temperatures vary with respect to location, the time of day, and day of year.

## Modeling daily variation in temperature
Temperature varies with time of day, but estimating the variation in temperature may be difficult unless you already have hourly estimates of air temperautre. Often, we have minimum and maximum air temperature for a particular day. Using equations from Campbell and Norman (2010), we can model variation in the time of day.


```python
import math
import matplotlib.pyplot as plt

def temperature(minT,maxT):#function that estimates ground temperatures
    daily_cycle = []
    average_T = ((minT + maxT)/2.0)
    A_0 = ((maxT - minT)/2.0)
    for i in range(24):
        daily_cycle.append(average_T + A_0 * math.exp(-0./0.08)*math.sin(((math.pi/12)*(i-8))-0./0.08))
    plt.plot(daily_cycle)
    plt.show()
    return daily_cycle
    
temperature(10,30)
```

## Modeling annual variation in temperature
As described above, we also know that temperatures vary across seasons as well. Fortunately we have ways of predicting daily minimum and maximum temperatures based on the monthly minumum and maximum temperatures.

```python
from scipy.interpolate import UnivariateSpline
import numpy as np
import matplotlib.pyplot as plt

def daily_minmax():
    maxTemps = np.array([13.9,14.0,17.0,22.0,26.0,31.0,36.0,36.0,34.0,32.0,26.0,19.0,14.0,13.9])
    minTemps = np.array([-5.9,-6.0,-3.0,0.0,4.0,10.0,15.0,18.0,17.0,13.0,5.0,2.0,-6.0,-5.9])
    days = np.array([-16, 15, 46, 75, 106, 136, 167, 197, 228, 259, 289, 320, 350, 381])
    annual_maxT = UnivariateSpline(days, maxTemps, k=3)
    annual_minT = UnivariateSpline(days, minTemps, k=3)
    DOY = np.arange(365)
    plt.plot(DOY,annual_maxT(DOY))
    plt.plot(DOY,annual_minT(DOY))
    plt.show()
    return annual_maxT[0]

daily_minmax()
```
## Modeling spatial variation in temperature
Once we have the daily minimum and maximum temperature we can combine them with the daily fluctuations to get hourly fluctuations. But now let's think about space.

```python
import numpy as np
from matplotlib import colors

def generate_maps():
    maxs = np.random.normal(35, 5, 100)
    maxs = maxs.reshape(10,10)
    mins = np.random.normal(15, 5, 100)
    mins = mins.reshape(10,10)
    return mins,maxs

a = generate_maps()
heatmap = plt.pcolor(a[0],cmap='coolwarm',vmin=5.0,vmax=50.0)
plt.show()
heatmap = plt.pcolor(a[1],cmap='coolwarm',vmin=5.0,vmax=50.0)
plt.show()
```
These maps can then be used to generate hourly fluctuations in temperature. We can also use data from maps!
```python
import numpy as np
ascii_grid = np.loadtxt("/Users/eriddell/Documents/Data_and_Projects/Salamander_model/dem_for_rb.asc", skiprows=6)
plt.pcolor(ascii_grid,cmap='Greys')
plt.gca().invert_yaxis()
plt.show()
```

## Class discussion
What are other types of habitat variables you are interested in and how would you try to model them conceptually?

## How does the environment influence movement?
Once we have our environment setup, we can make our movement or reproduction sensitive to the environmental conditions that we generated. In the example below, we see a randomly generated grid of shade, which we can think of as shade provided by vegetation. An animal is then moving through this environment, randomly shuttling between shaded and unshaded areas of different magnitudes.

![alt text](https://github.com/ecophysiology/IBM/blob/main/random_grid.png)

![alt text](https://github.com/ecophysiology/IBM/blob/main/explicit_Tb.png)

The details underlying this simulation are a little more complex than we have dealt with before. But next class, we are going to review different ways of simulating movement.

## Exercise
Generate an environmental layer. The layer can either be continuous, like temperature, or can be represented by discrete characters, like vegetation type or roads. Then using your population growth model, make reproduction or death a function of the environmental layer. Run the simulations with and without the simulation layer and then plot them to compare the models after 100 time steps.

