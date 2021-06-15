---
title: 'Weekly Exercises #4'
author: "Ingrid O'Connor"
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
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.3     ✓ purrr   0.3.4
## ✓ tibble  3.1.2     ✓ dplyr   1.0.6
## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
## ✓ readr   1.4.0     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(lubridate)     # for date manipulation
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(openintro)     # for the abbr2state() function
```

```
## Loading required package: airports
```

```
## Loading required package: cherryblossom
```

```
## Loading required package: usdata
```

```r
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
```

```
## 
## Attaching package: 'maps'
```

```
## The following object is masked from 'package:purrr':
## 
##     map
```

```r
library(ggmap)         # for mapping points on maps
```

```
## Google's Terms of Service: https://cloud.google.com/maps-platform/terms/.
```

```
## Please cite ggmap if you use it! See citation("ggmap") for details.
```

```r
library(gplots)        # for col2hex() function
```

```
## 
## Attaching package: 'gplots'
```

```
## The following object is masked from 'package:stats':
## 
##     lowess
```

```r
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
```

```
## Linking to GEOS 3.8.1, GDAL 3.1.4, PROJ 6.3.1
```

```r
library(leaflet)       # for highly customizable mapping
library(carData)       # for Minneapolis police stops data
library(ggthemes)      # for more themes (including theme_map())
theme_set(theme_minimal())
```


```r
# Starbucks locations
Starbucks <- read_csv("https://www.macalester.edu/~ajohns24/Data/Starbucks.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   Brand = col_character(),
##   `Store Number` = col_character(),
##   `Store Name` = col_character(),
##   `Ownership Type` = col_character(),
##   `Street Address` = col_character(),
##   City = col_character(),
##   `State/Province` = col_character(),
##   Country = col_character(),
##   Postcode = col_character(),
##   `Phone Number` = col_character(),
##   Timezone = col_character(),
##   Longitude = col_double(),
##   Latitude = col_double()
## )
```

```r
starbucks_us_by_state <- Starbucks %>% 
  filter(Country == "US") %>% 
  count(`State/Province`) %>% 
  mutate(state_name = str_to_lower(abbr2state(`State/Province`))) 

# Lisa's favorite St. Paul places - example for you to create your own data
favorite_stp_by_lisa <- tibble(
  place = c("Home", "Macalester College", "Adams Spanish Immersion", 
            "Spirit Gymnastics", "Bama & Bapa", "Now Bikes",
            "Dance Spectrum", "Pizza Luce", "Brunson's"),
  long = c(-93.1405743, -93.1712321, -93.1451796, 
           -93.1650563, -93.1542883, -93.1696608, 
           -93.1393172, -93.1524256, -93.0753863),
  lat = c(44.950576, 44.9378965, 44.9237914,
          44.9654609, 44.9295072, 44.9436813, 
          44.9399922, 44.9468848, 44.9700727)
  )

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   date = col_date(format = ""),
##   state = col_character(),
##   fips = col_character(),
##   cases = col_double(),
##   deaths = col_double()
## )
```

## Put your homework on GitHub!

If you were not able to get set up on GitHub last week, go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) and get set up first. Then, do the following (if you get stuck on a step, don't worry, I will help! You can always get started on the homework and we can figure out the GitHub piece later):

* Create a repository on GitHub, giving it a nice name so you know it is for the 4th weekly exercise assignment (follow the instructions in the document/video).  
* Copy the repo name so you can clone it to your computer. In R Studio, go to file --> New project --> Version control --> Git and follow the instructions from the document/video.  
* Download the code from this document and save it in the repository folder/project on your computer.  
* In R Studio, you should then see the .Rmd file in the upper right corner in the Git tab (along with the .Rproj file and probably .gitignore).  
* Check all the boxes of the files in the Git tab under Stage and choose commit.  
* In the commit window, write a commit message, something like "Initial upload" would be appropriate, and commit the files.  
* Either click the green up arrow in the commit window or close the commit window and click the green up arrow in the Git tab to push your changes to GitHub.  
* Refresh your GitHub page (online) and make sure the new documents have been pushed out.  
* Back in R Studio, knit the .Rmd file. When you do that, you should have two (as long as you didn't make any changes to the .Rmd file, in which case you might have three) files show up in the Git tab - an .html file and an .md file. The .md file is something we haven't seen before and is here because I included `keep_md: TRUE` in the YAML heading. The .md file is a markdown (NOT R Markdown) file that is an interim step to creating the html file. They are displayed fairly nicely in GitHub, so we want to keep it and look at it there. Click the boxes next to these two files, commit changes (remember to include a commit message), and push them (green up arrow).  
* As you work through your homework, save and commit often, push changes occasionally (maybe after you feel finished with an exercise?), and go check to see what the .md file looks like on GitHub.  
* If you have issues, let me know! This is new to many of you and may not be intuitive at first. But, I promise, you'll get the hang of it! 


## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises from tutorial

These exercises will reiterate what you learned in the "Mapping data with R" tutorial. If you haven't gone through the tutorial yet, you should do that first.

### Starbucks locations (`ggmap`)

  1. Add the `Starbucks` locations to a world map. Add an aesthetic to the world map that sets the color of the points according to the ownership type. What, if anything, can you deduce from this visualization?  
  

```r
world <- get_stamenmap(
  bbox = c(left = -180, bottom = -57, right = 179, top = 82.1),
  maptype = "toner",
  zoom = 2)
```

```
## Source : http://tile.stamen.com/toner/2/0/0.png
```

```
## Source : http://tile.stamen.com/toner/2/1/0.png
```

```
## Source : http://tile.stamen.com/toner/2/2/0.png
```

```
## Source : http://tile.stamen.com/toner/2/3/0.png
```

```
## Source : http://tile.stamen.com/toner/2/0/1.png
```

```
## Source : http://tile.stamen.com/toner/2/1/1.png
```

```
## Source : http://tile.stamen.com/toner/2/2/1.png
```

```
## Source : http://tile.stamen.com/toner/2/3/1.png
```

```
## Source : http://tile.stamen.com/toner/2/0/2.png
```

```
## Source : http://tile.stamen.com/toner/2/1/2.png
```

```
## Source : http://tile.stamen.com/toner/2/2/2.png
```

```
## Source : http://tile.stamen.com/toner/2/3/2.png
```

```r
ggmap(world) +
  geom_point(data = Starbucks,
             aes(x = Longitude, y = Latitude, color = `Ownership Type`),
             alpha = 0.3,
             size = 0.1) +
  theme_map() +
  guides(colour = guide_legend(override.aes = list(size=5)))
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-1-1.png)<!-- -->
  
  Overall, Starbucks seems to be most popular in the US, Japan, and the UK. This visualization tells us that the Starbucks in North America are primarily company owned and licensed. The Starbucks in Japan are primarily joint venture. The Starbucks in the UK are primarily franchise. There are other areas of consolidated types in other countries as well.

  2. Construct a new map of Starbucks locations in the Twin Cities metro area (approximately the 5 county metro area).  
  

