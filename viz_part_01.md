viz_part_01
================
Riya Bhilegaonkar
2022-09-29

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.0      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.2      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(ggridges)
```

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: C:\Users\RIYABH~1\AppData\Local/Cache/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2022-09-29 10:33:34 (8.418)

    ## file min/max dates: 1869-01-01 / 2022-09-30

    ## using cached file: C:\Users\RIYABH~1\AppData\Local/Cache/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2022-09-29 10:34:00 (1.703)

    ## file min/max dates: 1965-01-01 / 2020-03-31

    ## using cached file: C:\Users\RIYABH~1\AppData\Local/Cache/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2022-09-29 10:34:11 (0.952)

    ## file min/max dates: 1999-09-01 / 2022-09-30

Scatterplot:

``` r
ggplot(weather_df, aes(x=tmin, y=tmax)) + geom_point()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_part_01_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

Let’s make the same scatterplot, but different

``` r
weather_df %>%
  drop_na() %>%
  filter(name == "CentralPark_NY") %>%
  ggplot(aes(x=tmin, y=tmax)) + geom_point()
```

![](viz_part_01_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Another scatterplot:

``` r
weather_scatterplot =
  weather_df %>%
  drop_na() %>%
  ggplot(aes(x=tmin, y=tmax))

weather_scatterplot + geom_point()
```

![](viz_part_01_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

## Let’s fancy this up a bit

``` r
weather_df %>%
  ggplot(aes(x=tmin, y=tmax, color=name)) +
  geom_point(alpha=.3) +
  geom_smooth(se=FALSE) + 
  facet_grid(name~.)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_part_01_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Seasonal trends plots:

``` r
weather_df %>%
  ggplot(aes(x=date, y = tmax, color=name))+
  geom_point(aes(size=prcp), alpha=0.3) + 
  geom_smooth(se=FALSE)+
  facet_grid(.~name) +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](viz_part_01_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

Other:

``` r
weather_df %>%
  ggplot(aes(x=tmin, y=tmax))+
  geom_hex()
```

    ## Warning: Removed 15 rows containing non-finite values (stat_binhex).

![](viz_part_01_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
weather_df %>%
  ggplot(aes(x=tmax, fill=name))+
  geom_histogram()+
  facet_grid(.~name)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 3 rows containing non-finite values (stat_bin).

![](viz_part_01_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
weather_df %>%
  ggplot(aes(x=tmax, fill=name))+
  geom_density(alpha=.3)
```

    ## Warning: Removed 3 rows containing non-finite values (stat_density).

![](viz_part_01_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

Boxplot

``` r
weather_df %>%
  ggplot(aes(x=name, y=tmax, fill=name))+
  geom_boxplot()+
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

    ## Warning: Removed 3 rows containing non-finite values (stat_boxplot).

![](viz_part_01_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

Violin Plot:

``` r
weather_df %>%
  ggplot(aes(x=name, y=tmax, fill=name))+
  geom_violin()
```

    ## Warning: Removed 3 rows containing non-finite values (stat_ydensity).

![](viz_part_01_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

OR

``` r
weather_df %>%
  ggplot(aes(x=tmax, y=name)) +
  geom_density_ridges()
```

    ## Picking joint bandwidth of 1.84

    ## Warning: Removed 3 rows containing non-finite values (stat_density_ridges).

![](viz_part_01_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

## Saving and embedding plots

``` r
weather_scatterplot = 
  weather_df %>%
  ggplot(aes(x = date, y=tmax, color=name)) +
  geom_point(aes(size = prcp), alpha=0.3)+
  geom_smooth(se=FALSE)+
  facet_grid(.~name)

weather_scatterplot
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](viz_part_01_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

``` r
ggsave("results/weather_scatterplot.pdf", weather_scatterplot, width = 8, height =5)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

``` r
weather_scatterplot
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](viz_part_01_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->
