# Welcome to Individual-based Modeling

Individual-based models (IBMs) have been used extensively for the past 30 years to study a wide range of questions. At it's core, IBMs are used to understand how properties of the individual scale up to influence broad patterns in biology. These patterns often include population growth, species interactions, and responses to the environment.

But at the core, it's important to remember what we mean by 'model'.

A model can be defined as a representation or even abstraction of a concept or system that promotes the understanding of a real system. Thus, as George E.P. Box famously declared, "All models are wrong, but some are useful."

IBMs are a wonderful way to generate hypotheses. We must remember as modelers that the simulations we generate are not expected to exactly simulate nature - that would be impossible. The point is to boil down what we believe to be the major drivers of a system and then illustrate that the model is useful for understanding or predicting a particular system.

## How do we use IBMs?
### Extinction processes and community assembly

IBMs have been used to understand patterns of biodiversity in nature, including extinctions processes and gap formation in tropical forests (Köhler and Huth 1998). Individual properties included species-specific growth rates, crown height, diameter, and rates of photosynthesis and respiration. Flow diagrams also illustrate the various processes that are linked together. These relationships can be derived from empirical studies or theory.

<p align="center">
  <img width="300" height="450" src="https://www.science20.com/files/images/tropical_ecosystems_higher_biodiversity.jpg">
  <img width="450" height="300" src="https://ars.els-cdn.com/content/image/1-s2.0-S0304380098000660-gr2.gif">
</p>

IBMs are often based on the physical characteristics of individuals, and in this case, they used a physical model of plant height, crown features, and diameter to model shade and thus the potential for growth.

<p align="center">
  <img width="300" height="300" src="https://ars.els-cdn.com/content/image/1-s2.0-S0304380098000660-gr6.gif">
</p>

>The variation of the growth stages over time shows that the forest reaches its equilibrium after ≈200 years of simulation (Fig. 8A). Then ≈5% of the forest is in gap stage, which means that in the regarded plot, no trees larger than h≥20 m exist.
>- Köhler and Huth 1998, *The effects of tree species grouping in tropical rainforest modelling: Simulations with the individual-based model Formind*

### Life history and evolution

IBMs were most famously developed by ichthyologists. IBMs are really helpful for understanding how processes underlying the life cycle ultimately scale up to influence population-level dyanmics. These fish-based models also include aspects like environmental sensitivity to temperature, flow, available food, etc. As with the forest dynamic models, modelers often publish a flow diagram of their model to help the reader and reviewer understand how the model operates.

<p align="center">
  <img width="900" height="600" src="https://ars.els-cdn.com/content/image/1-s2.0-S0304380015003324-gr1.jpg">
</p>

And critically, individual based models can then be compared to observed variables. In this example, the output of the IBM was used to estimate variation in population dyanmics. We can see that the model generally performs very well at simulating the variation in population estimates.

<p align="center">
  <img width="900" height="600" src="https://ars.els-cdn.com/content/image/1-s2.0-S0304380015003324-gr2.jpg">
</p>

They can also be used to understand how organisms respond to environmental change, such as climate change. Evolutionary responses are difficult to measure and may take years to accomplish. But with IBMs, we can generate hypotheses on how evolution might influence responses to climate change, for instance.

>The model predicted severe declines in density and biomass under climate warming. These declines were lower than expected at range margins because of evolution towards smaller size at both emergence and maturation compared to the natural evolution under the baseline conditions. Despite stronger evolutionary responses, declining rates were substantially larger under the combined warming and flow reduction scenario, leading to a high probability of population extinction over contemporary time frames. Therefore, adaptive responses could not prevent extinction under high rates of environmental change.
>-Ayllon et al. 2026, *InSTREAM-Gen: Modelling eco-evolutionary dynamics of trout populations under anthropogenic environmental change*

### Behavior
IBMs are also helpful for designing experiments. In these simualtions, Sears et al. (2016) used IBMs to assess the influece of environmental heterogeneity (specifically temperature) on thermoregulatory performance of lizards.

> To illustrate how the spatial distribution of thermal resources affects thermoregulatory performance, we simulated movements of ectothermic animals in 2D landscapes. Simulated individuals moved through a 64- × 64-m grid, in which each cell (1 m2) had an operative temperature drawn from a bimodal distribution (Methods). Holding this statistical distribution constant, we configured the thermal landscape in various ways, comprising a set of spatial distributions ranging from clumped to dispersed (Supporting Information and Fig. S1). For each environment, we simulated behavioral thermoregulation and estimated the energetic cost of movement.
>- Sears et al. 2016, *Configuration of the thermal landscape determines thermoregulatory performance of ectotherms*

<p align="center">
  <img width="400" height="300" src="https://www.pnas.org/content/pnas/113/38/10595/F2.large.jpg?width=800&height=600&carousel=1">
</p>

Then, they designed an experiment with shade cloth in the desert to create the same environments in nature. They surgically implanted temperature loggers into the abdomen of lizards and measured thermoregualtory accuracy.

<p align="center">
  <img width="400" height="300" src="https://www.pnas.org/content/pnas/113/38/10595/F3.large.jpg?width=800&height=600&carousel=1">
</p>

What did they find? That the individual based models almost perfectly simulated how thermal heterogeneity influences thermoregulatory behavior in nature!
<p align="center">
  <img width="900" height="300" src="https://www.pnas.org/content/pnas/113/38/10595/F4.large.jpg?width=800&height=600&carousel=1">
</p>

## Why use Python?

Python is a powerful and relatively easy coding language. But that's not why we are using it. IBMs are available in software packages, such as NetLogo. But these models have limits and may not serve your purposes or question. Also, the code is often proprietary, making it impossible to adjust the code or sometimes even look at it. We are moving away from these 'black boxes', but there is enormous potential behind developing your own IBM that could serve you for the rest of your career.

## Excercise
Before we are done:
* Install Anaconda on your computer. Anaconda is the easiest way to perform Python/R data science and machine learning on a single machine. Developed for solo practitioners, it is the toolkit that equips you to work with thousands of open-source packages and libraries.
[Click here to download!](https://docs.anaconda.com/anaconda/install/index.html)

* Create a Github account! [Click here to register](https://www.github.com)



