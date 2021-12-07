🎄🎄🎄 day 04 🎄🎄🎄
================

``` r
library(tidyverse)
library(here)

# load in the 'calls'
calls <- read_lines(here("day_04/input.txt"), n_max = 1) %>% 
  str_split(",")
calls <- calls[[1]]

# a lookup tibble of all possible lines
lines <- expand.grid(x = 1:5, y = 1:5) %>%
  mutate(line = rep(1:(n()/5), each = 5)) %>% 
  as_tibble()

# load in the cards and parse them
cards <- read_csv(here("day_04/input.txt"), skip = 2, col_names = FALSE) %>% 
  mutate(card = rep(1:(n()/5), each = 5, length.out = n()),
         row = rep(1:5, times = 5, length.out = n()),
         X1 = str_replace_all(X1, "  ", " ")) %>% 
  separate(X1, into = as.character(1:5), " ") %>% 
  pivot_longer(`1`:`5`, names_to = "col", values_to = "value",) %>% 
  mutate(col = as.integer(col)) %>% 
  mutate(called = FALSE,
         won = FALSE,
         lastcall = NA)

# joins lines and cards (I think there's a fn that does this)
cards <- cards %>% 
  crossing(lines) %>% 
  filter(row == y & col == x) %>% 
  select(-x, -y) %>% 
  arrange(card, line)

mark <- function(cards, call){
  if(any(cards$won)) return(cards)
  
  cards %>% 
    mutate(called = (value == call) | called) %>% 
    group_by(card, line) %>% 
    mutate(won = all(called)) %>%
    ungroup() %>% 
    mutate(lastcall = call)
}

cards <- accumulate(calls, mark, .init = cards) %>% 
  last()

winner <- cards %>% 
  filter(won) %>% 
  pull(card) %>% 
  unique()

cards %>% 
  filter(card == winner) %>% 
  filter(!called) %>% 
  summarise(sum(parse_number(value)) * unique(parse_number(lastcall)))
```

    ## # A tibble: 1 x 1
    ##   `sum(parse_number(value)) * unique(parse_number(lastcall))`
    ##                                                         <dbl>
    ## 1                                                       55770
