Mini Data Analysis Milestone 2
================

# Setup

Begin by loading the data and the packages that will be used in the
analysis below:

``` r
library(datateachr) # <- contain the data I picked!
library(tidyverse)
library(broom)
library(here)
library(cowplot)
```

# Task 1: Process and summarize the data

### 1.1 (1 point)

First, write out the 4 research questions you defined in milestone 1
were. This will guide your work through milestone 2:

<!-------------------------- Start your work below ---------------------------->

1.  *Explore the interannual variation of the river flow. Is there an
    upward or downward trend?*
2.  *Does the interannual variation of the river flow exhibit cyclical
    changes? Do the maximum and minimum values fluctuate synchronously?*
3.  *Study the relationship between seasons and river flow extremes. In
    which season does the flow generally reach its maximum or minimum?*
4.  *Is the interannual variation of flow extremes related to the season
    in which they occur each year? How does the distribution of extremes
    vary across different seasons?*

To make my analysis of the questions easier and more in line with the
requirements, I adjusted the order of my questions a bit, but did not
change the questions I defined in milestone1 overall.
<!----------------------------------------------------------------------------->

### 1.2 (8 points)

Now, for each of your four research questions, choose one task from
options 1-4 (summarizing), and one other task from 4-8 (graphing). You
should have 2 tasks done for each research question (8 total). Make sure
it makes sense to do them! (e.g. don’t use a numerical variables for a
task that needs a categorical variable.). Comment on why each task helps
(or doesn’t!) answer the corresponding research question.

Ensure that the output of each operation is printed!

Also make sure that you’re using dplyr and ggplot2 rather than base R.
Outside of this project, you may find that you prefer using base R
functions for certain tasks, and that’s just fine! But part of this
project is for you to practice the tools we learned in class, which is
dplyr and ggplot2.

**Summarizing:**

1.  Compute the *range*, *mean*, and *two other summary statistics* of
    **one numerical variable** across the groups of **one categorical
    variable** from your data.
2.  Compute the number of observations for at least one of your
    categorical variables. Do not use the function `table()`!
3.  Create a categorical variable with 3 or more groups from an existing
    numerical variable. You can use this new variable in the other
    tasks! *An example: age in years into “child, teen, adult, senior”.*
4.  Compute the proportion and counts in each category of one
    categorical variable across the groups of another categorical
    variable from your data. Do not use the function `table()`!

**Graphing:**

6.  Create a graph of your choosing, make one of the axes logarithmic,
    and format the axes labels so that they are “pretty” or easier to
    read.
7.  Make a graph where it makes sense to customize the alpha
    transparency.

Using variables and/or tables you made in one of the “Summarizing”
tasks:

8.  Create a graph that has at least two geom layers.
9.  Create 3 histograms, with each histogram having different sized
    bins. Pick the “best” one and explain why it is the best.

Make sure it’s clear what research question you are doing each operation
for!

<!------------------------- Start your work below ----------------------------->

**1. *Explore the interannual variation of the river flow. Is there an
upward or downward trend?***

**Summarizing:**

We begin by understanding the range of years available in the dataset:

``` r
# Print the earliest year in the 'flow_sample' dataset
print(min(flow_sample$year))
```

    ## [1] 1909

``` r
# Print the latest year in the 'flow_sample' dataset
print(max(flow_sample$year))
```

    ## [1] 2018

``` r
# Print the total number of unique years in the 'flow_sample' dataset
print(length(unique(flow_sample$year)))
```

    ## [1] 109

We can see that range of years is extensive, making it challenging to
discern the interannual variation trend. Now we divide the
years(numerical variable) into decades(categorical variable) to
facilitate easier trend analysis:

