---
title: "Gender and Representation in LEGO"
layout: post
date: 2018-03-19 17:00
headerImage: false
tag:
- R
- STA199
projects: false
category: blog
author: maxbartlett
description: "Analysis of the differences between LEGO toys marketed to boys and girls."
--- 
**This was completed as a [homework assignment](https://www2.stat.duke.edu/courses/Spring18/Sta199/hw/hw-04.html) for [STA199: Introduction to Data Science](https://www2.stat.duke.edu/courses/Spring18/Sta199/) at Duke University. The LEGO dataset can be found [here](https://github.com/seankross/lego).**

In 2012, Lego released their Lego Friends line of toys, primarily marketed towards young girls. The toys [drew the ire of the public and media](https://www.theatlantic.com/entertainment/archive/2016/05/legos/484115/), that claimed that they were dumbed-down versions of the Lego toys marketed to boys. Michael McNally, a Lego spokesman claimed that "just about piece for piece, there are just as many pieces required to put something together [among Friends sets].” Does this claim hold up?

To find the answer, I first examined how Lego Friends sets compare to past Lego sets marketed to girls, as well as how they compared to current sets marketed to boys.

I grouped all of the girls' sets and took some of the most popular boys' sets to compare against the Friends set.

```r
Girls <- c("Homemaker", "Paradisa", "Scala", "Belville")
boys <- c("Star Wars", "Lord of the Rings", "Bionicle", "Vikings")
legosets_gender <-
  legosets %>%
  subset(Availability = c("Retail", "Retail - limited", "Not specified", "Unknown"),
         !is.na(Pieces) & Pieces > 0) %>%
  mutate(Gender_and_Friends = 
           case_when(
             Theme %in% girls ~ "Girls", #Does not include Friends set
             Theme %in% boys ~ "Boys",
             Theme == "Friends" ~ "Friends",
             TRUE ~ "Unspecified"
           )
  )
```

```r
## # A tibble: 6,058 x 15
##    Item_Number Name   Year Theme Subtheme Pieces Minifigures
##    <chr>       <chr> <int> <chr> <chr>     <int>       <int>
##  1 10246       Dete…  2015 Adva… Modular…   2262           6
##  2 10247       Ferr…  2015 Adva… Fairgro…   2464          10
##  3 10248       Ferr…  2015 Adva… Vehicles   1158          NA
##  4 10249       Toy …  2015 Adva… Winter …    898          NA
##  5 10581       Ducks  2015 Duplo Forest …     13           1
##  6 10582       Anim…  2015 Duplo Forest …     39           2
##  7 10583       Fish…  2015 Duplo Forest …     32           2
##  8 10584       Fore…  2015 Duplo Forest …    105           3
##  9 10585       Mom …  2015 Duplo ""           13           2
## 10 10586       Ice …  2015 Duplo ""           11           2
## # ... with 6,048 more rows, and 8 more variables: Image_URL <chr>,
## #   GBP_MSRP <dbl>, USD_MSRP <dbl>, CAD_MSRP <dbl>, EUR_MSRP <dbl>,
## #   Packaging <chr>, Availability <chr>, Gender_and_Friends <chr>
```

Next, I plotted the data using a barchart.

```r
legosets_gender %>%
  subset(Gender_and_Friends != "Unspecified") %>%
  ggplot(aes(x = reorder(Gender_and_Friends, Pieces, FUN=median), y = Pieces)) +
  geom_boxplot() + 
  coord_cartesian(ylim = c(0, 1000)) +
  labs(Title = "Lego Pieces in Girls', Boys' and Lego Friends sets", x = "Gender and Friends Category", y = "Number of Pieces")
```

![gender-graph-1](../assets/blog/gender-and-representation-in-lego/plot-gender-1.png)

Finally, I displayed the summary statistics of each of the sets.

```r
legosets_gender %>%
  subset(Gender_and_Friends != "Unspecified") %>%
  group_by(Gender_and_Friends) %>%
  summarize(mean_pieces = mean(Pieces), 
            median_pieces = median(Pieces),
            min_pieces = min(Pieces), 
            q1 = quantile(Pieces, .25), 
            q3 = quantile(Pieces, .75), 
            max_pieces = max(Pieces)) %>%
  arrange(desc(median_pieces))
```

```r
## # A tibble: 3 x 7
##   Gender_and_Frie… mean_pieces median_pieces min_pieces    q1    q3
##   <chr>                  <dbl>         <dbl>      <dbl> <dbl> <dbl>
## 1 Boys                   304.          116            1    47  380 
## 2 Friends                200.           98            2    43  253 
## 3 Girls                   87.8          65.5          2    36  118.
## # ... with 1 more variable: max_pieces <dbl>
```

While the sample of sets used is not necessarily representative of all sets marketed to girls and boys, my simple analysis has shown that, while Lego Friends is an improvement on past Lego brands marketed to girls (I'm looking at you, Lego Homemaker), the average Friends set is not as structurally complex as the average set marketed to boys. In my opinion, Lego should make their girls' sets more intricate, or, better yet, eliminate gender-based marketing entirely.