```r
metro <- get_stamenmap(
    bbox = c(left = -93.9, bottom = 44.7, right = -92.4, top = 45.3), 
    maptype = "toner-lite",
    zoom = 10)
```

```
## Source : http://tile.stamen.com/toner-lite/10/244/367.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/245/367.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/246/367.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/247/367.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/248/367.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/249/367.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/244/368.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/245/368.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/246/368.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/247/368.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/248/368.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/249/368.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/244/369.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/245/369.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/246/369.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/247/369.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/248/369.png
```

```
## Source : http://tile.stamen.com/toner-lite/10/249/369.png
```

```r
ggmap(metro) +
  geom_point(data = Starbucks, 
             aes(x = Longitude, y = Latitude, color = `Ownership Type`), 
             size = 1.5) +
  theme_map()
```

```
## Warning: Removed 25450 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-2-1.png)<!-- -->
  

  3. In the Twin Cities plot, play with the zoom number. What does it do?  (just describe what it does - don't actually include more than one map).  
  
  Bigger numbers of zoom give more details while smaller numbers make the map be less detailed. For example, in the last map if I change the zoom to 7, the only city names visible are Minneapolis, St. Paul, and Bloomington.

  4. Try a couple different map types (see `get_stamenmap()` in help and look at `maptype`). Include a map with one of the other map types.  
  

```r
metro2 <- get_stamenmap(
    bbox = c(left = -93.9, bottom = 44.7, right = -92.4, top = 45.3), 
    maptype = "watercolor",
    zoom = 10)
