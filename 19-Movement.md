# Movement
We have not yet explicitly dealt with movement yet, but much of the script you have already written for reproduction has the basic rules for movement. However, instead of creating a new individual, the position of the individual would simply update to the new coordinate. 

Individuals in our simulations have generally been moving or reproducing in random directions. That may make sense for reproduction (i.e., diserpsal in a random direction), but it doens't really make sense for movement, especially for vertebrates. Animals typically have an orientation, facing one way or another. And the direction that they are facing biases which direction they travel.

## Von Mises
To explore orientation, we are going to use the Von Mises probability density function for the angle x.

The function contains parameters μ and κ are analogous to μ and σ2 (the mean and variance) in the normal distribution.

- μ is a measure of location (the distribution is clustered around μ)
- κ is a measure of concentration (a reciprocal measure of dispersion)
 - If κ is zero, the distribution is uniform, and for small κ , it is close to uniform.
 - If κ is large, the distribution becomes very concentrated about the angle μ.

### Exercise
Take a few moments to explore the vonmises() function in NumPy. Change the values associated with μ and κ to understand how they change the the distributions that you can sample.

```python
# import numpy
import numpy as np
import matplotlib.pyplot as plt
  
# Using vonmises() method
gfg = np.random.vonmises(0, 2, 5000)
  
plt.hist(gfg, bins = 50, density = True)
plt.show()
```

## Distance
The second issue that I would like to discuss is movement across the grid. The first aspect that we need to think about is scale. How quickly does your individual move across the landscape? Can they make it accross the landscape in 3 time steps? 100? Is there one answer? No, it depends. The rate at which your individual moves across the landscape should be based on empirical or theoretical expectations. For instance, salamanders move very little generally across the landscape, often occupying a small 1m2 space on the landscape, whereas bears roam for hundreds of kilometers.

You also want to make sure that your animals interact with your environment in a realistic way. If your lizard is moving through shade from underneath a bush, it probably shouldn't take them 2 years to get across the shade patch. Nor should it take 1 second. All of these decisions are important for structuring movement in your simulation.

### Exercise
Take a few moments to explore the beta() function in NumPy. Change the values associated with α and β to understand how the distribution that you can sample to generate distances.

```python
# Using beta() method
gfg = np.random.beta(0.1,1.0,1000)
  
plt.hist(gfg, bins = 50, density = True)
plt.show()
```

## Trigonometry
This is the moment! The moment that every middle school math teacher has been waiting for. The moment you need to use trigonometry. Basically we will use trigonometry to find the opposite and adjacent sides of a triangle. The opposite side is always the change in y and the adjacent side of the triangle is always the change in x. Once we have the angle that we travel and the distance, we can use SOHCAHTOA to find the change in x and y for each time step. Remember:

sin(θ) = opposite/hypotanus

cos(θ) = adjacent/hypotanus

tan(θ) = opposite/adjacent

## Exercise
Use trigonometry and the functions that I have provided to create a function that simulates continuous movement through space. Think simply here! You don't need to create a grid. Extra points for those that integrate with classes.

## Solution
```python
def move(start_x,start_y):
    orientation = rd.randint(0,359)
    x = start_x
    y = start_y
    coordinates_x = [x]
    coordinates_y = [y]
    colors = [0,1,2,3,4,5,6,7,8,9,10]
    for i in range(10):
        angle = math.degrees(np.random.vonmises(0.,5000.0))
        orientation += angle
        distance = np.random.beta(4.0,1.0)
        move_x = math.cos(orientation)*distance
        move_y = math.sin(orientation)*distance
        x += move_x
        y += move_y
        coordinates_x.append(x)
        coordinates_y.append(y)
    plt.scatter(coordinates_x,coordinates_y, s=200, c=colors, cmap='gray')
    plt.xlim(0, 10)
    plt.ylim(0, 10)
    return coordinates_x,coordinates_y,colors

move(5,5)
```
