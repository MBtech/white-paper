---
layout: post
title: "Pandas Cheatsheet"
categories:
  - guide
tags:
  - guide
---

### Length of a DataFrame:
Fastest way to do so is with `len(df.index)`

Dataframe column format from string to datetime
df['Mycol'] =  pd.to_datetime(df['Mycol'])

### Using groupby to calculate aggregate stats:
```python
stats = df.groupby(['Class'])['Force'].agg(['mean', 'count', 'std'])
```
This will group the rows by `Class` and then calculate stats on the `Force` column

### Filtering dataframe using a match in an array of values:
```python
steps = [6, 18, 30]
df = df[df['Budget'].isin(steps)]
```

### Creating a DataFrame:
```python
# Using list of lists
data = [['tom', 10], ['nick', 15], ['juli', 14]]
df = pd.DataFrame(data, columns = ['Name', 'Age'])

# Using dictionary
data = {'Name':['Tom', 'nick', 'krish', 'jack'], 'Age':[20, 21, 19, 18]}
df = pd.DataFrame(data)

# Using array of dictionary
data = [{'a': 1, 'b': 2, 'c':3}, {'a':10, 'b': 20, 'c': 30}] 
df = pd.DataFrame(data)
```

### Indexing Dataframe:
```python
students = [ ('jack', 34, 'Sydeny') ,
('Riti', 30, 'Delhi' ) ,
('Aadi', 16, 'New York') ]
# Create a DataFrame object
df = pd.DataFrame(students, columns = ['Name' , 'Age', 'City'], index=['a', 'b', 'c'])

# Select column and rows by name using .loc
# Select all rows of Column 'Age'
df.loc[:, ['Name', 'Age']]

# Select a row based on index name
df.loc['b', :]

# Select column and rows using indices using .iloc
# Following two commands will produce the same output as the one with .loc
df.iloc[:, [0, 2]]
df.iloc[1, :]
# To select a range
df.iloc[0:2, :]
```

### Dataframe Operations
Append a row to dataframe
```python
df = df.append(df2)
```
You can ignore the indexes from the df2 dataframe using `ignore_index=True`

Convert a string column to upper case
```python
df['Name'] = df['Name'].str.upper()
```

Group and sum:
```python
df.groupby(['Fruit','Name']).sum()
```
This will group the dataframe based on 'Fruit' and 'Name' columns and values in rest of the columns along the row

Renaming the column names:
```python
df.rename(columns={'Name': 'Full Name'})
```

Set and reset index:
```python
df.set_index('month')
df.reset_index()
```

Drop Column:
```python
df.drop('Name')
```

Generate rannk based on results:
```python
df['Rank'] = df.groupby(['Year'])['Return'].rank()
```
Group the dataframe based on column `Year` and create ranks based on `Return`

Reversing the order of the dataframe columns:
```python
df.iloc[:, ::-1]
```

Swapping the cols with rows using transpose:
```python
df.T
```

### Pandas GroupBy
Iterating over the subgroups 
```python
by_state = df.groupby("state") 
for group, data in by_state:
  print(group)
  print(data)
```
**References:**
- [Pandas Groupby](https://realpython.com/pandas-groupby/)