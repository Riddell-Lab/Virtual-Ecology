# Plotting
There are several ways to plot in Python but we are going to focus on one of the more common methods that uses the module called matplotlib. Matplotlib is a cross-platform, data visualization and graphical plotting library for Python and its numerical extension NumPy. As such, it offers a viable open source alternative to MATLAB.

matplotlib.pyplot is a collection of functions that make matplotlib work like MATLAB. Each pyplot function makes some change to a figure: e.g., creates a figure, creates a plotting area in a figure, plots some lines in a plotting area, decorates the plot with labels, etc.

Let's go over an example.

```python
import matplotlib.pyplot as plt
plt.plot([1, 2, 3, 4])
plt.ylabel('some numbers')
plt.show()
```
What did you learn? Did matplotlib make an assumption?

```python
plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
```

## Formatting the style of your plot
For every x, y pair of arguments, there is an optional third argument which is the format string that indicates the color and line type of the plot. The letters and symbols of the format string are from MATLAB, and you concatenate a color string with a line style string. The default format string is 'b-', which is a solid blue line. For example, to plot the above with red circles, you would issue

```python
plt.plot([1, 2, 3, 4], [1, 4, 9, 16], 'ro')
plt.axis([0, 6, 0, 20])
plt.show()
```

See the plot documentation (```help(plt.plot)```) for a complete list of line styles and format strings. The axis function in the example above takes a list of [xmin, xmax, ymin, ymax] and specifies the viewport of the axes.


## NumPy!
If matplotlib were limited to working with lists, it would be fairly useless for numeric processing. Generally, you will use numpy arrays. In fact, all sequences are converted to numpy arrays internally. The example below illustrates plotting several lines with different format styles in one function call using arrays.

```python
import numpy as np

# evenly sampled time at 200ms intervals
t = np.arange(0., 5., 0.2)

# red dashes, blue squares and green triangles
plt.plot(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')
plt.show()
```

So what is NumPy?

NumPy is the de-facto Python library for N-dimensional arrays manipulation and computational computing. It is open-source, easy to use, memory friendly, and lightning-fast. 

Originally known as ‘Numeric,’ NumPy sets the framework for many data science libraries like SciPy, Pandas, and more.

While Python lists store a collection of ordered, alterable data objects, NumPy arrays only store a single type of object. So, we can say that NumPy arrays live under the lists’ umbrella. Therefore, there is nothing NumPy arrays do lists do not. Want to learn how much faster numpy arrays are than lists? Check out this program.

```python
"""General comparison between NumPy and lists"""
import numpy as np
from time import time
#Random numpy array
numpy_array = np.random.rand(1000000)
list_conv = list(numpy_array)
#Start timing NumPy compuation
start1 = time()
#Compute the mean using NumPy
numpy_mean = np.mean(numpy_array)
print(f"Computing the mean using NumPy: {numpy_mean}")
#End timing
end1 = time()
#Time taken
time1 = end1 - start1
print(f"Computation time: {time1}")
#Start timing list computation
start2 = time()
#Compute the mean using lists
list_mean = np.mean(list_conv)
print(f"Computing the mean using lists: {list_mean}")
#End timing
end2 = time()
#Time taken
time2 = end2 - start2
print(f"Computation time: {time2}")
#Check results are equal
print('Arrays were '+ str(time2/time1) + 'times faster than lists.')
```

However, when it comes to NumPy as a whole, it covers not only arrays manipulation but also many other routines such as binary operations, linear algebra, mathematical functions, and more. I believe it covers more than one can possibly need.

There are many functions in Numpy that we will use as we begin to make our IBMs. We have already introduced the numpy arange() function which works much like the range function, BUT you can use floats!

## Exercise
Take a minute to explore arange() and how it works! Try float values for start, stop, and step.

## Plotting categorical data and multiple panels

Plotting multiple panels is also pretty easy! The subplot command helps you to plot multiple sublpots in one figure. plt.subplot takes three arguments, the number of rows (nrows), the number of columns (ncols) and the plot number. Using the 3-digit code is a convenience function provided for when nrows, ncols and plot_number are all <10.

```python
names = ['group_a', 'group_b', 'group_c']
values = [1, 10, 100]

plt.figure(figsize=(9, 3))

plt.subplot(131)
plt.bar(names, values)
plt.subplot(132)
plt.scatter(names, values)
plt.subplot(133)
plt.plot(names, values)
plt.suptitle('Categorical Plotting')
plt.show()
```
For more than ten subplots, you will need to use the subplot2grid() function.
```python
import matplotlib.pyplot as plt

plt.figure(0)
for i in range(5):
    for j in range(4):
        plt.subplot2grid((5,4), (i,j))
plt.show()
```

If using a dataframe, we can also plot using strings.
```python
data = {'a': np.arange(50),
        'c': np.random.randint(0, 50, 50),
        'd': np.random.randn(50)}
data['b'] = data['a'] + 10 * np.random.randn(50)
data['d'] = np.abs(data['d']) * 100

plt.scatter('a', 'b', c='c', s='d', data=data)
plt.xlabel('entry a')
plt.ylabel('entry b')
plt.show()
```

In this case, the c and s in the scatter plot refer to color and shape, respectively. There are many possible colors to use.
c = array-like or list of colors or color, optional. The marker colors. Possible values:
- A scalar or sequence of n numbers to be mapped to colors using cmap and norm.
- A 2D array in which the rows are RGB or RGBA.
- A sequence of colors of length n.
- A single color format string.

Refer to the help on scatter to read more.

## Line formatting

```python
import numpy as np

x = np.arange(0,5,1)
y = np.arange(0,100,20)
plt.plot(x, y, linewidth=4.0)
```

We can also easily format lines in multiple subplots.

```python
def f(t):
    return np.exp(-t) * np.cos(2*np.pi*t)

t1 = np.arange(0.0, 5.0, 0.1)
t2 = np.arange(0.0, 5.0, 0.02)

plt.figure()
plt.subplot(211)
plt.plot(t1, f(t1), 'bo', t2, f(t2), 'k')

plt.subplot(212)
plt.plot(t2, np.cos(2*np.pi*t2), 'r--')
plt.show()
```

## Adding text
We can also easily add text to a figure.

```python
mu, sigma = 100, 15
x = mu + sigma * np.random.randn(10000)

# the histogram of the data
n, bins, patches = plt.hist(x, 50, density=1, facecolor='g', alpha=0.75)

plt.xlabel('Smarts')
plt.ylabel('Probability')
plt.title('Histogram of IQ')
plt.text(60, .025, r'$\mu=100,\ \sigma=15$')
plt.axis([40, 160, 0, 0.03])
plt.grid(True)
plt.show()
```

## Exercise
Use your animal DataFrame to create two plots. One that shows the relationship between body mass and metabolic rate for each scaling coefficient on a single graph, and another plot that shows each relationship on a subplot.
