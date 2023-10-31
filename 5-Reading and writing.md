# Reading and writing files
Today we are going to learn two ways of reading and writing files. The first is a standard way and the second uses the module called Pandas.

First let's review some basics. When we navigate through our computer, we often click on folders within folders. Typically, we refer to these as directories, not folders. And we can navigate around directories within Python in much the same way. First let's find our current working directory.

```python
from pathlib import Path
import os
Path.cwd()
```
First let's try making a new directory.

```python
import os
os.makedirs('path-name-here')
```
We can also then change our working directory using:
```python
os.chdir('new-path-here-as-string')
Path.cwd()
```
If the new path does not exist, you will get an error message.

All users have a folder for their own files called the home directory or home folder. You can get the home folder by calling

```python
Path.home()
```
On windows, it will be under C:\Users and on Mac it will be under /Users. Notice that Mac and Windows use different slashes for directories.

When reading and writing paths, we can use absolute paths (which start with the root folder (which contains all folders) or we can use relative paths (which is relative to the directory you are currently working in).

## Exercise
Try making a new directory using absolute and relative file paths. Use your working directory to figure out where to put your new folder.

Here are some more helpful functions.

```python
os.path.getsize('your-path-here')
os.listdir()
```
What do these functions do?

## The glob method
If you want to work on specific files, the glob method is simple.
```python
p = Path('your-path-here-to-desktop')
list(p.glob('*'))
```
Or if we want to return a certain only files with a certain extension:
```python
list(p.glob('*.pdf')
```
We can also use the '?' in helpful ways.
```python
list(p.glob('test?.txt')
```
## The reading and writing process
Let's try writing a very simple text document.
```python
from pathlib import Path
p = Path('spam.txt')
p.write_text('Hello, world!')
p.read_text()
```
Now let's get into a more common way of reading and writing files.
```python
hellofile = open(Path.home()/'spam.txt')
hellofile.read()
```
Now let's open a csv file and write to it.
```python
baconfile = open('bacon.csv','w')
baconfile.write('bacon\neggs\ntoast')
baconfile.close()
```
Now go check out your csv file!

Let's add some more to it (without replacing the text!).

```python
baconfile = open('bacon.csv','a')
baconfile.write('bacon,ham\neggs,chicken\ntoast,bread')
baconfile.close()
```
## Building a dataframe using Pandas
Pandas is a module that provides a really effective way to import, export, read, and manipulate data. We can't cover everything today, but I enourage people to read more about Pandas.

Now, let's create a dataframe.

```python
import pandas as pd

a = ['Anna','James','Grey', 'Reid','Whit']
b = [5,4,8,2,1]
c = [44,52,60,38,29]

df = pd.DataFrame(
    {'Name': a,
     'Age': b,
     'Height': c
    })
```
Or as an alternative:
```python
import pandas as pd

a = ['Anna','James','Grey', 'Reid','Whit']
b = [5,4,8,2,1]
c = [44,52,60,38,29]

pd.DataFrame(list(zip(a, b, c)), columns=['Name','Age', 'Height'])
```
where zip() is a powerful function. To see what it's doing enter this:
```python
print(tuple(zip(a,b)))
```
Now let's write a csv file:
```python
df.to_csv('data.csv')
```
That's it! Note that without a path, the file will be written to the working directory.

Now let's read a csv file.
```python
import pandas as pd
df = pd.read_csv('data.csv')
```

## Indexing
For indexing, python has a few options. First we can return an enture column if we want:

```python
df['Age']
df['Age'][0]
```
Notice that we cannot simply use indexing alone. This will result in a new error:

```python
df[0]
```
A Python KeyError occurs when you attempt to access an item in a dictionary that does not exist using the indexing syntax. This error is raised because Python cannot return a value for an item that does not exist in a dictionary

Here are some other options as well.
.loc is primarily label based, but may also be used with a boolean array. .loc will raise KeyError when the items are not found. These indices are typically used with specific labels, but only if we index the dataframe.
```python
df = pd.DataFrame(list(zip(b, c)),index = a, columns=['Age', 'Height'])
#Then we can search for information specific to Anna if indexed
df.loc['Anna']
```
.iloc is primarily integer position based (from 0 to length-1 of the axis). .iloc will raise IndexError if a requested indexer is out-of-bounds.
```python
df = pd.DataFrame(list(zip(b, c)),index = a, columns=['Age', 'Height'])
#Then we can search for information specific to Anna if indexed
df.iloc[0]
df.iloc[0][0]
```
## The many powerful functions in pandas
- describe() is used to generate descriptive statistics of the data in a Pandas DataFrame or Series. It summarizes central tendency and dispersion of the dataset. describe() helps in getting a quick overview of the dataset.
- drop_duplicates() returns a Pandas DataFrame with duplicate rows removed. Even among duplicates, there is an option to keep the first occurrence (record) of the duplicate or the last. You can also specify the inplace and ignore_index attribute.
- value_counts() returns a Pandas Series containing the counts of unique values. Consider a dataset that contains customer information about 5,000 customers of a company. value_counts() will help us in identifying the number of occurrences of each unique value in a Series. It can be applied to columns containing data like State, Industry of employment, or age of customers.
- groupby() is used to group a Pandas DataFrame by 1 or more columns, and perform some mathematical operation on it. groupby() can be used to summarize data in a simple manner.
- merge() is used to merge 2 Pandas DataFrame objects or a DataFrame and a Series object on a common column (field). If you are familiar with the concept of JOIN in SQL, merge function similar to that. It returns the merged DataFrame.
- sort_values() is used to sort column in a Pandas DataFrame (or a Pandas Series) by values in ascending or descending order. By specifying the inplace attribute as True, you can make a change directly in the original DataFrame.
- fillna() helps to replace all NaN values in a DataFrame or Series by imputing these missing values with more appropriate values.

Now, I'd like to briefly introduce dictionaries because they can look somewhat similar to pandas dataframes.
```python
import pandas as pd
myCat = {'size':'fat','color':'gray'}
myCat['size']
```
Dictionaries is a mutable collection of many values, but the indices can essentailly use anything! Like 'size' in this case. These are called *keys* and are linked together in *key-value pairs*. Unlike lists, dictionaries are not ordered, so they can't be sliced or indexed as we would with lists.

They can also be used to create dataframes, but they require an index.
```python
df = pd.DataFrame(myCat,index = ['Harold'])
```

## Exercise
Write a function that reads the animal.csv file into a pandas dataframe. Then in the same function, assign a particular mass to each animal based on the number of letters in the name of the animal. Then integrate the new column into the dataframe and export the csv file onto your computer.

## Solution
Making a function that generates mass from Pandas DataFrame.
```python
import pandas as pd

def make_mass(path):
    df = pd.read_csv(path)
    mass = []
    for i in range(len(df['animals'])):
        mass.append((len(df.iloc[i][0]))**1.8)
    df['mass'] = mass
    return df
df = make_mass('/Users/eriddell/Desktop/animals.csv')
```

## Pandas practice!

Let's get some more practice with using Pandas, particularly with indexing and creating new dataframes.

Let's review a bit more of indexing and working with dataframes.

```python
import pandas as pd

a = ['Anna','James','Grey', 'Reid','Whit']
b = [5,4,8,2,1]
c = [44,52,60,38,29]

df = pd.DataFrame(list(zip(b, c)),index = a, columns=['Age', 'Height'])

# There are several ways to index this DataFrame
# First, by column name
df['Age'] #returns whole column

#Second, by row name if indexed
df.loc['Anna'] # returns entire row
df.loc['Anna'][0] # returns the first column value for row indexed as Anna
df.loc['Anna','Age'] # returns Anna's age
df.loc['Anna':'Grey','Age':'Height'] #slices the dataframe
df.loc['Grey':'Anna','Age':'Height'] # returns nothing

#Third, by using .loc if not indexed
df = pd.DataFrame(list(zip(a,b, c)), columns=['Name','Age', 'Height'])
df.loc[0,'Name']

#Fourth, by using .iloc
df.iloc[0] #returns first row
df.iloc[0][1] # returns value in the first row and second column

#Fifth, us using iloc and slices
df.iloc[0:2] # returns first two rows with all columns
df.iloc[0:2,0:1] # returns first two rows with only the first column
df.iloc[1:,0:] # returns entire dataset without first row but with all columns

#Sixth, additional functionality
df.loc[:,"Age"].mean() # return the average age for all rows
df.loc[0:2,"Age"].mean() # return the average age for the first two rows but only if not indexed!
```


## Exercise
Create a new column for your Pandas DataFrame that uses the column to calculate metabolic rate for each species. Metabolic rate typically has an exponential relationship with body mass. If we assume that your previous function provides body mass in kilograms, use the function below to estimate the metabolic rate of each animal.

M = a + W<sup>b</sup>

When conducting the calculation, assume a = 6.17. The exponent b is known to vary. Thus, create three different columns that estimate metabolic rate using b = 0.6, 0.75, and 0.9. Thus the DataFrame will have five columns, the name, the mass, and three estimates for metabolic rate.

## Solution

```python
import pandas as pd

def make_mass(path):
    df = pd.read_csv(path)
    mass = []
    met1 = []
    met2 = []
    met3 = []
    for i in range(len(df['animals'])):
        mass.append((len(df.iloc[i][0]))**1.8)
    df['mass'] = mass
    for i in range(len(df['animals'])):
        met1.append(6.17*df['mass'][i]**0.6)
        met2.append(6.17*df['mass'][i]**0.75)
        met3.append(6.17*df['mass'][i]**0.9)
    df['met_6'] = met1
    df['met_7'] = met2
    df['met_9'] = met3
    return df
df = make_mass('/Users/eriddell/Desktop/animals.csv')
```
