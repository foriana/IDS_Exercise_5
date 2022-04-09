---
title: 'Weekly Exercises #5'
author: "Felicia Peterson"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(gifski)        # need the library for creating gifs but don't need to load each time
library(shiny)         # for creating interactive apps
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
data("garden_harvest")

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels and alt text.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.
  

```r
ratings <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-01-25/ratings.csv')
details <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-01-25/details.csv')

theme_set(theme_minimal())
```
  
  

```r
boardgame_haven <- ratings %>% 
  left_join(details,
            by = c("id" = "id"))

single_parent_only_child_household_games_lol <- boardgame_haven %>% 
  filter(maxplayers == 2) %>% 
  arrange(rank) %>% 
  slice_head(n = 10) %>% 
  ggplot() +
  geom_col(aes(x = users_rated,
               y = fct_reorder(name, users_rated, .desc = FALSE),
               fill = name,
               text = name)) + 
  labs(title = "Top 10 Games for Two-Players",
       subtitle = "This graph shows the number of users who rated each game.",
       x = "",
       y = "") + 
  theme(legend.position = "none",
        axis.text.x = element_blank(),
        axis.ticks.x = element_blank(),
        axis.text.y = element_blank(),
        axis.ticks.y = element_blank())
```


```r
data(penguins)
penguins <- penguins %>% 
  mutate(AngryBird = mean(bill_length_mm, na.rm = TRUE))

waddles <- penguins %>% 
  ggplot(aes(x=bill_length_mm, text = AngryBird))+
  geom_histogram(binwidth= 2, fill="purple4") + 
  geom_vline(xintercept=mean(penguins$bill_length_mm, na.rm = TRUE), linetype = "dashed", color = "red") + #na.rm=TRUE to remove the na lines
  labs(title="These Bills Do Not Pay But They're Great Anyway",
             x = "Bill Length (mm)",
       y = "Number of Penguins")
```



```r
ggplotly(single_parent_only_child_household_games_lol,
         tooltip = c("text", "x"))
```

```{=html}
<div id="htmlwidget-8f2718db5a292334a9b2" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-8f2718db5a292334a9b2">{"x":{"data":[{"orientation":"v","width":69472,"base":9.55,"x":[34736],"y":[0.899999999999999],"text":"users_rated: 69472<br />7 Wonders Duel","type":"bar","textposition":"none","marker":{"autocolorscale":false,"color":"rgba(248,118,109,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"7 Wonders Duel","legendgroup":"7 Wonders Duel","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":27686,"base":4.55,"x":[13843],"y":[0.9],"text":"users_rated: 27686<br />Android: Netrunner","type":"bar","textposition":"none","marker":{"autocolorscale":false,"color":"rgba(216,144,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Android: Netrunner","legendgroup":"Android: Netrunner","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":33046,"base":5.55,"x":[16523],"y":[0.9],"text":"users_rated: 33046<br />Arkham Horror: The Card Game","type":"bar","textposition":"none","marker":{"autocolorscale":false,"color":"rgba(163,165,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Arkham Horror: The Card Game","legendgroup":"Arkham Horror: The Card Game","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":9033,"base":1.55,"x":[4516.5],"y":[0.9],"text":"users_rated:  9033<br />Fields of Arle","type":"bar","textposition":"none","marker":{"autocolorscale":false,"color":"rgba(57,182,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Fields of Arle","legendgroup":"Fields of Arle","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":52190,"base":8.55,"x":[26095],"y":[0.899999999999999],"text":"users_rated: 52190<br />Patchwork","type":"bar","textposition":"none","marker":{"autocolorscale":false,"color":"rgba(0,191,125,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Patchwork","legendgroup":"Patchwork","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":38561,"base":6.55,"x":[19280.5],"y":[0.9],"text":"users_rated: 38561<br />Star Realms","type":"bar","textposition":"none","marker":{"autocolorscale":false,"color":"rgba(0,191,196,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Star Realms","legendgroup":"Star Realms","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":23716,"base":3.55,"x":[11858],"y":[0.9],"text":"users_rated: 23716<br />Star Wars: X-Wing Miniatures Game","type":"bar","textposition":"none","marker":{"autocolorscale":false,"color":"rgba(0,176,246,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Star Wars: X-Wing Miniatures Game","legendgroup":"Star Wars: X-Wing Miniatures Game","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":16975,"base":2.55,"x":[8487.5],"y":[0.9],"text":"users_rated: 16975<br />Targi","type":"bar","textposition":"none","marker":{"autocolorscale":false,"color":"rgba(149,144,255,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Targi","legendgroup":"Targi","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":43052,"base":7.55,"x":[21526],"y":[0.899999999999999],"text":"users_rated: 43052<br />Twilight Struggle","type":"bar","textposition":"none","marker":{"autocolorscale":false,"color":"rgba(231,107,243,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Twilight Struggle","legendgroup":"Twilight Struggle","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":6904,"base":0.55,"x":[3452],"y":[0.9],"text":"users_rated:  6904<br />Watergate","type":"bar","textposition":"none","marker":{"autocolorscale":false,"color":"rgba(255,98,188,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Watergate","legendgroup":"Watergate","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":13.8812785388128,"l":10.958904109589},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Top 10 Games for Two-Players","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-3473.6,72945.6],"tickmode":"array","ticktext":["0","20000","40000","60000"],"tickvals":[4.54747350886464e-13,20000,40000,60000],"categoryorder":"array","categoryarray":["0","20000","40000","60000"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":false,"tickfont":{"color":null,"family":null,"size":0},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,10.6],"tickmode":"array","ticktext":["Watergate","Fields of Arle","Targi","Star Wars: X-Wing Miniatures Game","Android: Netrunner","Arkham Horror: The Card Game","Star Realms","Twilight Struggle","Patchwork","7 Wonders Duel"],"tickvals":[1,2,3,4,5,6,7,8,9,10],"categoryorder":"array","categoryarray":["Watergate","Fields of Arle","Targi","Star Wars: X-Wing Miniatures Game","Android: Netrunner","Arkham Horror: The Card Game","Star Realms","Twilight Struggle","Patchwork","7 Wonders Duel"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":false,"tickfont":{"color":null,"family":null,"size":0},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"54d85f0f3873":{"x":{},"y":{},"fill":{},"text":{},"type":"bar"}},"cur_data":"54d85f0f3873","visdat":{"54d85f0f3873":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```
 

