🎄🎄🎄 day 02 🎄🎄🎄
================

``` r
library(tidyverse)
library(here)

course <- tibble(ins = c("forward 5", "down 5", "forward 8", "up 3", "down 8", "forward 2"))

course <- read_csv(here("day_02/input.txt"), col_names = "ins") %>% 
  extract(ins,
    c("dir", "value"),
    "(.*) (\\d+)",
    convert = TRUE
  )


course %>% 
  group_by(dir) %>% 
  summarise(total = sum(value)) %>% 
  pivot_wider(names_from = dir, values_from = total) %>% 
  summarise(forward * (down - up))
```

    ## # A tibble: 1 x 1
    ##   `forward * (down - up)`
    ##                     <int>
    ## 1                 1714950

``` r
course %>%
  mutate(value = if_else(dir == "up", -value, value)) %>% 
  mutate(aim = cumsum(value * (dir != "forward"))) %>% 
  filter(dir == "forward") %>% 
  mutate(delta_depth = value * aim) %>% 
  summarise(sum(value) * sum(delta_depth))
```

    ## # A tibble: 1 x 1
    ##   `sum(value) * sum(delta_depth)`
    ##                             <int>
    ## 1                      1281977850
