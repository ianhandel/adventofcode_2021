🎄🎄🎄 day 06 🎄🎄🎄
================

``` r
library(tidyverse)
library(here)

lant <- read_lines(here("day_06/input.txt")) %>% 
  str_split(",", simplify = TRUE) %>% 
  as.numeric()

lant <- c(3,4,3,1,2)
```

### Part 1

``` r
fish <- function(x){
  x <- c(x, rep(9, sum(x == 0)))
  x[x == 0] <- 7
  x - 1
}

accumulate(1:18, ~fish(.x), .init = lant) %>% 
  last() %>% 
  length()
```

    ## [1] 26

### Part 2

``` r
n0 <- map_dbl(0:8, ~sum(lant == .x)) %>% set_names(0:8)

fish2 <- function(n){
  zeros <- n[1]
  n[1:8] <- n[2:9]
  n[9] <- zeros
  n[7] <- n[7] + zeros
  n
}

accumulate(1:256, ~fish2(.x), .init = n0) %>%
  last() %>% 
  sum() %>%
  format(digits = 20)
```

    ## [1] "26984457539"