``` r
flow_decade <- flow_sample %>%
  mutate(decade = case_when(
    year >= 1900 & year < 1910 ~ '1900s',
    year >= 1910 & year < 1920 ~ '1910s',
    year >= 1920 & year < 1930 ~ '1920s',
    year >= 1930 & year < 1940 ~ '1930s',
    year >= 1940 & year < 1950 ~ '1940s',
    year >= 1950 & year < 1960 ~ '1950s',
    year >= 1960 & year < 1970 ~ '1960s',
    year >= 1970 & year < 1980 ~ '1970s',
    year >= 1980 & year < 1990 ~ '1980s',
    year >= 1990 & year < 2000 ~ '1990s',
    year >= 2000 & year < 2010 ~ '2000s',
    year >= 2010 & year < 2020 ~ '2010s',
    TRUE ~ 'Other'  # To ensure that all years are classified
  ))

# a quick preview of the dataset
head(flow_decade)
```

    ## # A tibble: 6 × 8
    ##   station_id  year extreme_type month   day  flow sym   decade
    ##   <chr>      <dbl> <chr>        <dbl> <dbl> <dbl> <chr> <chr> 
    ## 1 05BB001     1909 maximum          7     7   314 <NA>  1900s 
    ## 2 05BB001     1910 maximum          6    12   230 <NA>  1910s 
    ## 3 05BB001     1911 maximum          6    14   264 <NA>  1910s 
    ## 4 05BB001     1912 maximum          8    25   174 <NA>  1910s 
    ## 5 05BB001     1913 maximum          6    11   232 <NA>  1910s 
    ## 6 05BB001     1914 maximum          6    18   214 <NA>  1910s

However, I found that there’s only one entry for the ‘1900s’. Given its
lack of representativeness, I’ve decided to exclude it from the analysis
to ensure accuracy in the average comparisons.

``` r
print(unique(filter(flow_decade, decade == '1900s')$year))
```

    ## [1] 1909

Now we compute the mean of *flow* across the groups of *decade*.(For
each year, there are only two flow data points: maximum and minimum.
Thus, there’s no need to calculate the median or other summary
statistics.)

``` r
# Print the average flow data for each decade (excluding 1900s)
decade_avg_flow <- flow_decade %>%
  filter(decade != "1900s") %>% # exclude the 1900s in this analysis
  group_by(decade) %>%
  summarise(avg_flow = mean(flow, na.rm = TRUE))

# Print the average flow data for each decade (excluding 1900s)
print(decade_avg_flow)
```

    ## # A tibble: 11 × 2
    ##    decade avg_flow
    ##    <chr>     <dbl>
    ##  1 1910s     127. 
    ##  2 1920s     115. 
    ##  3 1930s     116. 
    ##  4 1940s      92.0
    ##  5 1950s     117. 
    ##  6 1960s     116. 
    ##  7 1970s     107. 
    ##  8 1980s     106. 
    ##  9 1990s      99.9
    ## 10 2000s      94.0
    ## 11 2010s     113.

**Graphing:**

The following code uses the ggplot2 package to create a line and scatter
plot illustrating the average flow by decade:

``` r
p <- ggplot(decade_avg_flow, aes(x = decade, y = avg_flow)) +
  geom_line(aes(group = 1), color = "skyblue") +   # Add a line geom layer to represent the trend of average flow over the decades
  geom_point(color = "lightcoral", size = 3) +  # Add a point geom layer to emphasize individual decade values
  labs(title = "Average Flow by Decade",
       x = "Decade",
       y = "Average Flow") +
  theme_minimal() + 
  theme(axis.text.x = element_text(angle = 45, hjust = 1))  # Adjust the x-axis labels for better readability

# Display the graph
print(p)
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

By summarizing and graphing, we can observe that the average flow for
each decade fluctuates, making it challenging to discern any clear
upward or downward trend.

**2. *Does the interannual variation of the river flow exhibit cyclical
changes? Do the maximum and minimum values fluctuate synchronously?***

**Summarizing:**

To observe whether the flow changes exhibit a cyclical pattern, visual
representation through graphs is more effective than summarizing the
data. Additionally, the dataset I picked does not have many categorical
variables, and there’s no compelling reason to artificially create new
ones for this specific question. Therefore, I did not perform a
summarization task for this particular question. However, I have
incorporated extra-required summarization in my analyses of other
questions, and I hope that can be taken into consideration for the score
of this question.

**Graphing:**

For each year, the flow data consists of only two distinct values: the
maximum and the minimum. Given these extremes, I think it’s essential to
study them separately.

I utilized both *geom_line* and *geom_point* layers in ggplot to depict
the interannual variations of river flow. It’s noteworthy that the
differences between the maximum and minimum values are substantial,
making them unsuitable for representation on a single coordinate system.
Thus, I chose to create separate plots for them. To present these plots
cohesively, I employed the *plot_grid* function from the *cowplot*
package to combine them.

``` r
# Create a graph for the interannual variation of maximum flow
max_year_flow_graph <- flow_sample %>% 
                   filter(extreme_type == 'maximum' & !is.na(flow)) %>%
                   ggplot(aes(year, flow)) +
                   geom_line() + 
                   geom_point(colour = "cornflowerblue") + 
                   ggtitle("Interannual Variation of Maximum Flow") +
                   xlab("Year") +
                   ylab("Flow") +
                   theme_bw()

