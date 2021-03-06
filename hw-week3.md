Github repo: https://github.com/madisondunn8/433-hw-week3

---
title: "hw week 3"
author: "Madison Dunn"
date: "9/28/2021"
output: github_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
library(nycflights13)
library(ggplot2)
library(dplyr)
```

1. How many flights have a missing dep_time? What other variables are missing? What might these rows represent?

```{r}
flights %>% 
  filter(is.na(dep_time)) #8255 missing departing time
```
- 8255 flights missing departing time. 

- Also missing dep_delay, arr_time, arr_delay, air_time

- These rows all likely represent flights that were canceled altogether, not just delayed. 

2. Currently dep_time and sched_dep_time are convenient to look at, but hard to compute with because they’re not really continuous numbers. Convert them to a more convenient representation of number of minutes since midnight.

```{r}
flights %>% 
  mutate(
    dep_mins_since_midnight = (dep_time%/%100)*60 + (dep_time %% 100),
    sched_dep_time_mins_since_midnight = (sched_dep_time%/%100)*60+(sched_dep_time %% 100)
  )
```


3. Look at the number of canceled flights per day. Is there a pattern? Is the proportion of canceled flights related to the average delay? Use multiple dyplr operations, all on one line, concluding with ggplot(aes(x= ,y=)) + geom_point()

```{r}
flights %>%
  mutate(canceled = is.na(dep_time)) %>%
  group_by(year, month, day) %>%
  summarise(prop_canceled = mean(canceled, na.rm = T),
            avg_delay = mean(dep_delay, na.rm = T)) %>%
  ggplot(aes(x=avg_delay,y=prop_canceled)) +
  geom_point()
```

- There does appear to be a relationship between average delay and the proportion of canceled flights. Based on this plot, in general, as the average time of delay increases, so does the proportion of canceled flights. 
