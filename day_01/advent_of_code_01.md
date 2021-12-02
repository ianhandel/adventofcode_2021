🎄🎄🎄 day 01 sonar sweep 🎄🎄🎄
================


``` r
library(tidyverse)
library(here)
library(slider)

depth <- read_csv(here("day_01/input.txt"), col_names = FALSE)$X1

depth %>% 
  diff() %>%
  {.>0} %>% 
  sum()
```

    ## [1] 1766

``` r
depth %>% 
  slider::slide_sum(after = 2) %>% 
  diff() %>%
  {.>0} %>% 
  sum()
```

    ## [1] 1797