```

```
## Source : http://tile.stamen.com/watercolor/10/244/367.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/245/367.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/246/367.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/247/367.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/248/367.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/249/367.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/244/368.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/245/368.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/246/368.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/247/368.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/248/368.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/249/368.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/244/369.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/245/369.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/246/369.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/247/369.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/248/369.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/249/369.jpg
```

```r
ggmap(metro2) +
  geom_point(data = Starbucks, 
             aes(x = Longitude, y = Latitude), 
             size = 1.5) +
  theme_map()
```

```
## Warning: Removed 25450 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->
  

  5. Add a point to the map that indicates Macalester College and label it appropriately. There are many ways you can do think, but I think it's easiest with the `annotate()` function (see `ggplot2` cheatsheet).
  

```r
ggmap(metro) +
  geom_point(data = Starbucks, 
             aes(x = Longitude, y = Latitude), 
             size = 1.5) +
  annotate("text", y = 44.925, x = -93.1691, label = "Macalester", color ="red", size = 3) +
  annotate("point", y = 44.9379, x = -93.1691, color = "red") +
  theme_map()
```

```
## Warning: Removed 25450 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-4-1.png)<!-- -->
  

### Choropleth maps with Starbucks data (`geom_map()`)

The example I showed in the tutorial did not account for population of each state in the map. In the code below, a new variable is created, `starbucks_per_10000`, that gives the number of Starbucks per 10,000 people. It is in the `starbucks_with_2018_pop_est` dataset.


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   state = col_character(),
##   est_pop_2018 = col_double()
## )
```

```r
starbucks_with_2018_pop_est <-
  starbucks_us_by_state %>% 
  left_join(census_pop_est_2018,
            by = c("state_name" = "state")) %>% 
  mutate(starbucks_per_10000 = (n/est_pop_2018)*10000)
```

  6. **`dplyr` review**: Look through the code above and describe what each line of code does.
  
  167 reads in a csv file and assigns it to be "census_pop_est_2018"
  168 separates the column "state" into two columns, called "dot" and "state", splitting by any non-alphanumeric characters. "extra" makes it so that if there are too many pieces from the separation, it will only be split twice.
  169 removes the "dot" column from the table by selecting all columns except that one.
  170 replaces the "state" column with the old state column, but all lower-case letters.
  174 joins the two tables census_pop_est_2018 and starbucks_us_by_state by making state_name equal to state. It is a left join, so it will keep all cases in starbucks_us_by_state.
  176 creates a new column, starbucks_per_10000, which divides n by the estimated population of the state in 2018 and multiplies that by 10000.

  7. Create a choropleth map that shows the number of Starbucks per 10,000 people on a map of the US. Use a new fill color, add points for all Starbucks in the US (except Hawaii and Alaska), add an informative title for the plot, and include a caption that says who created the plot (you!). Make a conclusion about what you observe.
  

```r
states_map <- map_data("state")

starbucks_with_2018_pop_est %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state_name,
               fill = starbucks_per_10000)) +
  geom_point(data = Starbucks %>% filter(`Country` == "US", `State/Province` != "HI", `State/Province` != "AK"),
             aes(x = Longitude, y = Latitude),
             size = .05,
             alpha = .2, 
             color = "goldenrod") +
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map() +
  labs(title = "Starbucks in U.S.", fill = "Starbucks per 10,000 People", caption = "Plot by Ingrid O'Connor") +
  theme(legend.background = element_blank(), legend.position = "right")
```

