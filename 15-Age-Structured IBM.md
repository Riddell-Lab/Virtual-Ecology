# Age-structured Individual-based Model

Now that we have our population growing through time, we can also start to incorporate some of dynamics associated with the age-structured population growth model. In the population growth model, we used age-specific fecundity and survival probabilities, and our individual-based models will need to be developed in much of the same way.

For now, the best way to simulate age would be to change the value of your individual, with each value representing an age class (1 = ages 0 to 1, etc.). Then with each time step (which could be a year or day), each individual ages from one year to the next. After a certain age, they have a 100% chance of mortality and their space is then converted into an empty cell. At each time step, each individual will have a certain probability of mortality and a specific fecundity (which I would start as a probability of reproduction). Remember that fecundities and survival change with a given age class.

For instance, we might use the uniform distribution in NumPy to simulate reproduction. Here is an example where an individual has a 50% chance of reproduction:

```python
if np.random.uniform(1,0) > 0.5:
    print('You reproduced!')
else:
    print('You did not reproduce.')
```

It might also help to use recursive functions, which we have not covered yet. These can help you recursively loop through a function that you are defining until a condition is met. For instance:

```python
def recur(count):
    a = random.randint(1,10)
    print(a)
    count = count
    if a != 7:
        count += 1
        recur(count)
    else:
        print(a,count)
recur(0)  
```

For the purposes of making your images, I would change the colors from binary to something like viridis. See the example below.

```python
import numpy as np
from PIL import Image as im
import random
import matplotlib.pyplot as plt

for i in range(10):
    env = np.zeros(100, np.uint8)
    random_number = random.randint(1,99)
    random_number2 = random.randint(1,99)
    env[random_number] = 1.0
    env[random_number2] = 2.0
    env = np.reshape(env, (10, 10))
    plt.pcolor(env, cmap = 'viridis' )
    plt.savefig('test'+str(i)+'.png')
```
## Exercise
Building off of your existing model, create a simulation of age-structured growth. The simulation should have at least 4 age classes (1,2,3,4,etc.). The first class cannot reproduce, and the subsequent classes have a certain probability reproduction and mortality. The last age class has a 100% chance of mortality. Run the population growth simulation for at least 20 time steps.

Hint: With each time step, you can imagine a function that says "if the value at index [x,y] is 1, then pass. Else if 2 (elif) then you have a certain probaility of reproduction