```r
ggplotly(waddles,
         tooltip = c("text", "x"))
```

```{=html}
<div id="htmlwidget-2cb059e8fc7f60c6316c" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-2cb059e8fc7f60c6316c">{"x":{"data":[{"orientation":"v","width":[2,2,2,2,2,2,2,2,2,2,2,2,2,2,2],"base":[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],"x":[32,34,36,38,40,42,44,46,48,50,52,54,56,58,60],"y":[1,10,31,40,38,34,23,58,31,47,20,4,3,1,1],"text":["bill_length_mm: 32<br />43.92193","bill_length_mm: 34<br />43.92193","bill_length_mm: 36<br />43.92193","bill_length_mm: 38<br />43.92193","bill_length_mm: 40<br />43.92193","bill_length_mm: 42<br />43.92193","bill_length_mm: 44<br />43.92193","bill_length_mm: 46<br />43.92193","bill_length_mm: 48<br />43.92193","bill_length_mm: 50<br />43.92193","bill_length_mm: 52<br />43.92193","bill_length_mm: 54<br />43.92193","bill_length_mm: 56<br />43.92193","bill_length_mm: 58<br />43.92193","bill_length_mm: 60<br />43.92193"],"type":"bar","textposition":"none","marker":{"autocolorscale":false,"color":"rgba(85,26,139,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[43.9219298245614,43.9219298245614],"y":[-2.9,60.9],"text":"","type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(255,0,0,1)","dash":"dash"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":37.2602739726027},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"These Bills Do Not Pay But They're Great Anyway","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[29.5,62.5],"tickmode":"array","ticktext":["30","40","50","60"],"tickvals":[30,40,50,60],"categoryorder":"array","categoryarray":["30","40","50","60"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Bill Length (mm)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-2.9,60.9],"tickmode":"array","ticktext":["0","20","40","60"],"tickvals":[0,20,40,60],"categoryorder":"array","categoryarray":["0","20","40","60"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Number of Penguins","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"54d8ce74c64":{"x":{},"text":{},"type":"bar"},"54d85d204a7a":{"xintercept":{}}},"cur_data":"54d8ce74c64","visdat":{"54d8ce74c64":["function (y) ","x"],"54d85d204a7a":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```
 
  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
small_trains %>% 
  filter(service == "International") %>% 
  ggplot(aes(x = delayed_number, 
             y = delay_cause,
             group = year)) +
  geom_jitter() + 
  labs(title = "International Train Service in France and Causes for Delay",
       subtitle = "Year: {frame_time}",
       y = "",
       x = "Percent of trains delayed") +
  transition_time(year)
