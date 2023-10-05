Mini Data-Analysis Deliverable 1
================

# Introduction

This is my very first data analysis project, and it’s also a deliverable
for Mini-Data Analysis Milestone 1 in STAT 545A.

In this deliverable, I selected a dataset after evaluating several
options, created a subdataset, made modifications for further research,
and examined the relationships between certain variables. Based on this
analysis, I have formulated four questions that I would like to explore
further in Milestone 2.

For the sake of convenient grading, I have kept the relevant guidance
and questions.

# Task0: Load needed packages

1.  Install the [`datateachr`](https://github.com/UBC-MDS/datateachr)
    package by typing the following into **R terminal**:

<!-- -->

    install.packages("devtools")
    devtools::install_github("UBC-MDS/datateachr")

2.  Load the packages below.

``` r
library(datateachr)
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

# Task 1: Choose a dataset

The `datateachr` package by Hayley Boyce and Jordan Bourak currently
composed of 7 semi-tidy datasets for educational purposes. Here is a
brief description of each dataset:

- *apt_buildings*: Acquired courtesy of The City of Toronto’s Open Data
  Portal. It currently has 3455 rows and 37 columns.

- *building_permits*: Acquired courtesy of The City of Vancouver’s Open
  Data Portal. It currently has 20680 rows and 14 columns.

- *cancer_sample*: Acquired courtesy of UCI Machine Learning Repository.
  It currently has 569 rows and 32 columns.

- *flow_sample*: Acquired courtesy of The Government of Canada’s
  Historical Hydrometric Database. It currently has 218 rows and 7
  columns.

- *parking_meters*: Acquired courtesy of The City of Vancouver’s Open
  Data Portal. It currently has 10032 rows and 22 columns.

- *steam_games*: Acquired courtesy of Kaggle. It currently has 40833
  rows and 21 columns.

- *vancouver_trees*: Acquired courtesy of The City of Vancouver’s Open
  Data Portal. It currently has 146611 rows and 20 columns.

------------------------------------------------------------------------

1.1 **(1 point)** Out of the 7 datasets available in the `datateachr`
package, choose **4** that appeal to you based on their description.
Write your choices below:

<!-------------------------- Start your work below ---------------------------->

1: apt_buildings 2: cancer_sample 3: flow_sample 4: vancouver_trees

<!----------------------------------------------------------------------------->

1.2 **(6 points)** One way to narrowing down your selection is to
*explore* the datasets. Use your knowledge of dplyr to find out at least
*3* attributes about each of these datasets (an attribute is something
such as number of rows, variables, class type…). The goal here is to
have an idea of *what the data looks like*.

<!-------------------------- Start your work below ---------------------------->

By running the following code, it can be observed that the
*apt_buildings* dataset is of type *data_frame*, which can be coerced
into a *tibble*. The data set consists of **37** variables(Columns) and
**3455** records(Rows). And most variables are of type *character*.

``` r
class(apt_buildings)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

``` r
glimpse(apt_buildings)
```

    ## Rows: 3,455
    ## Columns: 37
    ## $ id                               <dbl> 10359, 10360, 10361, 10362, 10363, 10…
    ## $ air_conditioning                 <chr> "NONE", "NONE", "NONE", "NONE", "NONE…
    ## $ amenities                        <chr> "Outdoor rec facilities", "Outdoor po…
    ## $ balconies                        <chr> "YES", "YES", "YES", "YES", "NO", "NO…
    ## $ barrier_free_accessibilty_entr   <chr> "YES", "NO", "NO", "YES", "NO", "NO",…
    ## $ bike_parking                     <chr> "0 indoor parking spots and 10 outdoo…
    ## $ exterior_fire_escape             <chr> "NO", "NO", "NO", "YES", "NO", NA, "N…
    ## $ fire_alarm                       <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    ## $ garbage_chutes                   <chr> "YES", "YES", "NO", "NO", "NO", "NO",…
    ## $ heating_type                     <chr> "HOT WATER", "HOT WATER", "HOT WATER"…
    ## $ intercom                         <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    ## $ laundry_room                     <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    ## $ locker_or_storage_room           <chr> "NO", "YES", "YES", "YES", "NO", "YES…
    ## $ no_of_elevators                  <dbl> 3, 3, 0, 1, 0, 0, 0, 2, 4, 2, 0, 2, 2…
    ## $ parking_type                     <chr> "Underground Garage , Garage accessib…
    ## $ pets_allowed                     <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    ## $ prop_management_company_name     <chr> NA, "SCHICKEDANZ BROS. PROPERTIES", N…
    ## $ property_type                    <chr> "PRIVATE", "PRIVATE", "PRIVATE", "PRI…
    ## $ rsn                              <dbl> 4154812, 4154815, 4155295, 4155309, 4…
    ## $ separate_gas_meters              <chr> "NO", "NO", "NO", "NO", "NO", "NO", "…
    ## $ separate_hydro_meters            <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    ## $ separate_water_meters            <chr> "NO", "NO", "NO", "NO", "NO", "NO", "…
    ## $ site_address                     <chr> "65  FOREST MANOR RD", "70  CLIPPER R…
    ## $ sprinkler_system                 <chr> "YES", "YES", "NO", "YES", "NO", "NO"…
    ## $ visitor_parking                  <chr> "PAID", "FREE", "UNAVAILABLE", "UNAVA…
    ## $ ward                             <chr> "17", "17", "03", "03", "02", "02", "…
    ## $ window_type                      <chr> "DOUBLE PANE", "DOUBLE PANE", "DOUBLE…
    ## $ year_built                       <dbl> 1967, 1970, 1927, 1959, 1943, 1952, 1…
    ## $ year_registered                  <dbl> 2017, 2017, 2017, 2017, 2017, NA, 201…
    ## $ no_of_storeys                    <dbl> 17, 14, 4, 5, 4, 4, 4, 7, 32, 4, 4, 7…
    ## $ emergency_power                  <chr> "NO", "YES", "NO", "NO", "NO", "NO", …
    ## $ `non-smoking_building`           <chr> "YES", "NO", "YES", "YES", "YES", "NO…
    ## $ no_of_units                      <dbl> 218, 206, 34, 42, 25, 34, 14, 105, 57…
    ## $ no_of_accessible_parking_spaces  <dbl> 8, 10, 20, 42, 12, 0, 5, 1, 1, 6, 12,…
    ## $ facilities_available             <chr> "Recycling bins", "Green Bin / Organi…
    ## $ cooling_room                     <chr> "NO", "NO", "NO", "NO", "NO", "NO", "…
    ## $ no_barrier_free_accessible_units <dbl> 2, 0, 0, 42, 0, NA, 14, 0, 0, 1, 25, …

By running the following code, it can be observed that the
*cancer_sample* data set is of type *data_frame*, which can be coerced
into a *tibble*. The data set consists of **32** variables(Columns) and
**569** records(Rows). And most variables are of type *double*.

``` r
class(cancer_sample)
```

    ## [1] "spec_tbl_df" "tbl_df"      "tbl"         "data.frame"

``` r
glimpse(cancer_sample)
```

    ## Rows: 569
    ## Columns: 32
    ## $ ID                      <dbl> 842302, 842517, 84300903, 84348301, 84358402, …
    ## $ diagnosis               <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "…
    ## $ radius_mean             <dbl> 17.990, 20.570, 19.690, 11.420, 20.290, 12.450…
    ## $ texture_mean            <dbl> 10.38, 17.77, 21.25, 20.38, 14.34, 15.70, 19.9…
    ## $ perimeter_mean          <dbl> 122.80, 132.90, 130.00, 77.58, 135.10, 82.57, …
    ## $ area_mean               <dbl> 1001.0, 1326.0, 1203.0, 386.1, 1297.0, 477.1, …
    ## $ smoothness_mean         <dbl> 0.11840, 0.08474, 0.10960, 0.14250, 0.10030, 0…
    ## $ compactness_mean        <dbl> 0.27760, 0.07864, 0.15990, 0.28390, 0.13280, 0…
    ## $ concavity_mean          <dbl> 0.30010, 0.08690, 0.19740, 0.24140, 0.19800, 0…
    ## $ concave_points_mean     <dbl> 0.14710, 0.07017, 0.12790, 0.10520, 0.10430, 0…
    ## $ symmetry_mean           <dbl> 0.2419, 0.1812, 0.2069, 0.2597, 0.1809, 0.2087…
    ## $ fractal_dimension_mean  <dbl> 0.07871, 0.05667, 0.05999, 0.09744, 0.05883, 0…
    ## $ radius_se               <dbl> 1.0950, 0.5435, 0.7456, 0.4956, 0.7572, 0.3345…
    ## $ texture_se              <dbl> 0.9053, 0.7339, 0.7869, 1.1560, 0.7813, 0.8902…
    ## $ perimeter_se            <dbl> 8.589, 3.398, 4.585, 3.445, 5.438, 2.217, 3.18…
    ## $ area_se                 <dbl> 153.40, 74.08, 94.03, 27.23, 94.44, 27.19, 53.…
    ## $ smoothness_se           <dbl> 0.006399, 0.005225, 0.006150, 0.009110, 0.0114…
    ## $ compactness_se          <dbl> 0.049040, 0.013080, 0.040060, 0.074580, 0.0246…
    ## $ concavity_se            <dbl> 0.05373, 0.01860, 0.03832, 0.05661, 0.05688, 0…
    ## $ concave_points_se       <dbl> 0.015870, 0.013400, 0.020580, 0.018670, 0.0188…
    ## $ symmetry_se             <dbl> 0.03003, 0.01389, 0.02250, 0.05963, 0.01756, 0…
    ## $ fractal_dimension_se    <dbl> 0.006193, 0.003532, 0.004571, 0.009208, 0.0051…
    ## $ radius_worst            <dbl> 25.38, 24.99, 23.57, 14.91, 22.54, 15.47, 22.8…
    ## $ texture_worst           <dbl> 17.33, 23.41, 25.53, 26.50, 16.67, 23.75, 27.6…
    ## $ perimeter_worst         <dbl> 184.60, 158.80, 152.50, 98.87, 152.20, 103.40,…
    ## $ area_worst              <dbl> 2019.0, 1956.0, 1709.0, 567.7, 1575.0, 741.6, …
    ## $ smoothness_worst        <dbl> 0.1622, 0.1238, 0.1444, 0.2098, 0.1374, 0.1791…
    ## $ compactness_worst       <dbl> 0.6656, 0.1866, 0.4245, 0.8663, 0.2050, 0.5249…
    ## $ concavity_worst         <dbl> 0.71190, 0.24160, 0.45040, 0.68690, 0.40000, 0…
    ## $ concave_points_worst    <dbl> 0.26540, 0.18600, 0.24300, 0.25750, 0.16250, 0…
    ## $ symmetry_worst          <dbl> 0.4601, 0.2750, 0.3613, 0.6638, 0.2364, 0.3985…
    ## $ fractal_dimension_worst <dbl> 0.11890, 0.08902, 0.08758, 0.17300, 0.07678, 0…

By running the following code, it can be observed that the *flow_sample*
data set is of type *data_frame*, which can be coerced into a *tibbles*.
The data set consists of **7** variables(Columns) and **218**
records(Rows).

``` r
class(flow_sample)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

``` r
glimpse(flow_sample)
```

    ## Rows: 218
    ## Columns: 7
    ## $ station_id   <chr> "05BB001", "05BB001", "05BB001", "05BB001", "05BB001", "0…
    ## $ year         <dbl> 1909, 1910, 1911, 1912, 1913, 1914, 1915, 1916, 1917, 191…
    ## $ extreme_type <chr> "maximum", "maximum", "maximum", "maximum", "maximum", "m…
    ## $ month        <dbl> 7, 6, 6, 8, 6, 6, 6, 6, 6, 6, 6, 7, 6, 6, 6, 7, 5, 7, 6, …
    ## $ day          <dbl> 7, 12, 14, 25, 11, 18, 27, 20, 17, 15, 22, 3, 9, 5, 14, 5…
    ## $ flow         <dbl> 314, 230, 264, 174, 232, 214, 236, 309, 174, 345, 185, 24…
    ## $ sym          <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…

By running the following code, it can be observed that the
*vancouver_trees* data set is of type *data_frame*, which can be coerced
into a *tibble*. The data set consists of **20** variables(Columns) and
**146,611** records(Rows).

``` r
class(vancouver_trees)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

``` r
glimpse(vancouver_trees)
```

    ## Rows: 146,611
    ## Columns: 20
    ## $ tree_id            <dbl> 149556, 149563, 149579, 149590, 149604, 149616, 149…
    ## $ civic_number       <dbl> 494, 450, 4994, 858, 5032, 585, 4909, 4925, 4969, 7…
    ## $ std_street         <chr> "W 58TH AV", "W 58TH AV", "WINDSOR ST", "E 39TH AV"…
    ## $ genus_name         <chr> "ULMUS", "ZELKOVA", "STYRAX", "FRAXINUS", "ACER", "…
    ## $ species_name       <chr> "AMERICANA", "SERRATA", "JAPONICA", "AMERICANA", "C…
    ## $ cultivar_name      <chr> "BRANDON", NA, NA, "AUTUMN APPLAUSE", NA, "CHANTICL…
    ## $ common_name        <chr> "BRANDON ELM", "JAPANESE ZELKOVA", "JAPANESE SNOWBE…
    ## $ assigned           <chr> "N", "N", "N", "Y", "N", "N", "N", "N", "N", "N", "…
    ## $ root_barrier       <chr> "N", "N", "N", "N", "N", "N", "N", "N", "N", "N", "…
    ## $ plant_area         <chr> "N", "N", "4", "4", "4", "B", "6", "6", "3", "3", "…
    ## $ on_street_block    <dbl> 400, 400, 4900, 800, 5000, 500, 4900, 4900, 4900, 7…
    ## $ on_street          <chr> "W 58TH AV", "W 58TH AV", "WINDSOR ST", "E 39TH AV"…
    ## $ neighbourhood_name <chr> "MARPOLE", "MARPOLE", "KENSINGTON-CEDAR COTTAGE", "…
    ## $ street_side_name   <chr> "EVEN", "EVEN", "EVEN", "EVEN", "EVEN", "ODD", "ODD…
    ## $ height_range_id    <dbl> 2, 4, 3, 4, 2, 2, 3, 3, 2, 2, 2, 5, 3, 2, 2, 2, 2, …
    ## $ diameter           <dbl> 10.00, 10.00, 4.00, 18.00, 9.00, 5.00, 15.00, 14.00…
    ## $ curb               <chr> "N", "N", "Y", "Y", "Y", "Y", "Y", "Y", "Y", "Y", "…
    ## $ date_planted       <date> 1999-01-13, 1996-05-31, 1993-11-22, 1996-04-29, 19…
    ## $ longitude          <dbl> -123.1161, -123.1147, -123.0846, -123.0870, -123.08…
    ## $ latitude           <dbl> 49.21776, 49.21776, 49.23938, 49.23469, 49.23894, 4…

<!----------------------------------------------------------------------------->

1.3 **(1 point)** Now that you’ve explored the 4 datasets that you were
initially most interested in, let’s narrow it down to 1. What lead you
to choose this one? Briefly explain your choice below.

<!-------------------------- Start your work below ---------------------------->

I choose **flow_sample** dataset.

In general, it has the fewest variables, only 7, which is more suitable
for beginners to do data analysis. Additionally, each variable has a
clear and straightforward meaning, and is primarily in numeric format,
making it convenient for statistical analysis.
<!----------------------------------------------------------------------------->

1.4 **(2 points)** Time for a final decision! Going back to the
beginning, it’s important to have an *end goal* in mind.

<!-------------------------- Start your work below ---------------------------->

My *end goal* is to figure out **temporal variation in flows**,
exploring the relationship between **flow** and other temporal factors
such as year, season and date.
<!----------------------------------------------------------------------------->

# Task 2: Exploring the dataset

2.1 **(12 points)** Complete *4 out of the following 8 exercises* to
dive deeper into your data.

1.  Plot the distribution of a numeric variable.
2.  Create a new variable based on other variables in your data (only if
    it makes sense)
3.  Investigate how many missing values there are per variable. Can you
    find a way to plot this?
4.  Explore the relationship between 2 variables in a plot.
5.  Filter observations in your data according to your own criteria.
    Think of what you’d like to explore - again, if this was the
    `titanic` dataset, I may want to narrow my search down to passengers
    born in a particular year…
6.  Use a boxplot to look at the frequency of different observations
    within a single variable. You can do this for more than one variable
    if you wish!
7.  Make a new tibble with a subset of your data, with variables and
    observations that you are interested in exploring.
8.  Use a density plot to explore any of your variables (that are
    suitable for this type of plot).

2.2 **(4 points)** For each of the 4 exercises that you complete,
provide a *brief explanation* of why you chose that exercise in relation
to your data (in other words, why does it make sense to do that?), and
sufficient comments for a reader to understand your reasoning and code.

<!-------------------------- Start your work below ---------------------------->

1.  Create a new variable based on other variables in your data (only if
    it makes sense)

I created a new variable *season* based on *month* to research on
seasonal changes in flows later.

``` r
# create a new data object named season_flow_sample and make modifications to it
season_flow_sample <- flow_sample %>% mutate(season = cut(month, breaks = c(0, 2, 5, 8, 11, 12), 
                      labels = c("Winter", "Spring", "Summer", "Fall", "Winter")))
head(season_flow_sample)
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

2.  Make a new tibble with a subset of your data, with variables and
    observations that you are interested in exploring.

By using *unique()* function, we can find that all the data is derived
from one station, so we don’t need to pay attention to *station*
variable.

Furthermore, I found through online research that the *sym* attribute is
related to the condition of the river during sampling, which has nothing
to do with temporal factors. Specifically, each element in the sym
column represents one of the following: A: partial days, B: frozen
conditions, E: estimated value, S: sample(s) collected on that day, NA:
no other information. Therefore, I should remove it as well.

``` r
# use unique() to see how many stations there are
unique(season_flow_sample$station_id)
```

    ## [1] "05BB001"

``` r
# delete variables that we don't use
sub_flow_sample <- season_flow_sample %>% 
                   select(-c("station_id","sym"))
head(sub_flow_sample)
```

    ## # A tibble: 6 × 6
    ##    year extreme_type month   day  flow season
    ##   <dbl> <chr>        <dbl> <dbl> <dbl> <fct> 
    ## 1  1909 maximum          7     7   314 Summer
    ## 2  1910 maximum          6    12   230 Summer
    ## 3  1911 maximum          6    14   264 Summer
    ## 4  1912 maximum          8    25   174 Summer
    ## 5  1913 maximum          6    11   232 Summer
    ## 6  1914 maximum          6    18   214 Summer

Besides, I noticed that there is a significant difference in *flow*
values between the extreme states of maximum and minimum. It is not
suitable to present the statistical results in a single plot, and these
two aspects should be studied separately. Therefore, I have divided the
dataset into two parts.

``` r
# divide the dataset into two parts based on extreme_type
max_flow <- filter(sub_flow_sample, extreme_type =="maximum")
head(max_flow)
```

    ## # A tibble: 6 × 6
    ##    year extreme_type month   day  flow season
    ##   <dbl> <chr>        <dbl> <dbl> <dbl> <fct> 
    ## 1  1909 maximum          7     7   314 Summer
    ## 2  1910 maximum          6    12   230 Summer
    ## 3  1911 maximum          6    14   264 Summer
    ## 4  1912 maximum          8    25   174 Summer
    ## 5  1913 maximum          6    11   232 Summer
    ## 6  1914 maximum          6    18   214 Summer

``` r
min_flow <- filter(sub_flow_sample, extreme_type =="minimum")
head(min_flow)
```

    ## # A tibble: 6 × 6
    ##    year extreme_type month   day  flow season
    ##   <dbl> <chr>        <dbl> <dbl> <dbl> <fct> 
    ## 1  1909 minimum         NA    NA NA    <NA>  
    ## 2  1910 minimum         NA    NA NA    <NA>  
    ## 3  1911 minimum          2    27  5.75 Winter
    ## 4  1912 minimum          3    14  5.8  Spring
    ## 5  1913 minimum          3    18  6.12 Spring
    ## 6  1914 minimum         11    17  7.16 Fall

3.  Explore the relationship between 2 variables in a plot.

I’d like to explore the relationship between *year* and *flow* to see
the interannual variation of maximum volume of the flow. Here, I choose
line graph to present the data.

``` r
year_flow_graph <- ggplot(max_flow, aes(year, flow)) +
                   geom_line() + 
                   geom_point(colour = "cornflowerblue") + 
                   ggtitle("Interannual Variation of Maximum Flow") +
                   xlab("Year") +
                   ylab("Flow") +
                   theme_bw()
# present the plot
print(year_flow_graph)
```

![](mini-project_mini-project-1_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

4.  Use a boxplot to look at the frequency of different observations
    within a single variable. You can do this for more than one variable
    if you wish!

I’d like to observe in which seasons the river flow reaches its maximum
or minimum values, as well as the distribution of data when extreme
values are reached. To achieve this, using *boxplot* is a good choice.

Because of the significant difference in values between the maximum and
minimum, using *wrap* would result in the minimum values’ plot appearing
as a single line. Therefore, I have divided it into two separate
boxplots.

``` r
# Seasonal Maximum Flow Variation Boxplot
# Through multiple attempts, I found that the maximum flow values do not occur in winter and fall. 
# Therefore, I use only two colors to shade the box plot
season_flow_max_graph <- ggplot(max_flow, aes(x=season,y=flow, fill=season)) +
                         geom_boxplot(outlier.shape=NA, width=0.4) + 
                         geom_jitter(width=0.1, size=0.15, alpha=0.5)+
                         scale_fill_manual(values=c("lightgreen","aquamarine")) + 
                         ggtitle("Seasonal Maximum Flow Variation Boxplot")+
                         xlab("Season") + 
                         ylab("Flow") + 
                         theme_bw()
# present the plot
print(season_flow_max_graph)
```

![](mini-project_mini-project-1_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

I noticed that some *season* data are missing (NA) when the flow reaches
its minimum value, which affects the graph’s plotting. Therefore, I need
to exclude them first. It’s worth mentioning that I didn’t use
*na.omit()* to clean the entire dataset because some NA values in
certain attributes don’t affect the study of other attributes. For
example, when studying the relationship between *year* and *flow*, NA
values in the *month* column won’t impact my research results. So, I
kept these rows with NA values and would delete them when necessary.

``` r
# Seasonal Minimum Flow Variation Boxplot
# Remove rows with NA values in the 'season' column
min_flow_omit <- min_flow[!is.na(min_flow$season),]

# Minimum flow values do not occur in summer. Therefore, I use three colors to shade the box plot
season_flow_min_graph <- ggplot(min_flow_omit, aes(x=season,y=flow, fill=season)) +
                         geom_boxplot(outlier.shape=NA, width=0.4) + 
                         geom_jitter(width=0.1, size=0.15, alpha=0.5)+
                         scale_fill_manual(values=c("snow","lightgreen","orange")) +
                         ggtitle("Seasonal Minimum Flow Variation Boxplot")+
                         xlab("Season") + 
                         ylab("Flow") + 
                         theme_bw()
# present the plot
print(season_flow_min_graph)
```

![](mini-project_mini-project-1_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

<!----------------------------------------------------------------------------->

# Task 3: Choose research questions

**(4 points)** So far, you have chosen a dataset and gotten familiar
with it through exploring the data. You have also brainstormed one
research question that interested you (Task 1.4). Now it’s time to pick
4 research questions that you would like to explore in Milestone 2!
Write the 4 questions and any additional comments below.

<!--- *****START HERE***** --->

1.  Explore the interannual variation of the river flow. Is there an
    upward or downward trend?
2.  Does the interannual variation of the river flow exhibit cyclical
    changes? Do the maximum and minimum values fluctuate synchronously?
3.  Study the relationship between seasons and river flow extremes. In
    which season does the flow generally reach its maximum or minimum?
    How does the distribution of extremes vary across different seasons?
4.  Is the interannual variation of flow extremes related to the season
    in which they occur each year?