## Solution
By Emily
```python
import agentpy as ap
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
import IPython

# define the agents

class AgeClass(ap.Agent):

    def setup(self):
        """ Initialize a new variable at agent creation. """
        self.condition = 0  
        # Empty space = 0, Age Class 1 = 1, Age Class 2 = 2, Age Class 3 = 3, Age Class 4 = 4
    
    def reproduce(self):
        """ Roll for reproduction to add a neighbor of the first age class. """
        rng = self.model.random
        
        for n in self.model.grid.neighbors(self):
            if n.condition == 0 and self.condition == 2 and self.p.reproduce_prob_2 > rng.random():
                n.condition = 1
            elif n.condition == 0 and self.condition == 3 and self.p.reproduce_prob_3 > rng.random():
                n.condition = 1  
            elif n.condition == 0 and self.condition == 4 and self.p.reproduce_prob_4 > rng.random():
                n.condition = 1
        
    def death(self):
        """ Roll for death and either die or go on to next age class. """
        rng = self.model.random
        
        if self.condition == 1 and self.p.death_prob_1 > rng.random():
            self.condition = 0   
        elif self.condition == 2 and self.p.death_prob_2 > rng.random():
            self.condition = 0
        elif self.condition == 3 and self.p.death_prob_3 > rng.random():
            self.condition = 0
        elif self.condition == 4:
            self.condition = 0
        elif self.condition == 0:
            self.condition = 0
        else:
            self.condition += 1
# define the model

class StableAgeDist(ap.Model):
    
    def setup(self):
        """ Initialize the agents and grid. """
        
        # create agents (individuals in age classes)
        self.agents = ap.AgentList(self, self.p.size**2, AgeClass)
   
        # create grid
        self.grid = ap.Grid(self, [self.p.size]*2, track_empty=False)
        self.grid.add_agents(self.agents, random=False, empty=False)
        
        # initiate population for all age classes
        # convert randomly selected agents to inital population defined in the parameters
        # Empty space = 0, Age Class 1 = 1, Age Class 2 = 2, Age Class 3 = 3, Age Class 4 = 4
        age_classes = [self.p.age_class_1,
                       self.p.age_class_2,
                       self.p.age_class_3,
                       self.p.age_class_4]
        
        for i, age in enumerate(age_classes):
            new_agents = self.agents.select(self.agents.condition == 0)
            new_agents.random(age).condition = i+1
    
    def update(self):
        """ Record number of individuals after setup and each step. """
        
        # record population in each age class at each step
        for i, c in enumerate(('Population_1', 'Population_2', 'Population_3', 'Population_4')):
            n_agents = len(self.agents.select(self.agents.condition == i+1))
            self[c] = n_agents
            self.record(c)
        
        # stop simulation for following circumstances
        if sum([self.Population_1, self.Population_2, self.Population_3, self.Population_4]) == 0:
            self.stop()
                      
    def step(self):
        """ Define the models' events per simulation step. """
        # calling functions assigned in the Agent class
        # running birth then death results in a skewed distribution graph where most agents are age class 2 instead of 1
        self.agents.death()
        self.agents.select(self.agents.condition > 1).reproduce()
        
    def end(self):
        """ Record evaluation measures at the end of the simulation. """
        n_agents = len(self.agents.select(self.agents.condition != 0))
        self.report(f"Total population:", n_agents)
# define parameters

parameters = {
    # initial populations
    'age_class_1': 200,
    'age_class_2': 0,
    'age_class_3': 0,
    'age_class_4': 0,
    # agent birth probabilities if using Agent class birth function
    'reproduce_prob_2': (2/8),
    'reproduce_prob_3': (3/8),
    'reproduce_prob_4': (1/8),
    # death probabilities
    'death_prob_1': 0.2,
    'death_prob_2': 0.5,
    'death_prob_3': 0.75,
    'size': 50,
    # simulation will likely run for inifite time steps, defining the steps parameter is important for this model
    'steps': 50
}

# Create single-run animation with custom colors and stackplot
    
def age_dist_lineplot(data, ax):
    """ Stackplot of age class % population over time. """
    x = data.index.get_level_values('t')
    y = [data[var] for var in ['Population_1', 'Population_2', 'Population_3', 'Population_4']]

    sns.set_style('ticks')
    labels=['1', '2', '3', '4']
    colors = ['red', 'orange', 'blue', 'green']
    
    for i, c in enumerate(colors):
        ax.plot(x, y[i], label=labels[i], color=c)

    ax.legend(title="Age Class", loc='upper center', bbox_to_anchor=(0.5, 1.05), ncol=4, framealpha=1, edgecolor='black')
    ax.set_xlim(0, max(1, len(x)-1))
    ax.set_ylim(0, 1000)
    ax.set_xlabel("Time steps")
    ax.set_ylabel("Population (N)")

def animation_plot(m, axs):
    ax1, ax2 = axs
    ax2.set_title(f"Total individuals: {(m.Population_1+m.Population_2+m.Population_3+m.Population_4)}\n"
                 f"Percent grid covered: {((m.Population_1+m.Population_2+m.Population_3+m.Population_4)/(parameters['size']**2)*100):.2f}%")

    # Plot stackplot on first axis
    age_dist_lineplot(m.output.variables.StableAgeDist, ax1)
    ax1.legend(title="Age Class", loc='upper center', bbox_to_anchor=(0.5, 1.05), ncol=4, framealpha=1, edgecolor='black')

    # Plot network on second axis
    attr_grid = m.grid.attr_grid('condition')
    color_dict = {0:'white', 1:'red', 2:'orange', 3:'blue', 4:'green'}
    ap.gridplot(attr_grid, ax=ax2, color_dict=color_dict, convert=True)

fig, axs = plt.subplots(1, 2, figsize=(12, 6)) # Prepare figure
model = StableAgeDist(parameters)
animation = ap.animate(StableAgeDist(parameters), fig, axs, animation_plot)
IPython.display.HTML(animation.to_jshtml(fps=2))

```
