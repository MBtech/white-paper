---
layout: post
title: "PBS and Interactive Graphs"
categories:
  - guide
  - csblog
tags:
  - guide
---


Generally I don't pay a whole lot of attention to current affairs. But today I saw the exchange rate
between the PKR and USD and I was taken by surprise at how much value of PKR had fallen.

So, I started to look at statistical measures that are used are indicator for economy. One thing lead to another
and I ended up writing a python program to automatically extra data out of the PDF reports that are made available
by the Bureau of Statistics in Pakistan. Compared to the usual static graphs that I generally do, I decided to work
with interactive graphing library. One such library is Bokeh and that is what I experimented with today.

[Bokeh](http://bokeh.pydata.org/en/latest/) can create nice interactive visualizations for modern web browsers. There are some [other options](https://mode.com/blog/python-interactive-plot-libraries) available which I might give a try soon enough.

### Adding Hover information
Here is a small code snippet for Bokeh to add tooltip to the graph to display the information about the data points when you hover over them.

```python
from bokeh.io import show
from bokeh.models import ColumnDataSource
from bokeh.plotting import figure
import pandas as pd

df = pd.DataFrame({'x': [1, 2, 3], 'A_y' : [1, 5, 3], 'A': [0.2, 0.1, 0.2],
                  'B_y' : [2, 4, 3], 'B':[0.1, 0.3, 0.2]})

tools_to_show = 'box_zoom,save,hover,reset'
p = figure(plot_height =300, plot_width = 1200,
           toolbar_location='above', tools=tools_to_show,

           # "easy" tooltips in Bokeh 0.13.0 or newer
           tooltips=[("Name","$name"), ("Aux", "@$name")])

columns = ['A', 'B']
source = ColumnDataSource(df)
for col in columns:

    # have to use different colnames for y-coords so tooltip can refer to @$name
    p.line('x', col + "_y", source=source, name=col)

show(p)
```
This snippet has been taken from [this](https://stackoverflow.com/questions/51271775/pandas-bokeh-how-get-dataframe-column-name-for-hover-tool) stackoverflow answer.

### Color Palettes

```python
from bokeh.palettes import *
colors = viridis(5)
```
More information about the available palettes can be found [here](https://bokeh.pydata.org/en/latest/docs/reference/palettes.html#large-palettes).

### Checkboxes
Based on [this answer](https://stackoverflow.com/questions/39522642/bokeh-check-checkboxes-with-a-button-checkbox-callback) on stackoverflow.

```python
checkbox = CheckboxGroup(labels=items, active=range(len(lines)))

iterable = [('p'+str(i),lines[i]) for i in range(len(lines))]+[('checkbox',checkbox)]

checkbox_code = ''.join(['p'+str(i)+'.visible = checkbox.active.includes('+str(i)+');' for i in range(len(lines))])
checkbox.callback = CustomJS(args={key: value for key,value in iterable}, code=checkbox_code)

clear_button = Button(label='Clear all',width=100)
clear_button_code = "checkbox.active=[];"+checkbox_code
clear_button.callback = CustomJS(args={key: value for key,value in iterable}, code=clear_button_code)

check_button = Button(label='Check all',width=100)
check_button_code = "checkbox.active="+str(range(len(lines)))+";"+checkbox_code
check_button.callback = CustomJS(args={key: value for key,value in iterable}, code=check_button_code)
```
Reversing the order of the dataframe columns.
`df.iloc[:, ::-1]`

Swapping the cols with rows using transpose
`df.T`

Dataframe from dict
`pd.DataFrame.from_dict(data)`

**References:**
