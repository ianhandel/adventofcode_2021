🎄🎄🎄 day 06 🎄🎄🎄
================

``` r
library(tidyverse)
library(here)

lant <- read_lines(here("day_06/input.txt")) %>% 
  str_split(",", simplify = TRUE) %>% 
  as.numeric()

# lant <- c(3,4,3,1,2)
```

### Part 1

``` r
fish <- function(x){
  x <- c(x, rep(9, sum(x == 0)))
  x[x == 0] <- 7
  x - 1
}

accumulate(1:80, ~fish(.x), .init = lant) %>% 
  last() %>% 
  length()
```

    ## [1] 376194
