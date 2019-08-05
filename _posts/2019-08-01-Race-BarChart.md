---
layout: post
title: "Race Animate Bar Chart"
categories:
  - guide
tags:
  - guide
---
Well just like my other random adventures in the world of CS. This one started randomly.
I started to look into how to make those animated race bar charts that you might have seen going viral.
Something like the one below.

This post is mainly based on [this article](https://towardsdatascience.com/create-animated-bar-charts-using-r-31d09e5841da). Though since I had limited experience with R, I had to figure out what different parts of the code were doing so that I can change the code. So what follows is my own expansion and modification of the code available in that article.

## Setup
To experiment with the code below you would need to install R and I would recommend installing R Studio as well.
Then you can open RStudio and install the necessary R packages using the following command in RStudio console.
```
install.packages(c("ggplot2", "gganimate", "tidyverse", "janitor", "scales", "readxl"))
```

## Preprocessing code
First the preprocessing code. To clean up the data a little and transform it into a form that we want. You can download the xls file [here](https://drive.google.com/file/d/1ppA99SZPOlVXlda7ddOFo_DIm_Skxjeq/view?usp=sharing).
First, we import the libraries. Next we read the xls file into a dataframe. Since dataframe contains row for geographical and economic regions as well we need to select only the rows that include countries.
The next line uses the `gather()` function to collect a set of column names and places them into a single “year” column. So unfolds is unfolded. Originally the data in the format below:

| Country     	| 1980   	| 1981   	| 1982   	|
|-------------	|--------	|--------	|--------	|
| Afghanistan 	| 500.21 	| 510.31 	| 512.75 	|
| Albania     	| 728.35 	| 817.72 	| 824.73 	|

and after the gather() function it transforms into the following format:

| Country     	| year 	| value  	|
|-------------	|------	|--------	|
| Afghanistan 	| 1980 	| 500.21 	|
| Albania     	| 1980 	| 728.35 	|
| Afghanistan 	| 1981 	| 510.31 	|
| Albania     	| 1981 	| 817.72 	|
| Afghanistan 	| 1982 	| 512.75 	|
| Albania     	| 1982 	| 824.73 	|

This transformation is followed up by the `clean_names()` function from the janitor package to clean up. Resulting names are unique and consist only of the _ character, numbers, and letters. The another transformation using mutate() simply applied as.numeric() function on the year column making sure that the values are converted to numeric data. The `no data` values are converted to `NA` numerical data. Lastly, the data frame is written to a CSV file.

```r
library(tidyverse)
library(janitor)
library(readxl)

filename <- "./IMF_GDP_PC_April_2019.xls"
gdp <- read_xls(filename)
#filter only country rows
gdp <- gdp[2:194,]

gdp_tidy <- gdp %>%
  gather(year,value,2:46) %>%
  janitor::clean_names() %>%
  mutate(year = as.numeric(year))
write_csv(gdp_tidy,"./gdp_ppppc.csv")
```

## Animated Plot
In order to create an animated plot, we first need to create static plots for each year.
Here's a summary of what's happening. The csv file is read and the column name of the first column is changed to "country_name" and the values in the "value" column is transformed into double.
The next set of transformations essentially transform and filter the data to create a dataframe that contains countries with top 10 highest GDP per capita values for each year along with the rank, their relative value and the label for the bar graph.
The static plots are generated for each year. The `transition_states()` function is used to stitch individual static plots together. These static plots and the transitioning information is then combined to create a `gganim` object that we can pass to the [`animate()` function](https://www.rdocumentation.org/packages/gganimate/versions/1.0.3/topics/animate).

Lastly, the plot can be either and saved as a GIF file (using the gifski renderer) or MP4 file (using the ffmpeg_renderer).
```r
library(tidyverse)
library(gganimate)

# Data manipulation
gdp_tidy <- read_csv("./gdp_ppppc.csv")
colnames(gdp_tidy)[1] <- "country_name"
gdp_tidy <- gdp_tidy %>%
  mutate(value = as.double(value))

gdp_formatted <- gdp_tidy %>%
  group_by(year) %>%
  # The * 1 makes it possible to have non-integer ranks while sliding
  mutate(rank = rank(-value),
         Value_rel = value/value[rank==1],
         Value_lbl = paste0(" ",round(value/1000.0))) %>%
  group_by(country_name) %>%
  filter(rank <=10) %>%
  ungroup()

# Generating static plot
staticplot = ggplot(gdp_formatted, aes(rank, group = country_name,
                                       fill = as.factor(country_name), color = as.factor(country_name))) +
  geom_tile(aes(y = value/2,
                height = value,
                width = 0.9), alpha = 0.8, colour = "black") +
  geom_text(aes(y = value/2, label = paste(country_name, " ")), size=12, vjust = 0.2, hjust = 1, colour = "black") +
  geom_text(aes(y=value,label = Value_lbl, hjust=0), size=8,  colour = "black") +
  coord_flip(clip = "off", expand = FALSE) +
  scale_y_continuous(labels = scales::comma) +
  scale_x_reverse() +
  guides(color = FALSE, fill = FALSE) +
  theme(axis.line=element_blank(),
        axis.text.x=element_blank(),
        axis.text.y=element_blank(),
        axis.ticks=element_blank(),
        axis.title.x=element_blank(),
        axis.title.y=element_blank(),
        legend.position="none",
        panel.background=element_blank(),
        panel.border=element_blank(),
        panel.grid.major=element_blank(),
        panel.grid.minor=element_blank(),
        panel.grid.major.x = element_line( size=.1, color="grey" ),
        panel.grid.minor.x = element_line( size=.1, color="grey" ),
        plot.title=element_text(size=25, hjust=0.5, face="bold", colour="black"),
        plot.subtitle=element_text(size=16, hjust=0.5, face="italic", color="black"),
        plot.caption =element_text(size=18, hjust=0.5, face="italic", color="black"),
        plot.background=element_blank(),
        plot.margin = margin(2,2, 2, 4, "cm"))

# Animate
anim = staticplot + transition_states(year, transition_length = 4, state_length = 1) +
  view_follow(fixed_x = TRUE)  +
  labs(title = 'GDP per Capita : {closest_state}',  
       subtitle  =  "Top 10 Countries",
       caption  = "GDP per capita in thousands USD | Data Source: IMF Data")

# Render to a gif instead
#animate(anim, 200, fps = 20,  width = 1200, height = 1000,
#        renderer = gifski_renderer("gganim.gif"))

# For MP4
animate(anim, fps=20, duration=60,
        width = 1920, height = 1080,
        renderer = ffmpeg_renderer()) -> for_mp4
anim_save("animation_gdp_ppp.mp4", animation = for_mp4 )
```
Here's the end result:
<img src="https://i.imgur.com/RMVnPaY.gif"/>

### References:
1. On tidying data: https://garrettgman.github.io/tidying/
2. Clean names function: https://rdrr.io/cran/janitor/man/clean_names.html