# Create a graph for the interannual variation of minimum flow
min_year_flow_graph <- flow_sample %>% 
                   filter(extreme_type == 'minimum' & !is.na(flow)) %>%
                   ggplot(aes(year, flow)) +
                   geom_line() + 
                   geom_point(colour = "salmon") + 
                   ggtitle("Interannual Variation of Minimum Flow") +
                   xlab("Year") +
                   ylab("Flow") +
                   theme_bw()

# Combine the two graphs vertically in a single plot with specified proportions for their heights
combined_graph <- plot_grid(max_year_flow_graph, min_year_flow_graph, align="hv", ncol=1, rel_heights=c(3/5, 2/5))

# Display the result
print(combined_graph)
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

Based on the visual representations of the interannual variation of
maximum and minimum river flows, we can observe some cyclical patterns
in the data. The maximum flow shows periodic spikes, particularly
evident around 1925, 1950, 1975, and just after 2000. Similarly, the
minimum flow also seems to exhibit some periodicity, albeit at a
different scale and less pronounced than the maximum flow. When
comparing the two charts, it appears that the maximum and minimum values
do not always fluctuate synchronously. While there are periods where
they seem to follow a similar trend, there are also instances where they
diverge. This indicates that the factors influencing the peak and low
flows might not always be the same or might have different magnitudes of
impact.

**3. *Study the relationship between seasons and river flow extremes. In
which season does the flow generally reach its maximum or minimum?***

**Summarizing:**

Let’s first divide the month(numerical variable) into season(categorical
variable).

To ensure that the seasons are displayed on the x-axis in the order of
spring, summer, fall, and winter, and to ensure that a season still
appears even when there’s no data for it, I explicitly set the *season*
as a factor in the plot.

``` r
# create a new data object named season_flow_sample and make modifications to it
season_flow_sample <- flow_sample %>% 
                      mutate(season = case_when(month >= 3 & month < 6 ~ "Spring",
                                                month >= 6 & month < 9 ~ "Summer",
                                                month >= 9 & month < 12 ~ "Fall",
                                                TRUE ~ "Winter"
                                                ))%>%
                      mutate(season = factor(season, levels = c("Spring", "Summer","Fall","Winter")))

# remove the data points with no season value
season_flow_sample_omit <- season_flow_sample[!is.na(season_flow_sample$season),]

# show the modified dataset
head(season_flow_sample_omit)
```

    ## # A tibble: 6 × 8
    ##   station_id  year extreme_type month   day  flow sym   season
    ##   <chr>      <dbl> <chr>        <dbl> <dbl> <dbl> <chr> <fct> 
    ## 1 05BB001     1909 maximum          7     7   314 <NA>  Summer
    ## 2 05BB001     1910 maximum          6    12   230 <NA>  Summer
    ## 3 05BB001     1911 maximum          6    14   264 <NA>  Summer
    ## 4 05BB001     1912 maximum          8    25   174 <NA>  Summer
    ## 5 05BB001     1913 maximum          6    11   232 <NA>  Summer
    ## 6 05BB001     1914 maximum          6    18   214 <NA>  Summer

**Graphing:**

The goal of creating histograms is to visually understand the
distribution and frequency of data. Here, we’ve presented three
histograms with different focus areas: average flow per season, average
flow per month, and the distribution of flow extremes per season.

**Histogram1: Average Flow per Season**

This histogram provides an overview of the average flow values for each
season. By looking at this, one can quickly understand the season that
experiences the highest and the lowest average flow. However, it might
lack the depth needed to understand extreme flow variations.

