---
layout: post
title: "Creating plots in Python"
categories:
  - cheatsheet
  - csblog
tags:
  - cheatsheet
---

For this cookbook or cheatsheet I will be mainly using matplotlib and seaborn.


### Setting a only one limit on the x or y axis
For x-axis you can use the variable left or right. For y-axis you can use bottom or high. 
```python
plt.xlim(left=0)
plt.ylim(bottom=0)
```

### Scatter plot:
```python
ax = sns.scatterplot(x="total_bill", y="tip", hue="time", data=tips)
# Use the style to change the markers based on the data 
ax = sns.scatterplot(x="total_bill", y="tip", hue="time", style="time", data=tips)
# Use size to vary the size of marker based on data
ax = sns.scatterplot(x="total_bill", y="tip", hue="time", style="time", size="size", data=tips)
```

### Plot with multiple scatter plots:
```python
g = sns.relplot(x="total_bill", y="tip",
                 col="time", hue="day", style="day",
                 kind="scatter", data=tips)
```
This will generate two plots based on the category of in the time column

### Boxplot seaborn:
```python
ax = sns.boxplot(x="day", y="total_bill", hue="time", data=tips, linewidth=2.5)
```

### Lineplot with confidence interval:
```python
sns.relplot(x="timepoint", y="signal", kind="line", data=fmri)
```
If you want standard deviation instead of confidence interval
```python
sns.relplot(x="timepoint", y="signal", kind="line", ci="sd", data=fmri)
```

### Changing a specific ticklabel:
```python
ax = sns.scatterplot(x="x", y="y", hue="z", data=tips)
labels = [item.get_text() for item in ax.get_xticklabels()]
labels[1] = 'Testing'
ax.set_xticklabels(labels)
plt.show()
```
Changing the second label to 'Testing'. Well this is easily extended if you want to customize all labels


### Enabling ticks
```python
ax.tick_params(axis='x', which='minor', bottom=True)
```

### Color bar tick labelling:
```python
fig, ax = plt.subplots()

data = np.clip(randn(250, 250), -1, 1)
cax = ax.imshow(data, interpolation='nearest', cmap=cm.coolwarm)

# Add colorbar, make sure to specify tick locations to match desired ticklabels
cbar = fig.colorbar(cax, ticks=[-1, 0, 1])
cbar.ax.set_yticklabels(['< -1', '0', '> 1']) 
```

### Change tick size:
```python
ax.tick_params(axis='both', which='major', labelsize=10)
```

**References:**
- [Scatter plot with seaborn](https://seaborn.pydata.org/generated/seaborn.scatterplot.html)
- [Colorbar tick labelling Demo](https://matplotlib.org/3.1.1/gallery/ticks_and_spines/colorbar_tick_labelling_demo.html)