![](04_exercises_files/figure-html/unnamed-chunk-6-1.png)<!-- -->
  
  Although there are a lot of Starbucks in the northeast, it also must be more populated than states on the West coast, which seem to have a similar amomunt of Starbucks, but are much lighter in color (more Starbucks per 10,000 people).

### A few of your favorite things (`leaflet`)

  8. In this exercise, you are going to create a single map of some of your favorite places! The end result will be one map that satisfies the criteria below. 

  * Create a data set using the `tibble()` function that has 10-15 rows of your favorite places. The columns will be the name of the location, the latitude, the longitude, and a column that indicates if it is in your top 3 favorite locations or not. For an example of how to use `tibble()`, look at the `favorite_stp_by_lisa` I created in the data R code chunk at the beginning.  
  

```r
favorites_ingrid <- tibble(
  place = c("Home", "Macalester College", "Cam Ranh Bay Restaurant", "Subway", "My Cabin", "Black Sheep Pizza", "Theodore Wirth Park", "Giant's Ridge", "Minnehaha Falls", "University of Minnesota"),
  long = c(-93.346580, -93.1712321, -93.291980, -93.354020, -91.909092, -93.275688, -93.321327, -92.303300, -93.210983, -93.233118),
  lat = c(44.733210, 44.9378965, 44.746650, 44.746700, 47.371596, 44.987309, 44.992489, 47.575710, 44.915274, 44.976160),
  favorite = c(TRUE, TRUE, FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, FALSE)
  )
```
  

  * Create a `leaflet` map that uses circles to indicate your favorite places. Label them with the name of the place. Choose the base map you like best. Color your 3 favorite places differently than the ones that are not in your top 3 (HINT: `colorFactor()`). Add a legend that explains what the colors mean.  
  

```r
pal2 <- colorFactor(c("red","blue"), 
                     domain = favorites_ingrid$favorite) 
leaflet(favorites_ingrid) %>%
  addTiles() %>%
  addCircles(
    popup = ~paste(place),
    opacity = 1,
    color = ~pal2(favorite)
  ) %>%
  addLegend(pal = pal2, 
            values = ~favorite, 
            title = "Top 3 Favorite?",
            position = "bottomright") 
```

```
## Assuming "long" and "lat" are longitude and latitude, respectively
```

```{=html}
<div id="htmlwidget-a963a324de85f4306b3d" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-a963a324de85f4306b3d">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["//{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"http://openstreetmap.org\">OpenStreetMap<\/a> contributors, <a href=\"http://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA<\/a>"}]},{"method":"addCircles","args":[[44.73321,44.9378965,44.74665,44.7467,47.371596,44.987309,44.992489,47.57571,44.915274,44.97616],[-93.34658,-93.1712321,-93.29198,-93.35402,-91.909092,-93.275688,-93.321327,-92.3033,-93.210983,-93.233118],10,null,null,{"interactive":true,"className":"","stroke":true,"color":["#0000FF","#0000FF","#FF0000","#FF0000","#0000FF","#FF0000","#FF0000","#FF0000","#FF0000","#FF0000"],"weight":5,"opacity":1,"fill":true,"fillColor":["#0000FF","#0000FF","#FF0000","#FF0000","#0000FF","#FF0000","#FF0000","#FF0000","#FF0000","#FF0000"],"fillOpacity":0.2},["Home","Macalester College","Cam Ranh Bay Restaurant","Subway","My Cabin","Black Sheep Pizza","Theodore Wirth Park","Giant's Ridge","Minnehaha Falls","University of Minnesota"],null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null,null]},{"method":"addLegend","args":[{"colors":["#FF0000","#0000FF"],"labels":["FALSE","TRUE"],"na_color":null,"na_label":"NA","opacity":0.5,"position":"bottomright","type":"factor","title":"Top 3 Favorite?","extra":null,"layerId":null,"className":"info legend","group":null}]}],"limits":{"lat":[44.73321,47.57571],"lng":[-93.35402,-91.909092]}},"evals":[],"jsHooks":[]}</script>
```
  
  
  * Connect all your locations together with a line in a meaningful way (you may need to order them differently in the original data).  
  

