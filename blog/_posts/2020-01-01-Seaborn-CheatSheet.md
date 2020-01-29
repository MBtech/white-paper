---
layout: post
title: "Seaborn Cheatsheet"
categories:
  - cheatsheet
tags:
  - cheatsheet
---
Boxplot seaborn:
```python
ax = sns.boxplot(x="day", y="total_bill", hue="time", data=tips, linewidth=2.5)
```

Lineplot with confidence interval:
```python
sns.relplot(x="timepoint", y="signal", kind="line", data=fmri)
```
If you want standard deviation instead of confidence interval
```python
sns.relplot(x="timepoint", y="signal", kind="line", ci="sd", data=fmri)
```

**References:**
- [Relational plot](https://seaborn.pydata.org/tutorial/relational.html#relational-tutorial)
