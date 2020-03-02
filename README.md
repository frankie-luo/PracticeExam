Practice Exam
=============

This practice exam asks you to do several code wrangling tasks that we
have done in class so far.

Clone this repo into Rstudio and fill in the necessary code. Then,
commit and push to github. Finally, turn in a link to canvas.

    ## Warning: package 'tidyverse' was built under R version 3.6.2

    ## -- Attaching packages -------------------------------------------------------------------------------------------------------------------------------------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.2.1     v purrr   0.3.2
    ## v tibble  2.1.3     v dplyr   0.8.4
    ## v tidyr   1.0.0     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.4.0

    ## Warning: package 'ggplot2' was built under R version 3.6.2

    ## Warning: package 'dplyr' was built under R version 3.6.2

    ## -- Conflicts ----------------------------------------------------------------------------------------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

    ## Warning: package 'nycflights13' was built under R version 3.6.2

Make a plot with three facets, one for each airport in the weather data.
The x-axis should be the day of the year (1:365) and the y-axis should
be the mean temperature recorded on that day, at that airport.

    library(lubridate)

    ## 
    ## Attaching package: 'lubridate'

    ## The following object is masked from 'package:base':
    ## 
    ##     date

    df <- weather %>% mutate(day_of_year = yday(time_hour))
    df2 <- df %>% select(origin, day_of_year, temp)
    df3 <- df2 %>% group_by(origin, day_of_year) %>% summarise(mean_temp = mean(temp, na.rm = T))
    df3 %>% ggplot(aes(x = day_of_year, y = mean_temp)) + geom_point() + facet_wrap(~ origin)

![](README_files/figure-markdown_strict/unnamed-chunk-2-1.png)

Make a non-tidy matrix of that data where each row is an airport and
each column is a day of the year.

    df4 <- df3 %>% pivot_wider(names_from = day_of_year, values_from = mean_temp)
    df4

    ## # A tibble: 3 x 365
    ## # Groups:   origin [3]
    ##   origin   `1`   `2`   `3`   `4`   `5`   `6`   `7`   `8`   `9`  `10`  `11`
    ##   <chr>  <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
    ## 1 EWR     36.8  28.7  29.6  34.3  36.6  39.9  40.3  38.6  42.1  43.6  42.0
    ## 2 JFK     36.9  28.6  30.1  34.7  36.8  39.3  40.1  39.4  42.7  43.6  41.3
    ## 3 LGA     37.2  28.8  30.3  35.8  38.3  41.0  41.4  42.3  44.9  44.3  40.3
    ## # ... with 353 more variables: `12` <dbl>, `13` <dbl>, `14` <dbl>,
    ## #   `15` <dbl>, `16` <dbl>, `17` <dbl>, `18` <dbl>, `19` <dbl>,
    ## #   `20` <dbl>, `21` <dbl>, `22` <dbl>, `23` <dbl>, `24` <dbl>,
    ## #   `25` <dbl>, `26` <dbl>, `27` <dbl>, `28` <dbl>, `29` <dbl>,
    ## #   `30` <dbl>, `31` <dbl>, `32` <dbl>, `33` <dbl>, `34` <dbl>,
    ## #   `35` <dbl>, `36` <dbl>, `37` <dbl>, `38` <dbl>, `39` <dbl>,
    ## #   `40` <dbl>, `41` <dbl>, `42` <dbl>, `43` <dbl>, `44` <dbl>,
    ## #   `45` <dbl>, `46` <dbl>, `47` <dbl>, `48` <dbl>, `49` <dbl>,
    ## #   `50` <dbl>, `51` <dbl>, `52` <dbl>, `53` <dbl>, `54` <dbl>,
    ## #   `55` <dbl>, `56` <dbl>, `57` <dbl>, `58` <dbl>, `59` <dbl>,
    ## #   `60` <dbl>, `61` <dbl>, `62` <dbl>, `63` <dbl>, `64` <dbl>,
    ## #   `65` <dbl>, `66` <dbl>, `67` <dbl>, `68` <dbl>, `69` <dbl>,
    ## #   `70` <dbl>, `71` <dbl>, `72` <dbl>, `73` <dbl>, `74` <dbl>,
    ## #   `75` <dbl>, `76` <dbl>, `77` <dbl>, `78` <dbl>, `79` <dbl>,
    ## #   `80` <dbl>, `81` <dbl>, `82` <dbl>, `83` <dbl>, `84` <dbl>,
    ## #   `85` <dbl>, `86` <dbl>, `87` <dbl>, `88` <dbl>, `89` <dbl>,
    ## #   `90` <dbl>, `91` <dbl>, `92` <dbl>, `93` <dbl>, `94` <dbl>,
    ## #   `95` <dbl>, `96` <dbl>, `97` <dbl>, `98` <dbl>, `99` <dbl>,
    ## #   `100` <dbl>, `101` <dbl>, `102` <dbl>, `103` <dbl>, `104` <dbl>,
    ## #   `105` <dbl>, `106` <dbl>, `107` <dbl>, `108` <dbl>, `109` <dbl>,
    ## #   `110` <dbl>, `111` <dbl>, ...

For each (airport, day) contruct a tidy data set of the airport’s
“performance” as the proportion of flights that departed less than an
hour late.

    df5 <- flights %>% mutate(day_of_year = yday(time_hour)) %>% select(origin, day_of_year, dep_delay) 
    df6 <- df5 %>% group_by(origin, day_of_year) %>% summarise(performance = sum(dep_delay < 60, na.rm = T)/n())
    df6

    ## # A tibble: 1,095 x 3
    ## # Groups:   origin [3]
    ##    origin day_of_year performance
    ##    <chr>        <dbl>       <dbl>
    ##  1 EWR              1       0.915
    ##  2 EWR              2       0.823
    ##  3 EWR              3       0.970
    ##  4 EWR              4       0.929
    ##  5 EWR              5       0.962
    ##  6 EWR              6       0.947
    ##  7 EWR              7       0.921
    ##  8 EWR              8       0.976
    ##  9 EWR              9       0.973
    ## 10 EWR             10       0.977
    ## # ... with 1,085 more rows