```r
# Order of when I first visited
favorites_ingrid <- tibble(
  place = c("Home", "My Cabin", "Minnehaha Falls", "University of Minnesota", "Cam Ranh Bay Restaurant", "Subway", "Black Sheep Pizza", "Theodore Wirth Park", "Giant's Ridge", "Macalester College"),
  long = c(-93.346580, -91.909092, -93.210983, -93.233118, -93.291980, -93.354020, -93.275688, -93.321327, -92.303300, -93.1712321),
  lat = c(44.733210, 47.371596, 44.915274, 44.976160, 44.746650, 44.746700, 44.987309, 44.992489, 47.575710, 44.9378965),
  favorite = c(TRUE, TRUE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, TRUE)
  )
leaflet(favorites_ingrid) %>%
  addTiles() %>%
  addCircles(
    popup = ~paste(place),
    opacity = 1,
    color = ~pal2(favorite)
  ) %>%
  addLegend(pal = pal2, 
            values = ~favorite, 
            title = "Top 3 Favorite?",
            position = "bottomright") %>%
  addPolylines(lng = ~long, 
               lat = ~lat, 
               color = col2hex("darkred"))
```

```
## Assuming "long" and "lat" are longitude and latitude, respectively
```

```{=html}
<div id="htmlwidget-bce88a195858506b1e61" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-bce88a195858506b1e61">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["//{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"http://openstreetmap.org\">OpenStreetMap<\/a> contributors, <a href=\"http://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA<\/a>"}]},{"method":"addCircles","args":[[44.73321,47.371596,44.915274,44.97616,44.74665,44.7467,44.987309,44.992489,47.57571,44.9378965],[-93.34658,-91.909092,-93.210983,-93.233118,-93.29198,-93.35402,-93.275688,-93.321327,-92.3033,-93.1712321],10,null,null,{"interactive":true,"className":"","stroke":true,"color":["#0000FF","#0000FF","#FF0000","#FF0000","#FF0000","#FF0000","#FF0000","#FF0000","#FF0000","#0000FF"],"weight":5,"opacity":1,"fill":true,"fillColor":["#0000FF","#0000FF","#FF0000","#FF0000","#FF0000","#FF0000","#FF0000","#FF0000","#FF0000","#0000FF"],"fillOpacity":0.2},["Home","My Cabin","Minnehaha Falls","University of Minnesota","Cam Ranh Bay Restaurant","Subway","Black Sheep Pizza","Theodore Wirth Park","Giant's Ridge","Macalester College"],null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null,null]},{"method":"addLegend","args":[{"colors":["#FF0000","#0000FF"],"labels":["FALSE","TRUE"],"na_color":null,"na_label":"NA","opacity":0.5,"position":"bottomright","type":"factor","title":"Top 3 Favorite?","extra":null,"layerId":null,"className":"info legend","group":null}]},{"method":"addPolylines","args":[[[[{"lng":[-93.34658,-91.909092,-93.210983,-93.233118,-93.29198,-93.35402,-93.275688,-93.321327,-92.3033,-93.1712321],"lat":[44.73321,47.371596,44.915274,44.97616,44.74665,44.7467,44.987309,44.992489,47.57571,44.9378965]}]]],null,null,{"interactive":true,"className":"","stroke":true,"color":"#8B0000","weight":5,"opacity":0.5,"fill":false,"fillColor":"#8B0000","fillOpacity":0.2,"smoothFactor":1,"noClip":false},null,null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]}],"limits":{"lat":[44.73321,47.57571],"lng":[-93.35402,-91.909092]}},"evals":[],"jsHooks":[]}</script>
```
  
  
  * If there are other variables you want to add that could enhance your plot, do that now.  
  

