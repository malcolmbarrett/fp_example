
<!-- README.md is generated from README.Rmd. Please edit that file -->

## Constructing a forest plot from scratch in ggplot2

``` r
library(mbmisc) #  devtools::install_github("malcolmbarrett/mbmisc")
library(tidyverse)
library(patchwork)

ors <- data.frame(
  labs = c('Overall\nAdjusted', '2012-2013\nAdjusted', '2013-2014\nAdjusted','2014-2015\nAdjusted'),
  measure = c(0.94, 1.22, 0.59, 1.09),
  lower = c(0.77, 0.80, 0.40 , 0.83),
  upper = c(1.15, 1.84, 0.85, 1.44)
)

# forest plot
forest <- ggplot(ors, aes(x = measure, y = labs, xmin = lower, xmax = upper)) +
    geom_point(shape = 18, size = 3) +
    geom_errorbarh(height = 0) +
    scale_x_log10() + 
    theme_bw() +
    ylab(NULL) + 
    xlab("Odds Ratio")

# plot the ORs + 95% CI 
txt <- ors %>% 
  mutate(estimates = est_ci(measure, lower, upper)) %>% # est_ci() from mbmisc
  ggplot(aes(x = 1, y = labs, label = estimates)) + 
    geom_text() +
    theme_bw() +
    theme(axis.text = element_blank(),
          axis.ticks = element_blank(),
          axis.title = element_blank(),
          panel.grid = element_blank(),
          panel.border = element_blank())

forest + txt # patchwork combines them easily 
```

![](README_files/figure-gfm/forest%20plot-1.png)<!-- -->
