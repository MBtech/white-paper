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

**References:**
- [Scatter plot with seaborn](https://seaborn.pydata.org/generated/seaborn.scatterplot.html)