```r
favorites_ingrid %>%
  mutate(twin_cities = c(TRUE, FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, FALSE, TRUE))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["place"],"name":[1],"type":["chr"],"align":["left"]},{"label":["long"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["lat"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["favorite"],"name":[4],"type":["lgl"],"align":["right"]},{"label":["twin_cities"],"name":[5],"type":["lgl"],"align":["right"]}],"data":[{"1":"Home","2":"-93.34658","3":"44.73321","4":"TRUE","5":"TRUE"},{"1":"My Cabin","2":"-91.90909","3":"47.37160","4":"TRUE","5":"FALSE"},{"1":"Minnehaha Falls","2":"-93.21098","3":"44.91527","4":"FALSE","5":"TRUE"},{"1":"University of Minnesota","2":"-93.23312","3":"44.97616","4":"FALSE","5":"TRUE"},{"1":"Cam Ranh Bay Restaurant","2":"-93.29198","3":"44.74665","4":"FALSE","5":"TRUE"},{"1":"Subway","2":"-93.35402","3":"44.74670","4":"FALSE","5":"TRUE"},{"1":"Black Sheep Pizza","2":"-93.27569","3":"44.98731","4":"FALSE","5":"TRUE"},{"1":"Theodore Wirth Park","2":"-93.32133","3":"44.99249","4":"FALSE","5":"TRUE"},{"1":"Giant's Ridge","2":"-92.30330","3":"47.57571","4":"FALSE","5":"FALSE"},{"1":"Macalester College","2":"-93.17123","3":"44.93790","4":"TRUE","5":"TRUE"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
  
  
## Revisiting old datasets

This section will revisit some datasets we have used previously and bring in a mapping component. 

### Bicycle-Use Patterns

The data come from Washington, DC and cover the last quarter of 2014.

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usualy, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`. This code reads in the large dataset right away.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   name = col_character(),
##   lat = col_double(),
##   long = col_double(),
##   nbBikes = col_double(),
##   nbEmptyDocks = col_double()
## )
```

  9. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. This time, plot the points on top of a map. Use any of the mapping tools you'd like.
  

```r
dc <- get_stamenmap(
  bbox = c(left = -77.3490, top = 39.1234, bottom = 38.7886, right = -76.6005),
  maptype = "terrain",
  zoom = 11
)
```

```
## Source : http://tile.stamen.com/terrain/11/583/781.png
```

```
## Source : http://tile.stamen.com/terrain/11/584/781.png
```

```
## Source : http://tile.stamen.com/terrain/11/585/781.png
```

```
## Source : http://tile.stamen.com/terrain/11/586/781.png
```

```
## Source : http://tile.stamen.com/terrain/11/587/781.png
```

```
## Source : http://tile.stamen.com/terrain/11/588/781.png
```

```
## Source : http://tile.stamen.com/terrain/11/583/782.png
```

```
## Source : http://tile.stamen.com/terrain/11/584/782.png
```

```
## Source : http://tile.stamen.com/terrain/11/585/782.png
```

```
## Source : http://tile.stamen.com/terrain/11/586/782.png
```

```
## Source : http://tile.stamen.com/terrain/11/587/782.png
```

```
## Source : http://tile.stamen.com/terrain/11/588/782.png
```

```
## Source : http://tile.stamen.com/terrain/11/583/783.png
```

```
## Source : http://tile.stamen.com/terrain/11/584/783.png
```

```
## Source : http://tile.stamen.com/terrain/11/585/783.png
```

```
## Source : http://tile.stamen.com/terrain/11/586/783.png
```

```
## Source : http://tile.stamen.com/terrain/11/587/783.png
```

```
## Source : http://tile.stamen.com/terrain/11/588/783.png
```

```
## Source : http://tile.stamen.com/terrain/11/583/784.png
```

```
## Source : http://tile.stamen.com/terrain/11/584/784.png
```

```
## Source : http://tile.stamen.com/terrain/11/585/784.png
```

```
## Source : http://tile.stamen.com/terrain/11/586/784.png
```

```
## Source : http://tile.stamen.com/terrain/11/587/784.png
```

```
## Source : http://tile.stamen.com/terrain/11/588/784.png
```

```r
stations2 <- Trips %>%
  group_by(sstation) %>%
  summarize(number_of_departures = n()) %>%
  inner_join(Stations, by = c("sstation" = "name"))