```

![](05_exercises_new_files/figure-html/unnamed-chunk-6-1.gif)<!-- -->

```r
anim_save("Q2frenchtrains.gif")
```


```r
knitr::include_graphics("Q2frenchtrains.gif")
```

<img src="Q2frenchtrains.gif" title="This gif shows the percent of different types of delays international train service in France face over the years. Overall, traffic management seems to be a consistently high cause of trian delays." alt="This gif shows the percent of different types of delays international train service in France face over the years. Overall, traffic management seems to be a consistently high cause of trian delays."  />

## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each variety and arranged (HINT: `fct_reorder()`) from most to least harvested weights (most on the bottom).  
  * Add animation to reveal the plot over date. 

I have started the code for you below. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0.


```r
daily_tomato <- garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  ungroup() %>% 
  complete(variety, 
           date, 
           fill = list(daily_harvest_lb = 0)) %>% 
  mutate(variety = fct_reorder(variety, daily_harvest_lb, sum)) %>%
  group_by(variety) %>% 
  mutate(cumsum_tomato = cumsum(daily_harvest_lb))  #1. which variable do you want to reorder, 2. by which variable, function you want to use?
```


```r
daily_tomato %>% 
ggplot(aes(x = date, y = cumsum_tomato, fill = variety)) + 
  geom_area() +
  geom_text(aes(label = variety), position = "stack") +
  labs(title = "Tomato Time: Cumulative Harvest in Pounds Over Time",
       subtitle = "Date: {frame_along}",
       x = "",
       y = "") +
  theme(legend.position = "none") +
  transition_reveal(date) 
```

![](05_exercises_new_files/figure-html/unnamed-chunk-9-1.gif)<!-- -->

```r
anim_save("Q3tomatoharvest.gif")
  

#transition date only shows what happens on a specific date; transition reveal shows progression
```

```r
knitr::include_graphics("Q3tomatoharvest.gif")
```

![](Q3tomatoharvest.gif)<!-- -->
## Maps, animation, and movement!

  4. Map Lisa's `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.
  

```r
mallorca_map <- get_stamenmap(
  bbox = c( left = 2.1629, bottom = 39.2248, right = 3.6598, top = 40.0013),
  maptype = "terrain",
  zoom = 11)

ggmap(mallorca_map) +
  geom_path(data = mallorca_bike_day7,
            aes(x = lon,
                y = lat, 
                color = ele),
            size = 1) +
  geom_point(data = mallorca_bike_day7,
             aes(x = lon, 
                 y = lat),
             size = 2,
             color = "red") +
  scale_color_viridis_c(option = "magma") +
  theme_map() +
  labs(title = "Lisa's Mallorca Bike Ride: More Elevated Than Ever Before", 
       subtitle =  "Date: {frame_along}",
       x = "",
       y = "") +
  theme(legend.background = element_blank()) +
  transition_reveal(time)
```

![](05_exercises_new_files/figure-html/unnamed-chunk-11-1.gif)<!-- -->

```r
anim_save("Q4mallorcabike.gif")
```
 

```r
knitr::include_graphics("Q4mallorcabike.gif")
```

<img src="Q4mallorcabike.gif" title="This is a gift of Professor Lendway's biking trip on Mallorca. There is a dot showing where she is at any given point of time and the different shades of color on the path signify the different elevations she rode on. The darkest colors are the lowest levels, while the lightest colors are higher in elevation." alt="This is a gift of Professor Lendway's biking trip on Mallorca. There is a dot showing where she is at any given point of time and the different shades of color on the path signify the different elevations she rode on. The darkest colors are the lowest levels, while the lightest colors are higher in elevation."  />
  5. In this exercise, you get to meet Lisa's sister, Heather! She is a proud Mac grad, currently works as a Data Scientist where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files putting them in swim, bike, run order (HINT: `bind_rows()`), 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
panama_map <- get_stamenmap(
  bbox = c( left = -79.6866, bottom = 8.8254, right = -79.4817, top = 8.9966),
  maptype = "terrain",
  zoom = 11)

panama_heather <- bind_rows(list(panama_swim, panama_bike, panama_run))
```
  

```r
ggmap(panama_map) +
  geom_path(data = panama_heather,
            aes(x = lon,
                y = lat, 
                color = event),
            size = 1) +
  geom_point(data = panama_heather,
             aes(x = lon, 
                 y = lat,
                 color = event),
             size = 4) +

  theme_map() +
  transition_reveal(time)
