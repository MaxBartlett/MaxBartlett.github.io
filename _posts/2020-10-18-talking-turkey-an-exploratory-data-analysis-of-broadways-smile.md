---
title: "Talking Turkey: Exploratory Data Analysis of Broadway's <i>Smile</i>"
layout: post
date: 2020-10-18 15:00
image: /assets/images/turkey-blog-header.png
headerImage: false
tag:
- R
- Exploratory Data Analysis
- Tidyverse
- Broadway
- Musicals
- #TidyTuesday
- ggplot2
- Data Visualization
projects: false
category: blog
author: maxbartlett
description: "Max Bartlett's Blog"
---

![](/assets/images/turkey-blog-header.jpg)

## Preface

*This lesson uses the [Broadway Musicals](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-04-28/readme.md) dataset from [R For Data Science #TidyTuesday](https://github.com/rfordatascience/tidytuesday). Credit to [Alex Cookson](https://github.com/tacookson) for providing the data, cleaning script, and README.*

*This data comes from Playbill. Weekly box office grosses comprise data on revenue and attendance figures for theatres that are part of The Broadway League, an industry association for, you guessed it, Broadway theatre.*

*CPI data is from the U.S. Bureau of Labor Statistics. There are many, many measures of CPI, so the one used here is "All items less food and energy in U.S. city average, all urban consumers, seasonally adjusted" (table CUSR0000SA0L1E).*


## Introduction

Exploratory data analysis (EDA) is a vital part of every data scientist's toolkit, and playing with R for Data Science's [#TidyTuesday](https://github.com/rfordatascience/tidytuesday) repository is an excellent way to hone your skills. When I stumbled upon the Broadway musicals dataset, I knew that I had to write a blog post. Both of my parents were professional actors in musical theater and were part of Broadway productions, and I hoped to see how their experiences in showbusiness aligned with the data.

In this blog post, I'll guide you through my own exploratory data analysis in R. I've included the code in a Github repository located [here](https://github.com/MaxBartlett/MaxBartlett.github.io/blob/master/assets/blog/talking-turkey.Rmd), and I encourage you to download and follow along.

## Background

> **Turkey:** Virtually all forms of entertainment use this [term] to indicate a bad or losing show. Its origin comes from actors (turkey actors) who opened poor shows on Thanksgiving Day in the hope of making money as part of the annual tradition.[^1]

My primary focus for this analysis is the 1986 Broadway production of *Smile* by Marvin Hamlisch and Howard Ashman, in which my mother was a cast member. From what she's told me, the show was a turkey, and I'm interested in seeing how the data reflects this.

## Forming Questions

Since I have prior knowledge of the data, it's relatively easy to frame my analysis. But even if that wasn't the case, I could still generate a topic of interest by asking:

* What type of variation occurs within my variables?
* What type of covariation occurs between my variables?

And answering these questions by:

* Visualizing the distribution of the variable’s values
* Visualizing the relationship between two or more variables

But to do this, I must first examine the data. It's important to peruse the data dictionary, if one exists, before writing any code. The data dictionary for this project can be found in the [GitHub repository’s README](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-04-28/readme.md). If you aren’t familiar with the topic of the data, you may find it helpful to perform general research as well.

## Examining the Data

First we'll load the appropriate R packages

```r
library(tidyverse)
library(lubridate) # for handling dates
library(scales) # for additional ggplot2 functionality
```

And then load the data in R.

```r
# Read in the data
grosses <- read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-04-28/grosses.csv', guess_max = 40000)
synopses <- read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-04-28/synopses.csv')
cpi <- read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-04-28/cpi.csv')
pre_1985_starts <- read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-04-28/pre-1985-starts.csv')
```

As we can see there are four datasets, `grosses`, `synopses`, `cpi`, and `pre_1985_starts`. From looking at the data dictionary above, I think the `grosses` tibble will be most important in evaluating *Smile’s* popularity and financial performance. The `cpi` tibble will also be useful in calculating an approximate inflation rate.

Below, I’m creating a new tibble `grosses_adj`, which contains new columns (also with the suffix ’_adj’) that account for inflation in dollar amounts. A dollar is worth a lot less now than in the ’80s!

```r
# pulls most recent cpi in the tibble (March 2020)
cpi_recent <- cpi %>%
  select(cpi) %>%
  slice_tail() %>%
  pull()
```
```r
# creates new columns in 2020 dollars with suffix '_adj'
grosses_adj <- grosses %>%
  mutate(year_month = floor_date(week_ending, 'month')) %>%
  left_join(cpi, 'year_month') %>%
  mutate(across(contains(c('gross', 'price')), 
                list(adj = ~ cpi_recent/cpi * .))) %>%
         select(-year_month, -cpi)
```

## Visualizing a Single Variable

From here, I'll consider what visualizations might be appropriate for tracking *Smile's* success (or lack thereof).

Each row, or observation, represents one week of a show's run. Therefore, my visualizations will display how certain statistics about the show's weekly financial performance and attendance varied across time.

As a reminder, let's refer back to the documentation. According to the data dictionary, `weekly_gross` is the weekly box office gross (a.k.a. total revenue, not including costs and taxes) for an individual show, while `week_ending` is the date of the end of the weekly measurement period (always a Sunday). One of my new variables, `weekly_gross_adj` is simply `weekly_gross` adjusted for inflation, current as of March 2020.

Since this is time-series data (each observation comprises values at a certain time), it is best displayed as a line chart. First, I'll plot `weekly_gross_adj` across the dates in `week_ending`. 

I'll build up my plot piece by piece using `ggplot2`, with new lines commented to explain their function.

### Initial Graph

```r
grosses_adj %>% # dataset adjusted for inflation
  filter(show == 'Smile') %>% # display data only for the show 'Smile'
  ggplot(aes(x = week_ending, y = weekly_gross_adj)) +  # plot weekly_gross_adj against week_ending
  geom_line() + # plot line
  geom_point() # plot points
```
![](/assets/images/talking-turkey/1.png)

Since there are so few observations, I've added both ```geom_line()``` and ```geom_point()``` to the graph, as ```geom_line()``` alone could be inadvertantly deceptive. I'll continue to apply data visualization best practices by adding informative labels.

### Adding Labels

```r
grosses_adj %>%
  filter(show == 'Smile') %>%
  ggplot(aes(x = week_ending, y = weekly_gross_adj)) +
  geom_line() +
  geom_point() +
  labs(title = 'Weekly Adjusted Gross Revenue for Smile on Broadway', # add title label
       x = 'Date', # add x-axis label
       y = 'Adjusted Gross Revenue (March 2020)') # add y-axis label
```
![](/assets/images/talking-turkey/2.png)

This graph is really starting to come together. But two things are still difficult to interpret. The x-axis labels for date don't display the year, and the y-axis labels are displayed in scientific notation. I can change these by using the [```scale_continuous```](https://ggplot2.tidyverse.org/reference/scale_continuous.html) family of functions in ```ggplot2```. Below, I'll change the x-axis labels to a 'month-day-year' format, and change the y-axis labels to dollars. I'll also change the theme for aesthetic reasons.


```r
grosses_adj %>%
  filter(show == 'Smile') %>%
  ggplot(aes(x = week_ending, y = weekly_gross_adj)) +
  geom_line() +
  geom_point() +
  labs(title = 'Weekly Adjusted Gross Revenue for Smile on Broadway',
       x = 'Date',
       y = 'Adjusted Gross Revenue (March 2020)') +
  scale_x_date(labels = date_format("%m-%d-%Y")) + # change the x-axis labels to month-date-year
  scale_y_continuous(labels = dollar) + # change the y-axis labels to dollars
  theme_minimal() # change the theme to minimal
```
![](/assets/images/talking-turkey/3.png)

### To Truncate or Not to Truncate?

You may notice one last thing that could be important -- the y-axis doesn't start at 0. Is this deceptive? Like many things in statistics, it depends. Ask yourself [these](http://www.chadskelton.com/2018/06/bar-charts-should-always-start-at-zero.html) questions if you find yourself in a similar situation:

*   Does this visualization only tell part of the story?

    * This graph displays data for the entire run of *Smile*. If I were to show only the first two weeks, for example, it would tell a completely different story.

*   Is the relationship otherwise minimized/exaggerated by including/not including zero? That is, does a small numeric change make a significant practical difference?

    *  For a Broadway production, it likely doesn't make a great deal of difference either way. For an example where it does make a difference, consider these [two](https://www.washingtonpost.com/resizer/XiLCHAesheKU4QJ_niK8VrlZvhw=/arc-anglerfish-washpost-prod-washpost/public/NZRQMVUGJY4GHALRVT2A6Z7RGM.jpg) [graphs](https://www.washingtonpost.com/resizer/eyb7mBUBRd_qnHZ4jaRD9DsanaE=/arc-anglerfish-washpost-prod-washpost/public/ZCS3Y6M2MM72VHV7CM3PFGL6PU.jpg). While the increase looks small in the first graph, it is deceptive because a small increase in global temperature has enormous real-world implications.

*   Is zero a reasonable value for the dataset?

    * To my knowledge, no Broadway show has ever grossed $0 in a given week. I'll check the data below to make sure:

```r
grosses_adj %>%
  filter(weekly_gross_adj == 0) %>% # filtering for shows that have grossed $0
  select(week_ending, show, seats_sold, weekly_gross_adj) # selecting relevant columns
```
```r
## # A tibble: 125 x 4
##    week_ending show                                  seats_sold weekly_gross_adj
##    <date>      <chr>                                      <dbl>            <dbl>
##  1 1986-03-30  "The Flying Karamazov Brothers \"Jug…          0                0
##  2 1986-04-06  "The Flying Karamazov Brothers \"Jug…          0                0
##  3 1986-04-13  "The Flying Karamazov Brothers \"Jug…          0                0
##  4 1986-04-20  "The Flying Karamazov Brothers \"Jug…          0                0
##  5 1993-04-11  "Raffi"                                        0                0
##  6 1994-11-20  "Basia on Broadway"                            0                0
##  7 1994-11-27  "Basia on Broadway"                            0                0
##  8 2007-10-14  "Young Frankenstein"                           0                0
##  9 2007-10-21  "Young Frankenstein"                           0                0
## 10 2007-10-28  "Young Frankenstein"                           0                0
## # … with 115 more rows
```

While there are some instances of this in the data, reality doesn't reflect this. For example, in the Broadway production of Mel Brooks' *Young Frankenstein*, 

> *The show's producers ... broke with Broadway tradition and decided they will not disclose their weekly box-office grosses and attendance figures.*[^2]<sup>,</sup>[^3]

So a value of 0 actually indicates missing data. Your conclusions are only as good as your data source, so make sure to double check them if something seems fishy.

In this case, I'll keep the original graph. But I'll print the adjusted graph below to show the difference.

```{r}
grosses_adj %>%
  filter(show == 'Smile') %>%
  ggplot(aes(x = week_ending, y = weekly_gross_adj)) +
  geom_line() +
  geom_point() +
  labs(title = 'Weekly Gross Revenue for Smile on Broadway',
       x = 'Date',
       y = 'Adjusted Gross Revenue (March 2020)') +
  scale_x_date(labels = date_format("%m-%d-%Y")) +
  scale_y_continuous(labels = dollar, limits = c(0, 800000)) + # change graph limits to include 0
  theme_minimal()
```
![](/assets/images/talking-turkey/4.png)

While both graphs tell the same story, the second graph adds unnecessary white space. 

Now that I've settled on the final graph, I can begin to interpret it.

## Interpreting the Visualization

### Previews

As we can see, the show started out with a very low gross revenue, which rose for about two weeks until it hit its peak at the end of November, then it quickly plummeted until the end of December, before it slowly rose again until it closed in the middle of January. 

At first this might seem confusing. Why did the revenue rise and fall in such quick succession? But it might make more sense with appropriate context.

Before officially opening, every Broadway show starts off in previews.

> Every single Broadway show starts off in previews and the shows in this stage are making corrections, edits and updates to their production in order to make the show perfect. Show directors change songs, scenes, characters, actors, costumes, choreography, lighting and even the theatre sets during these performances.[^4]

Up until recently, preview tickets were usually much cheaper than tickets for the finalized show, but this gap has tightened with the advent of the internet.

In ```grosses_adj``` there is a ```previews``` column displaying the number of previews in a given week. Let's add this as a label to the graph.

```r
grosses_adj %>%
  filter(show == 'Smile') %>%
  ggplot(aes(x = week_ending, y = weekly_gross_adj, label = previews)) + # add previews label
  geom_line() +
  geom_point() +
  geom_text(nudge_x = -1) + # offset previews label from line so we can see it
  labs(title = 'Weekly Gross Revenue for Smile on Broadway',
       x = 'Date',
       y = 'Adjusted Gross Revenue (March 2020)') +
  scale_x_date(labels = date_format("%m-%d-%Y")) +
  scale_y_continuous(labels = dollar) +
  theme_minimal()
```
![](/assets/images/talking-turkey/5.png)

Now we know that there were two weeks of previews before the show officially opened. The first 'week' of the show was actually just a weekend, with three shows (Saturday night, Sunday matinée, Sunday Evening), all of which were previews. The next week had eight shows, all of which were previews.

### Critics

On the opening night of Broadway shows, critics flock to the theater and prepare their reviews for the next morning's paper. *Smile*'s opening night was November 24, 1986, and Frank Rich's [review](https://www.nytimes.com/1986/11/25/theater/theater-smile-a-musical-comedy.html) in *The New York Times* was printed the next day.

> *Frank Rich was chief theater critic of The New York Times from 1980 to 1993, earning the nickname "Butcher of Broadway" for his power over the prospects of Broadway shows*[^5]

Reviews from major critics can make or break a show. Let's see what he had to say about *Smile* in the Gray Lady.

> *Schizoid in tone, dramatically diffuse and undistinguished in such crucial areas as music, dance and humor, this musical finally doesn't recall the winning entertainments of its chosen stylistic period so much as the perky, equally professional also-rans, like ''All American'' or ''Mr. President.''*[^6]

Ouch. Needless to say, revenues immediately plummeted. Let's display this by adding a label designating the release of *The New York Times* review.

```r
grosses_adj %>%
  filter(show == 'Smile') %>%
  ggplot(aes(x = week_ending, y = weekly_gross_adj)) +
  geom_line() +
  geom_point() +
  labs(title = 'Weekly Adjusted Gross Revenue for Smile on Broadway',
       x = 'Date',
       y = 'Adjusted Gross Revenue (March 2020)') +
  scale_x_date(labels = date_format("%m-%d-%Y")) +
  scale_y_continuous(labels = comma) +
  geom_vline(xintercept = ymd(19861125), color="darkred", linetype = "dashed") + # add dashed line to indicate review date
  annotate("text", x = ymd(19861129), y = 475000, label="NYT\nReview", size=4, color = "darkred") + # add 'NYT Review' text next to line
  theme_minimal()
```
![](/assets/images/talking-turkey/6.png)

What explains the uptick in the final weeks? *Smile* was cancelled with two weeks' notice, and attendance saw a slight resurgence as theatergoers knew it was their final chance to see the show.

### Percent Capacity

Next I'll make a similar graph displaying ```pct_capacity``` to see if it makes sense in context.

```r
grosses_adj %>%
  filter(show == 'Smile') %>%
  ggplot(aes(x = week_ending, y = pct_capacity)) + # graph pct_capacity instead of weekly_gross_adj
  geom_line() +
  geom_point() +
  labs(title = 'Percent of Theatre Capacity Sold for Smile on Broadway',
       x = 'Date',
       y = 'Percent Capacity') +
  scale_x_date(labels = date_format("%m-%d-%Y")) +
  theme_minimal()
```
![](/assets/images/talking-turkey/7.png)

As expected, the drop in percent capacity reflects the drop in adjusted gross revenue.

## Visualizing Multiple Variables

How do these metrics compare with those of other shows at the time? Next, I'll graph the ```pct_capacity``` for the top-five grossing shows running at the same time as *Smile*'s run, as well as that of *Smile*. 

```r
# pull list of weeks of Smile's run
smile_week_ending <- grosses_adj %>%
  filter(show == 'Smile') %>%
  pull(week_ending)
```
```r
# pull top five grossing shows during Smile's run
top_five_grossing <- grosses_adj %>%
  filter(week_ending %in% smile_week_ending) %>%
  group_by(show) %>%
  summarize(total_gross_adj = sum(weekly_gross_adj)) %>%
  arrange(desc(total_gross_adj)) %>%
  slice(1:5) %>%
  pull(show)
```
```r
grosses_adj %>%
  filter(show == 'Smile' | show %in% top_five_grossing, # filter for Smile or top five grossing shows
         week_ending %in% smile_week_ending) %>% # filter dates only during Smile's run
  ggplot(aes(x = week_ending, y = pct_capacity, color = show)) + # graph pct_capacity instead of weekly_gross_adj
  geom_line() +
  geom_point() +
  labs(title = 'Percent of Theatre Capacity Sold for Top-Five Grossing Broadway Shows',
       subtitle = 'During Run of Smile',
       x = 'Date',
       y = 'Percent Capacity',
       color = "Show") +
  scale_x_date(labels = date_format("%m-%d-%Y")) +
  theme_minimal()
```
![](/assets/images/talking-turkey/8.png)

### Adjusting the Legend

We've applied the format used in the last graph, but there is still room for improvement. By default, the order given in the legend is alphabetical, and it doesn't match up with the order of lines in the graph. This makes it somewhat difficult to read. Additionally, using a [color-blind friendly color-palette](http://www.cookbook-r.com/Graphs/Colors_(ggplot2)/#a-colorblind-friendly-palette) would improve accessibility.

```r
color_blind_palette <- c("#999999", "#E69F00", "#56B4E9", "#009E73", # color-blind friendly color palette
          "#F0E442", "#0072B2", "#D55E00", "#CC79A7") 
```
```r
grosses_adj %>%
  filter(show == 'Smile' | show %in% top_five_grossing,
         week_ending %in% smile_week_ending) %>%
  mutate(show = fct_reorder(show, pct_capacity, .desc = TRUE)) %>% # reorder shows based on median pct_capacity
  ggplot(aes(x = week_ending, y = pct_capacity, color = show)) +
  geom_line() +
  geom_point() +
  labs(title = 'Percent of Theatre Capacity Sold for Top-Five Grossing Broadway Shows',
       subtitle = 'During Run of Smile',
       x = 'Date',
       y = 'Percent Capacity',
       color = "Show") +
  scale_x_date(labels = date_format("%m-%d-%Y")) +
  scale_color_manual(values = color_blind_palette) + # apply color-blind friendly palette
  theme_minimal()
```
![](/assets/images/talking-turkey/9.png)

### Percent Capacity and Total Capacity

There's one last thing that could be deceptive about ```pct_capacity``` as the sole metric for this graph -- the fact that Broadway theater capacities can vary enormously.

```r
# pulls theatre and seats_in_theatre for smallest and largest Broadway theatres
grosses_adj %>%
  select(theatre, seats_in_theatre) %>%
  unique() %>%
  arrange(seats_in_theatre) %>%
  filter(seats_in_theatre > 0) %>% # 0 represents missing data
  slice(1, n())
```
```r
## # A tibble: 2 x 2
##   theatre          seats_in_theatre
##   <chr>                       <dbl>
## 1 Biltmore Theatre              483
## 2 Gershwin Theatre             1969
```

The largest theater on Broadway, the Gershwin Theatre, has a capacity more than four times that of the smallest theater, The Biltmore Theatre (now known as the Samuel J. Friedman Theatre). I'll display this information in the legend of my graph.

```r
grosses_adj %>%
  filter(show == 'Smile' | show %in% top_five_grossing,
         week_ending %in% smile_week_ending) %>%
  mutate(show_seats = fct_reorder(paste0(show, ' (', seats_in_theatre, ')'), pct_capacity, .desc = TRUE)) %>% # create new var show_seats that displays "show, (seats_in_theatre)"
  ggplot(aes(x = week_ending, y = pct_capacity, color = show_seats)) +
  geom_line() +
  geom_point() +
  labs(title = 'Percent of Theatre Capacity Sold for Top-Five Grossing Broadway Shows',
       subtitle = 'During Run of Smile',
       x = 'Date',
       y = 'Percent Capacity',
       color = "Show (Seats in Theatre)") +
  scale_x_date(labels = date_format("%m-%d-%Y")) +
  scale_color_manual(values = color_blind_palette) +
  theme_minimal()
```
![](/assets/images/talking-turkey/10.png)

Although there are a wide range of capacities in Broadway theaters in general, it appears that all of the theaters in this graph have similar capacity.

### Interpretation

*Cats* and *Me and My Girl* seem to follow similarly consistent trajectories while the movement of the rest of the shows appear to mirror each other. The original Broadway production of *Cats* is one of the most successful Broadway shows of all time, winning seven of 11 Tony award nominations and running for 18 years. The Broadway revival of *Me and My Girl* won three of 11 Tony Award nominations and ran for three years. The shows other than *Smile* were similarly distinguished in terms of Tony Awards and the duration of their runs. 

Further work could help explain the apparent gulf in percent capacity between *Cats* and *Me and My Girl* and the rest of the pack.

## Conclusion

Exploratory data analysis is fascinating and is easily my favorite part of data science. At its core, EDA is a form of creative storytelling, especially when used in conjunction with other qualitative sources. Whether you're a data science novice or an expert, or just a fan of musicals, I hope you've gotten something out of this post. I encourage you to check out the other datasets in the TidyTuesday Github, and analyses found on Twitter and elsewhere under [#TidyTuesday](https://twitter.com/hashtag/tidytuesday).

### Footnotes

[^1]: [Broadway turkey](https://english.stackexchange.com/questions/383665/broadway-turkey)

[^2]: [Broadway Grosses - Young Frankenstein](https://www.playbill.com/production/gross?production=00000150-aea3-d936-a7fd-eef771670004)

[^3]: [Variety Estimates Young Frankenstein Gross $1.5 Million for First Week](https://www.playbill.com/article/variety-estimates-young-frankenstein-gross-15-million-for-first-week-com-144779)

[^4]: [Discount Broadway Show Tickets](https://www.nytix.com/shows/in-previews)

[^5]: [Frank Rich](https://en.wikipedia.org/wiki/Frank_Rich)

[^6]: [THEATER: 'SMILE,' A MUSICAL COMEDY](https://www.nytimes.com/1986/11/25/theater/theater-smile-a-musical-comedy.html)