🎄🎄🎄 day 03 🎄🎄🎄
================

``` r
# get input

library(tidyverse)
library(here)

bin <- read_csv(here("day_03/input.txt"), col_names = "X")$X
```

``` r
gamma <- str_extract_all(bin, boundary("character"), simplify = TRUE) %>% 
  apply(2, function(x) round(mean(x == "1"))) %>%
  paste(collapse = "") %>% 
  strtoi(base = 2)

epsilon <- 2^12 - 1 - gamma

epsilon * gamma
```

    ## [1] 4006064