``` r
# Histogram1: Average Flow per Season
p1 <- ggplot(season_flow_sample_omit, aes(x = season, y = flow)) +
  stat_summary(fun = mean, geom = "col", fill = "lightgray") +
  labs(title = "Average Flow per Season") +
  ylab("Average Flow")+
  theme_bw()
p1
```

    ## Warning: Removed 2 rows containing non-finite values (`stat_summary()`).

![](mini-project-2_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

**Histogram2: Average Flow per Month**

This histogram provides an overview of the average flow values for every
month. It provides a granular view of flow changes for each month but
might not be as apt for observing seasonal changes.

``` r
# Histogram2: Average Flow per Month
p2 <- ggplot(season_flow_sample_omit, aes(x = month, y = flow)) +
  stat_summary(fun = mean, geom = "col", fill = "khaki") +
  labs(title = "Average Flow per Month") +
  ylab("Average Flow")+
  theme_bw()
p2
```

    ## Warning: Removed 2 rows containing non-finite values (`stat_summary()`).

![](mini-project-2_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->
**Histogram3: Seasonal River Flow Extremes**

This histogram presents the extreme flow conditions for each season,
differentiating between maximum and minimum flows. This is particularly
useful for studying the relationship between seasons and river flow
extremes. From this chart, it’s clear in which seasons the flow
generally reaches its maximum or minimum. Therefore, I think this
histogram is the best of 3.

``` r
# Histogram3: Seasonal River Flow Extremes
p3 <- ggplot(season_flow_sample_omit,aes(x = season, fill = extreme_type))+
      geom_bar(position = position_dodge(preserve = "single")) +
      labs(title = "Distribution of Flow Extremes per Season") +
      ylab("Count")+
      theme_bw()
p3
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

We can observe from the graph that the river generally reaches its
maximum flow in the summer and its minimum flow in the winter. The
transitional seasons, spring and fall, show mixed tendencies, with
spring leaning more towards maximum flows and fall exhibiting fewer
extremes overall.

**4. *Is the interannual variation of flow extremes related to the
season in which they occur each year? How does the distribution of
extremes vary across different seasons?***

**Summarizing:**

The summarizing section below meets the requirement of the task: Compute
the proportion and counts in each category of one categorical variable
across the groups of another categorical variable from your data.

Again, Let’s study the maximum flow extremes and minimum flow extremes
separately.

**Maximum Flow Extremes:**

First, calculate the average of the maximum flow, then identify the
number of times the flow exceeds this average in each season. Next, we
tally the total count of maximum flow extremes for each season. Finally,
combining the data from the two previous steps, compute the proportion
of maximum flow extremes that exceed the average in each season.

``` r
# calculate the mean of the flow values where the extreme_type is 'maximum'
mean_max_flow <- mean(filter(season_flow_sample_omit, extreme_type == 'maximum')$flow, na.rm = TRUE)

# identify the number of maximum flow extremes in each season that are greater than the calculated mean
max_counts_over_mean <- season_flow_sample_omit %>%
                           filter(extreme_type == 'maximum') %>%
                           filter(flow > mean_max_flow) %>%
                           group_by(season) %>%
                           summarise(count_over_mean = n())

# determine the total number of maximum flow extremes for each season
max_season_counts <- season_flow_sample_omit %>%
                 filter(extreme_type == 'maximum') %>%
                 group_by(season) %>%
                 summarise(count_total = n())

# Using left_join to combine the above two datasets based on the season. 
max_combined_counts <- left_join(max_counts_over_mean, max_season_counts, by = "season", suffix = c("_over_mean", "_total"))

# compute the proportion of maximum flow extremes in each season that surpass the mean
max_result <- max_combined_counts %>%
          mutate(prop = count_over_mean / count_total)

# display the result
print(max_result)
```

    ## # A tibble: 2 × 4
    ##   season count_over_mean count_total  prop
    ##   <fct>            <int>       <int> <dbl>
    ## 1 Spring               3          16 0.188
    ## 2 Summer              44          93 0.473

Based on the summary table above, the interannual variation of maximum
extremes of flow appears to be notably influenced by the season,
especially in summer, where around 47.31% of the total observed extremes
are above the mean. Conversely, in spring, only 18.75% of the total
recorded extremes surpass the average, indicating a stronger seasonal
pattern during the summer months.

**Minimum Flow Extremes:**

The analysis for the minimum flow extremes is analogous to that of the
maximum.

``` r
# calculate the mean of the flow values where the extreme_type is 'minimum'
mean_min_flow <- mean(filter(season_flow_sample_omit, extreme_type == 'minimum')$flow, na.rm = TRUE)

# identify the number of minimum flow extremes in each season that are greater than the calculated mean
min_counts_over_mean <- season_flow_sample_omit %>%
                           filter(extreme_type == 'minimum') %>%
                           filter(flow > mean_min_flow) %>%
                           group_by(season) %>%
                           summarise(count_over_mean = n())

# determine the total number of minimum flow extremes for each season
min_season_counts <- season_flow_sample_omit %>%
                 filter(extreme_type == 'minimum') %>%
                 group_by(season) %>%
                 summarise(count_total = n())

# Using left_join to combine the above two datasets based on the season. 
min_combined_counts <- left_join(min_counts_over_mean, min_season_counts, by = "season", suffix = c("_over_mean", "_total"))

# compute the proportion of minimum flow extremes in each season that surpass the mean
min_result <- min_combined_counts %>%
          mutate(prop = count_over_mean / count_total)

# display the result
print(min_result)
```

    ## # A tibble: 3 × 4
    ##   season count_over_mean count_total  prop
    ##   <fct>            <int>       <int> <dbl>
    ## 1 Spring              23          45 0.511
    ## 2 Fall                 1           5 0.2  
    ## 3 Winter              25          59 0.424

Based on the summary table above, I don’t believe the interannual
variation of minimum extremes of flow is significantly influenced by the
seasons in which they occur each year. Notably, the proportions of
minimum flow extremes in Spring and Winter that exceed the mean are
quite similar. However, there seems to be a lack of data for the Fall
season.

**Graphing:**

The two boxplots below contain multiple geom layers. In fact, this was
already achieved in milestone1. In this report, after adjusting the
structure of the dataset, the graphics have become more concise and
readable. Here is my analysis of the graphical results:

For the distribution across seasons:

- Spring: This season seems to have a relatively wide range of flow
  extremes and some outliers, suggesting it might experience sporadic
  extreme flow events.

- Summer: Has higher median flow values in the first plot and is absent
  in the second. The presence of outliers indicates occasional extreme
  events.

- Fall: In the second plot, fall has a lower median flow and fewer
  outliers, indicating less extreme variation.

- Winter: The flow extremes are relatively higher than fall but lower
  than spring. There are some outliers, indicating occasional extreme
  flow events.

``` r
season_flow_max_graph <- season_flow_sample_omit %>%
                         filter(extreme_type == "maximum") %>%
                         ggplot(aes(x=season,y=flow, fill=season)) +
                         geom_boxplot(outlier.shape=NA, width=0.4) + 
                         geom_jitter(width=0.1, size=0.15, alpha=0.5)+
                         scale_fill_manual(values=c("lightgreen","aquamarine","orange","snow")) +
                         scale_x_discrete(drop = FALSE) +
                         ggtitle("Seasonal Maximum Flow Variation Boxplot")+
                         xlab("Season") + 
                         ylab("Flow") + 
                         theme_bw()
# present the plot
print(season_flow_max_graph)
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

``` r
season_flow_min_graph <- season_flow_sample_omit %>%
                         filter(extreme_type == "minimum") %>%
                         ggplot(aes(x=season,y=flow, fill=season)) +
                         geom_boxplot(outlier.shape=NA, width=0.4) + 
                         geom_jitter(width=0.1, size=0.15, alpha=0.5)+
                         scale_fill_manual(values=c("lightgreen","aquamarine","orange","snow")) +
                         scale_x_discrete(drop = FALSE) +
                         ggtitle("Seasonal Minimum Flow Variation Boxplot")+
                         xlab("Season") + 
                         ylab("Flow") + 
                         theme_bw()
# present the plot
print(season_flow_min_graph)
```

    ## Warning: Removed 2 rows containing non-finite values (`stat_boxplot()`).

    ## Warning: Removed 2 rows containing missing values (`geom_point()`).

![](mini-project-2_files/figure-gfm/unnamed-chunk-14-2.png)<!-- -->

<!----------------------------------------------------------------------------->

### 1.3 (2 points)

Based on the operations that you’ve completed, how much closer are you
to answering your research questions? Think about what aspects of your
research questions remain unclear. Can your research questions be
refined, now that you’ve investigated your data a bit more? Which
research questions are yielding interesting results?

<!------------------------- Write your answer here ---------------------------->

I believe I am now very familiar with the dataset I am studying. My
ultimate goal is to decipher the temporal variation in flows, delving
into the relationship between flow and other temporal determinants like
the year, season, and date. Aside from being unable to ascertain the
trend of flow changes, I feel confident in my understanding of the other
issues.

What I find both intriguing and valuable in my research findings is that
river flow typically peaks in the summer, with occasional peaks in the
spring. More often than not, the flow hits its minimum during the winter
and spring, with rare instances in the fall. Additionally, extreme
values of river flow exhibit periodic fluctuations, and these
oscillations are quite frequent. A more detailed analysis can be seen in
the above section.

Regrettably, the dataset has limited variables, which means my analysis
can only scratch the surface. I believe there are numerous other factors
like climate and topography that merit consideration. Incorporating
these would likely yield even more fascinating insights.

<!----------------------------------------------------------------------------->

# Task 2: Tidy your data

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

- Each row is an **observation**
- Each column is a **variable**
- Each cell is a **value**

### 2.1 (2 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

``` r
head(season_flow_sample_omit)
```

    ## # A tibble: 6 × 8
    ##   station_id  year extreme_type month   day  flow sym   season
    ##   <chr>      <dbl> <chr>        <dbl> <dbl> <dbl> <chr> <fct> 
    ## 1 05BB001     1909 maximum          7     7   314 <NA>  Summer
    ## 2 05BB001     1910 maximum          6    12   230 <NA>  Summer
    ## 3 05BB001     1911 maximum          6    14   264 <NA>  Summer
    ## 4 05BB001     1912 maximum          8    25   174 <NA>  Summer
    ## 5 05BB001     1913 maximum          6    11   232 <NA>  Summer
    ## 6 05BB001     1914 maximum          6    18   214 <NA>  Summer

- station_id: Each cell contains a unique identifier for an observation
  station, making it a variable. Thus, it is tidy.
- year: Each cell contains a year, which is a variable. Therefore, it is
  tidy.
- extreme_type: Each cell denotes an “extreme type,” such as maximum or
  minimum. This also acts as a variable, so it is tidy.
- month: Representing a month, where each cell has a month value, it is
  tidy.
- day: Represents a day, and since each cell holds a day value, it is
  tidy.
- flow: This represents the observation values of flow, and each cell
  holds a flow value. Hence, it is tidy.
- sym: While this column might not hold meaningful information(for my
  study), it still satisfies the definition of tidy data because each
  cell is a value.
- season: Representing a season, each cell contains a season value,
  making it tidy.

From the above analysis, it’s evident that each row is an observation,
each column is a variable, and each cell holds a singular value. As a
result, based on the principles of tidy data, we can conclude that the
*season_flow_sample_omit* dataset is tidy.

<!----------------------------------------------------------------------------->

### 2.2 (4 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

**Untidying the data:**

Let’s make the data untidy by uniting the year, month, and day columns
into a single column:

``` r
untidy_data <- season_flow_sample_omit %>%
  unite("date", year, month, day, sep="-")
# display the result
head(untidy_data)
```

    ## # A tibble: 6 × 6
    ##   station_id date      extreme_type  flow sym   season
    ##   <chr>      <chr>     <chr>        <dbl> <chr> <fct> 
    ## 1 05BB001    1909-7-7  maximum        314 <NA>  Summer
    ## 2 05BB001    1910-6-12 maximum        230 <NA>  Summer
    ## 3 05BB001    1911-6-14 maximum        264 <NA>  Summer
    ## 4 05BB001    1912-8-25 maximum        174 <NA>  Summer
    ## 5 05BB001    1913-6-11 maximum        232 <NA>  Summer
    ## 6 05BB001    1914-6-18 maximum        214 <NA>  Summer

The *unite* function from *tidyr* takes multiple columns and merges them
into a single column. In this case, we’ve merged year, month, and day
into a new column named “date”, making the dataset untidy.

**Tidying the data back:**

``` r
tidy_data <- untidy_data %>%
  separate(date, into = c("year", "month", "day"), sep="-")
# display the result
head(tidy_data)
```

    ## # A tibble: 6 × 8
    ##   station_id year  month day   extreme_type  flow sym   season
    ##   <chr>      <chr> <chr> <chr> <chr>        <dbl> <chr> <fct> 
    ## 1 05BB001    1909  7     7     maximum        314 <NA>  Summer
    ## 2 05BB001    1910  6     12    maximum        230 <NA>  Summer
    ## 3 05BB001    1911  6     14    maximum        264 <NA>  Summer
    ## 4 05BB001    1912  8     25    maximum        174 <NA>  Summer
    ## 5 05BB001    1913  6     11    maximum        232 <NA>  Summer
    ## 6 05BB001    1914  6     18    maximum        214 <NA>  Summer

The *separate* function from *tidyr* does the reverse of unite. It takes
a single column and splits it into multiple columns. Here, we’re
splitting “date” back into “year”, “month”, and “day”.

**Explanation:**

Untidying Reasoning:

By merging the year, month, and day columns into a single “date” column,
we’ve taken tidy data (where each column is a variable) and made it less
tidy. Now, instead of having distinct variables in separate columns, we
have multiple pieces of information combined in the “date” column. By
doing this, the dataset becomes more readable and easier to understand.

Tidying Reasoning:

By separating the “date” column back into its original components, we’re
adhering to the principles of tidy data where each variable forms a
column. This makes the data easier to analyze.
<!----------------------------------------------------------------------------->

### 2.3 (4 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the remaining tasks:

<!-------------------------- Start your work below ---------------------------->

1.  *Explore the interannual variation of the river flow. Is there an
    upward or downward trend?*
2.  *Is the interannual variation of flow extremes related to the season
    in which they occur each year? How does the distribution of extremes
    vary across different seasons?*

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

The reason I chose the first question is that the analysis I’ve done so
far isn’t sufficient for me to arrive at research conclusions. The data
fluctuates, and I cannot discern a clear overall upward or downward
trend. In Task3, I will use a linear regression model to address this
issue.

In fact, I believe I already have the answer to the second question.
However, I chose it because my analysis might not be comprehensive
enough, perhaps due to a lack of statistical knowledge, or maybe because
the dataset is not comprehensive enough.
<!----------------------------------------------------------------------------->

<!--------------------------- Start your work below --------------------------->

# Task 3: Modelling

## 3.0 (no points)

Pick a research question from 1.2, and pick a variable of interest
(we’ll call it “Y”) that’s relevant to the research question. Indicate
these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: *Explore the interannual variation of the river
flow. Is there an upward or downward trend?*

**Variable of interest**: *flow*

<!----------------------------------------------------------------------------->

## 3.1 (3 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

- **Note**: It’s OK if you don’t know how these models/tests work. Here
  are some examples of things you can do here, but the sky’s the limit.

  - You could fit a model that makes predictions on Y using another
    variable, by using the `lm()` function.
  - You could test whether the mean of Y equals 0 using `t.test()`, or
    maybe the mean across two groups are different using `t.test()`, or
    maybe the mean across multiple groups are different using `anova()`
    (you may have to pivot your data for the latter two).
  - You could use `lm()` to test for significance of regression
    coefficients.

<!-------------------------- Start your work below ---------------------------->

Let’s fit the linear model:

``` r
flow_model <- lm(flow ~ year, data = season_flow_sample_omit)
summary(flow_model)
```

    ## 
    ## Call:
    ## lm(formula = flow ~ year, data = season_flow_sample_omit)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -117.03 -102.97    8.45   92.11  367.80 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)
    ## (Intercept) 583.1516   479.7697   1.215    0.226
    ## year         -0.2409     0.2443  -0.986    0.325
    ## 
    ## Residual standard error: 112 on 214 degrees of freedom
    ##   (因为不存在，2个观察量被删除了)
    ## Multiple R-squared:  0.004523,   Adjusted R-squared:  -0.0001287 
    ## F-statistic: 0.9723 on 1 and 214 DF,  p-value: 0.3252

<!----------------------------------------------------------------------------->

## 3.2 (3 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

- Be sure to indicate in writing what you chose to produce.
- Your code should either output a tibble (in which case you should
  indicate the column that contains the thing you’re looking for), or
  the thing you’re looking for itself.
- Obtain your results using the `broom` package if possible. If your
  model is not compatible with the broom function you’re needing, then
  you can obtain your results by some other means, but first indicate
  which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

From the fitted model, the regression coefficient for year will tell us
about the annual trend in river flow. If the coefficient is positive, it
suggests an upward trend, and if it’s negative, it suggests a downward
trend.

``` r
tidy_results <- tidy(flow_model)
tidy_results
```

    ## # A tibble: 2 × 5
    ##   term        estimate std.error statistic p.value
    ##   <chr>          <dbl>     <dbl>     <dbl>   <dbl>
    ## 1 (Intercept)  583.      480.        1.22    0.226
    ## 2 year          -0.241     0.244    -0.986   0.325

Through the linear regression model, we can conclude that there is a
downward trend in the interannual variation of the river flow, but the
trend is very slight.

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 4.1 (3 points)

Take a summary table that you made from Task 1, and write it as a csv
file in your `output` folder. Use the `here::here()` function.

- **Robustness criteria**: You should be able to move your Mini Project
  repository / project folder to some other location on your computer,
  or move this very Rmd file to another location within your project
  repository / folder, and your code should still work.
- **Reproducibility criteria**: You should be able to delete the csv
  file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
# Define the file path using the here() function
file_path <- here::here("output", "decade_avg_flow.csv")

# Write the data frame to the CSV file
write.csv(decade_avg_flow, file_path, row.names = FALSE)

# List the files in the 'output' directory
files_in_output <- list.files(here::here("output"))
print(files_in_output)
```

    ## [1] "decade_avg_flow.csv" "flow_model.rds"

We can see that we’ve obtained “decade_avg_flow.csv” in output
directory.

<!----------------------------------------------------------------------------->

## 4.2 (3 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

- The same robustness and reproducibility criteria as in 4.1 apply here.

<!-------------------------- Start your work below ---------------------------->

Write the model object to an RDS file:

``` r
# Define the RDS file path using the here() function
rds_path <- here::here("output", "flow_model.rds")

# Save the model object to the RDS file
saveRDS(flow_model, rds_path)
```

Load the model object from the RDS file:

``` r
# Read the model object from the RDS file
loaded_flow_model <- readRDS(rds_path)

# Display the result
summary(loaded_flow_model)
```

    ## 
    ## Call:
    ## lm(formula = flow ~ year, data = season_flow_sample_omit)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -117.03 -102.97    8.45   92.11  367.80 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)
    ## (Intercept) 583.1516   479.7697   1.215    0.226
    ## year         -0.2409     0.2443  -0.986    0.325
    ## 
    ## Residual standard error: 112 on 214 degrees of freedom
    ##   (因为不存在，2个观察量被删除了)
    ## Multiple R-squared:  0.004523,   Adjusted R-squared:  -0.0001287 
    ## F-statistic: 0.9723 on 1 and 214 DF,  p-value: 0.3252

<!----------------------------------------------------------------------------->

# Overall Reproducibility/Cleanliness/Coherence Checklist

Here are the criteria we’re looking for.

## Coherence (0.5 points)

The document should read sensibly from top to bottom, with no major
continuity errors.

The README file should still satisfy the criteria from the last
milestone, i.e. it has been updated to match the changes to the
repository made in this milestone.

## File and folder structure (1 points)

You should have at least three folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (1 point)

All output is recent and relevant:

- All Rmd files have been `knit`ted to their output md files.
- All knitted md files are viewable without errors on Github. Examples
  of errors: Missing plots, “Sorry about that, but we can’t show files
  that are this big right now” messages, error messages from broken R
  code
- All of these output files are up-to-date – that is, they haven’t
  fallen behind after the source (Rmd) files have been updated.
- There should be no relic output files. For example, if you were
  knitting an Rmd to html, but then changed the output to be only a
  markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

## Tagged release (0.5 point)

You’ve tagged a release for Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
