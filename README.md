HW week 3
================
Madison Dunn
9/28/2021

Github link: <https://github.com/madisondunn8/433-hw-week3>

1.  How many flights have a missing dep\_time? What other variables are
    missing? What might these rows represent?

<!-- end list -->

``` r
flights %>% 
  filter(is.na(dep_time)) #8255 missing departing time
```

    ## # A tibble: 8,255 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1       NA           1630        NA       NA           1815
    ##  2  2013     1     1       NA           1935        NA       NA           2240
    ##  3  2013     1     1       NA           1500        NA       NA           1825
    ##  4  2013     1     1       NA            600        NA       NA            901
    ##  5  2013     1     2       NA           1540        NA       NA           1747
    ##  6  2013     1     2       NA           1620        NA       NA           1746
    ##  7  2013     1     2       NA           1355        NA       NA           1459
    ##  8  2013     1     2       NA           1420        NA       NA           1644
    ##  9  2013     1     2       NA           1321        NA       NA           1536
    ## 10  2013     1     2       NA           1545        NA       NA           1910
    ## # ... with 8,245 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

  - 8255 flights missing departing time.

  - Also missing dep\_delay, arr\_time, arr\_delay, air\_time

  - These rows all likely represent flights that were canceled
    altogether, not just delayed.

<!-- end list -->

2.  Currently dep\_time and sched\_dep\_time are convenient to look at,
    but hard to compute with because they’re not really continuous
    numbers. Convert them to a more convenient representation of number
    of minutes since midnight.

<!-- end list -->

``` r
flights %>% 
  mutate(
    dep_mins_since_midnight = (dep_time%/%100)*60 + (dep_time %% 100),
    sched_dep_time_mins_since_midnight = (sched_dep_time%/%100)*60+(sched_dep_time %% 100)
  )
```

    ## # A tibble: 336,776 x 21
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # ... with 336,766 more rows, and 13 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>,
    ## #   dep_mins_since_midnight <dbl>, sched_dep_time_mins_since_midnight <dbl>

3.  Look at the number of canceled flights per day. Is there a pattern?
    Is the proportion of canceled flights related to the average delay?
    Use multiple dyplr operations, all on one line, concluding with
    ggplot(aes(x= ,y=)) + geom\_point()

<!-- end list -->

``` r
flights %>%
  mutate(canceled = is.na(dep_time)) %>%
  group_by(year, month, day) %>%
  summarise(prop_canceled = mean(canceled, na.rm = T),
            avg_delay = mean(dep_delay, na.rm = T)) %>%
  ggplot(aes(x=avg_delay,y=prop_canceled)) +
  geom_point()
```

    ## `summarise()` has grouped output by 'year', 'month'. You can override using the `.groups` argument.

![](README_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

  - There does appear to be a relationship between average delay and the
    proportion of canceled flights. Based on this plot, in general, as
    the average time of delay increases, so does the proportion of
    canceled flights.