Construct a tidy data set to that give weather summaries for each
(airport, day). Use the total precipitation, minimum visibility, maximum
wind\_gust, and average wind\_speed.

    df7 <- df %>% select(origin, day_of_year, precip, visib, wind_gust, wind_speed)
    df8 <- df7 %>% group_by(origin, day_of_year) %>% summarise(total_precipitation = sum(precip), minimum_visibility = min(visib), maximum_wind_gust = max(wind_gust, na.rm = T), average_wind_speed = mean(wind_speed))
    df8 <- df8[!is.infinite(df8$maximum_wind_gust),]
    df8

    ## # A tibble: 797 x 6
    ## # Groups:   origin [3]
    ##    origin day_of_year total_precipita~ minimum_visibil~ maximum_wind_gu~
    ##    <chr>        <dbl>            <dbl>            <dbl>            <dbl>
    ##  1 EWR              1             0                10               26.5
    ##  2 EWR              2             0                10               26.5
    ##  3 EWR              4             0                10               31.1
    ##  4 EWR              5             0                10               26.5
    ##  5 EWR              6             0                 6               19.6
    ##  6 EWR              7             0                10               24.2
    ##  7 EWR              8             0                 8               25.3
    ##  8 EWR              9             0                 6               20.7
    ##  9 EWR             10             0                10               26.5
    ## 10 EWR             14             0.06              0.5             27.6
    ## # ... with 787 more rows, and 1 more variable: average_wind_speed <dbl>

Construct a linear model to predict the performance of each
(airport,day) using the weather summaries and a “fixed effect” for each
airport. Display the summaries.

    df9 <- inner_join(df6, df8)

    ## Joining, by = c("origin", "day_of_year")

    df9

    ## # A tibble: 797 x 7
    ## # Groups:   origin [3]
    ##    origin day_of_year performance total_precipita~ minimum_visibil~
    ##    <chr>        <dbl>       <dbl>            <dbl>            <dbl>
    ##  1 EWR              1       0.915             0                10  
    ##  2 EWR              2       0.823             0                10  
    ##  3 EWR              4       0.929             0                10  
    ##  4 EWR              5       0.962             0                10  
    ##  5 EWR              6       0.947             0                 6  
    ##  6 EWR              7       0.921             0                10  
    ##  7 EWR              8       0.976             0                 8  
    ##  8 EWR              9       0.973             0                 6  
    ##  9 EWR             10       0.977             0                10  
    ## 10 EWR             14       0.985             0.06              0.5
    ## # ... with 787 more rows, and 2 more variables: maximum_wind_gust <dbl>,
    ## #   average_wind_speed <dbl>

    lm1 <- lm(data = df9, performance ~ .)
    summary(lm1)

    ## 
    ## Call:
    ## lm(formula = performance ~ ., data = df9)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -0.47614 -0.02168  0.01984  0.04667  0.20877 
    ## 
    ## Coefficients:
    ##                       Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)          8.150e-01  1.831e-02  44.501  < 2e-16 ***
    ## originJFK            3.640e-02  8.518e-03   4.273 2.16e-05 ***
    ## originLGA            2.002e-02  7.813e-03   2.563   0.0106 *  
    ## day_of_year          7.405e-05  3.172e-05   2.335   0.0198 *  
    ## total_precipitation -5.627e-02  1.161e-02  -4.849 1.50e-06 ***
    ## minimum_visibility   1.296e-02  1.144e-03  11.334  < 2e-16 ***
    ## maximum_wind_gust   -1.079e-03  7.020e-04  -1.538   0.1246    
    ## average_wind_speed  -1.234e-03  1.210e-03  -1.019   0.3084    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.09184 on 787 degrees of freedom
    ##   (2 observations deleted due to missingness)
    ## Multiple R-squared:  0.3022, Adjusted R-squared:  0.296 
    ## F-statistic: 48.69 on 7 and 787 DF,  p-value: < 2.2e-16

Repeat the above, but only for EWR. Obviously, exclude the fixed effect
for each airport.

    data = df9 %>% filter(origin == 'JFK')
    data$origin = NULL
    lm2 <- lm(data = data, performance ~ .)
    summary(lm2)

    ## 
    ## Call:
    ## lm(formula = performance ~ ., data = data)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -0.34619 -0.01768  0.01635  0.03653  0.16773 
    ## 
    ## Coefficients:
    ##                       Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)          8.511e-01  2.963e-02  28.723  < 2e-16 ***
    ## day_of_year          6.842e-05  4.915e-05   1.392  0.16528    
    ## total_precipitation -4.741e-02  1.803e-02  -2.630  0.00913 ** 
    ## minimum_visibility   1.201e-02  1.690e-03   7.107  1.5e-11 ***
    ## maximum_wind_gust   -8.580e-04  1.136e-03  -0.755  0.45104    
    ## average_wind_speed  -1.182e-03  2.149e-03  -0.550  0.58280    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.08021 on 228 degrees of freedom
    ##   (1 observation deleted due to missingness)
    ## Multiple R-squared:  0.3443, Adjusted R-squared:  0.3299 
    ## F-statistic: 23.95 on 5 and 228 DF,  p-value: < 2.2e-16