```

![](05_exercises_new_files/figure-html/unnamed-chunk-14-1.gif)<!-- -->

```r
anim_save("Q5panamaheather.gif")
```
  

```r
knitr::include_graphics("Q5panamaheather.gif")
```

<img src="Q5panamaheather.gif" title="This is a gif showing Heather's Ironman 70.3 Pan Am Championships in which she swims, bikes, and runs. Each leg of her race is shown in a different color." alt="This is a gif showing Heather's Ironman 70.3 Pan Am Championships in which she swims, bikes, and runs. Each leg of her race is shown in a different color."  />
## COVID-19 data

  6. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. The code below gives the population estimates for each state and loads the `states_map` data. Here is a list of details you should include in the plot:
  
  * Put date in the subtitle.   
  * Because there are so many dates, you are going to only do the animation for the the 15th of each month. So, filter only to those dates - there are some lubridate functions that can help you do this.   
  * Use the `animate()` function to make the animation 200 frames instead of the default 100 and to pause for 10 frames on the end frame.   
  * Use `group = date` in `aes()`.  
  * Comment on what you see.  
  
  __I see__


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

states_map <- map_data("state")

covid19_states <- covid19 %>% 
  arrange(desc(date)) %>% 
  group_by(state) %>%
  #summarise(cases = last(cases)) %>% #this works, but doesn't keep the date (needed later)
  mutate(numberrow = 1:n(), #find out what this means in depth
    state = str_to_lower(`state`))


covid_19_state_populations <- covid19_states %>% 
  left_join(census_pop_est_2018,
            by = c("state" = "state")) %>% 
  mutate(covid_per_10000 = (cases/est_pop_2018)*10000)

covid_day_15 <- covid_19_state_populations %>% 
   filter(day(date) == 15)
```



```r
us_pandemic <- covid_day_15 %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state,
               fill = covid_per_10000,
               group = date)) + #find out why group = date
  expand_limits(x = states_map$long, y = states_map$lat) + 
  labs(title = "COVID-19 Cases in the United States",
       subtitle = "Date: {closest_state}",
       fill = "Cases per 10,000 people") + 
  
  theme_map() + 
  theme(legend.background = element_blank()) +
  transition_states(date, transition_length = 0)
                      #state_length = 10)
  
animate(us_pandemic, nframes = 200, end_pause = 10)
```

![](05_exercises_new_files/figure-html/unnamed-chunk-17-1.gif)<!-- -->

```r
anim_save("Q6pandemictide.gif")
```

```r
knitr::include_graphics("Q6pandemictide.gif")
```

<img src="Q6pandemictide.gif" title="This gif shows the United States' cumulative COVID-19 cases per 10,000 residents as it changes over time." alt="This gif shows the United States' cumulative COVID-19 cases per 10,000 residents as it changes over time."  />
## Your first `shiny` app (for next week!)

  7. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. You should create a new project for the app, separate from the homework project. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' daily number of COVID cases per 100,000 over time. The x-axis will be date. You will have an input box where the user can choose which states to compare (`selectInput()`), a slider where the user can choose the date range, and a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
Put the link to your app here: 

__I couldn't get the shiny to work :(__

Here was my code: 
library(tidyverse)     
library(lubridate)     
library(shiny)         
library(babynames)
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")



ui <- fluidPage("United States' daily number of COVID-19 cases per 100,000 people over time.",
  sliderInput(inputId = "date", 
              label = "Date Range",
              min = as.Date(min(covid19$date)), 
              max = as.Date(max(covid19$date)), 
              value = c(as.Date(min(covid19$date)), as.Date(max(covid19$date)))),
  #textInput("name", 
          #  "Name", 
          #  value = "", 
           # placeholder = "Lisa"),
  selectInput(inputId = "state", 
              label = "State", 
              choices = unique(covid19$state)),
  submitButton(text = "Compare                        Selected States"),
  plotOutput(outputId = "covidplot")
)

server <- function(input, output) {
  
  output$covidplot <- renderPlot({ 
    covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
    
    covid19 <- covid19 %>% 
      group_by(state) %>% 
      summarise(cases = last(cases)) %>% 
      mutate(state = str_to_lower(`state`))
   
     census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
      separate(state, into = c("dot","state"), extra = "merge") %>% 
      select(-dot) %>% 
      mutate(state = str_to_lower(state))
    
     covid_19_state_populations <- covid19_states %>% 
      left_join(census_pop_est_2018,
                by = c("state" = "state"))
     
     modified_covid19 <- covid_19_state_populations %>% 
       mutate(covid_prop = cases/est_pop_2018)  
     
    modified_covid19 %>% 
      #old code
      #filter(state %in% state) %>% 
      #new code - you forgot input$
      filter(state %in% input$state) %>% 
      ggplot() +
      #old code
      #geom_line(aes(x = date, y = case), color = state) +
      #new code - you had case instead of cases and color was outside of aes()
      geom_line(aes(x = date, y = cases, color = state)) +
      scale_x_continuous(limits = input$date) +
      theme_minimal()
  })
}

shinyApp(ui = ui, server = server)
  
## GitHub link

  8. Below, provide a link to your GitHub repo with this set of Weekly Exercises. 

[The beleaguered GitHub link](https://github.com/foriana/IDS_Exercise_5.git)

**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