ggmap(dc) + 
  geom_point(data = stations2,
             aes(x = long, y = lat, color = number_of_departures),
             size = 1,
             alpha = 1) +
  scale_color_viridis_c(option = "plasma") +
  theme_map() +
  labs(color = "Number of Departures", title = "Washington D.C. Bike Rental Stations (last quarter of 2014)")
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-12-1.png)<!-- -->
  
  10. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? Also plot this on top of a map. I think it will be more clear what the patterns are.
  

```r
stations3 <- Trips %>%
  mutate(client_casual = (client == "Casual")) %>%
  group_by(sstation) %>%
  summarize(total_departures = n(), casual_departures = sum(client_casual)) %>%
  mutate(percent_casual = casual_departures / total_departures) %>%
  inner_join(Stations, by = c("sstation" = "name"))
```

```r
ggmap(dc) + 
  geom_point(data = stations3,
             aes(x = long, y = lat, color = percent_casual),
             size = 1,
             alpha = 1) +
  scale_color_viridis_c(option = "plasma") +
  theme_map() +
  labs(color = "Percent of Casual Clients", title = "Washington D.C. Bike Rental Stations (last quarter of 2014)") +
  theme(legend.position = "right")
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

Many of the stations with a high percentage of casual clients are along the river and in the center of the city, most likely at tourist sites.
  
### COVID-19 data

The following exercises will use the COVID-19 data from the NYT.

  11. Create a map that colors the states by the most recent cumulative number of COVID-19 cases (remember, these data report cumulative numbers so you don't need to compute that). Describe what you see. What is the problem with this map?
  
  12. Now add the population of each state to the dataset and color the states by most recent cumulative cases/10,000 people. See the code for doing this with the Starbucks data. You will need to make some modifications. 
  
  13. **CHALLENGE** Choose 4 dates spread over the time period of the data and create the same map as in exercise 12 for each of the dates. Display the four graphs together using faceting. What do you notice?
  
## Minneapolis police stops

These exercises use the datasets `MplsStops` and `MplsDemo` from the `carData` library. Search for them in Help to find out more information.

  14. Use the `MplsStops` dataset to find out how many stops there were for each neighborhood and the proportion of stops that were for a suspicious vehicle or person. Sort the results from most to least number of stops. Save this as a dataset called `mpls_suspicious` and display the table.  
  
  15. Use a `leaflet` map and the `MplsStops` dataset to display each of the stops on a map as a small point. Color the points differently depending on whether they were for suspicious vehicle/person or a traffic stop (the `problem` variable). HINTS: use `addCircleMarkers`, set `stroke = FAlSE`, use `colorFactor()` to create a palette.  
  
  16. Save the folder from moodle called Minneapolis_Neighborhoods into your project/repository folder for this assignment. Make sure the folder is called Minneapolis_Neighborhoods. Use the code below to read in the data and make sure to **delete the `eval=FALSE`**. Although it looks like it only links to the .sph file, you need the entire folder of files to create the `mpls_nbhd` data set. These data contain information about the geometries of the Minneapolis neighborhoods. Using the `mpls_nbhd` dataset as the base file, join the `mpls_suspicious` and `MplsDemo` datasets to it by neighborhood (careful, they are named different things in the different files). Call this new dataset `mpls_all`.


```r
mpls_nbhd <- st_read("Minneapolis_Neighborhoods/Minneapolis_Neighborhoods.shp", quiet = TRUE)
```

  17. Use `leaflet` to create a map from the `mpls_all` data  that colors the neighborhoods by `prop_suspicious`. Display the neighborhood name as you scroll over it. Describe what you observe in the map.
  
  18. Use `leaflet` to create a map of your own choosing. Come up with a question you want to try to answer and use the map to help answer that question. Describe what your map shows. 
  
  
## GitHub link

  19. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 04_exercises.Rmd, provide a link to the 04_exercises.md file, which is the one that will be most readable on GitHub.


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
