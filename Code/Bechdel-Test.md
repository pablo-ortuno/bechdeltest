Bechdel Test
================
Pablo Ortuno
10/03/2021

#### Load and clean data

``` r
tuesdata <- tidytuesdayR::tt_load('2021-03-09')
raw_bechdel <- tuesdata[["raw_bechdel"]]
movies <- tuesdata[["movies"]]
```

------------------------------------------------------------------------

``` r
reject = c("test", "imdb", "budget", "domgross", "intgross", "code", "period_code", "decade_code", "imdb_id", "response", "poster", "error")
bracket_levels = c('0-200', '200-400', '400-600','600-800','800-1000','1000-1200','1200-1400','1400-1600','1600-1800','1800-2000','2000+')

clean_movies <- movies %>%
  select(!reject) %>%
  rename('budget' = budget_2013,
         'domgross' = domgross_2013,
         'intgross' = intgross_2013,
         'result' = binary) %>%
  mutate(domgross = as.integer(domgross),
         intgross = as.integer(intgross),
         total_gross = (domgross + intgross)*(10^-6),
         bracket = case_when(
           0 <= total_gross & total_gross < 200 ~ '0-200',
           200 <= total_gross & total_gross < 400 ~ '200-400',
           400 <= total_gross & total_gross < 600 ~ '400-600',
           600 <= total_gross & total_gross < 800 ~ '600-800',
           800 <= total_gross & total_gross < 1000 ~ '800-1000',
           1000 <= total_gross & total_gross < 1200 ~ '1000-1200',
           1200 <= total_gross & total_gross < 1400 ~ '1200-1400',
           1400 <= total_gross & total_gross < 1600 ~ '1400-1600',
           1600 <= total_gross & total_gross < 1800 ~ '1600-1800',
           1800 <= total_gross & total_gross < 2000 ~ '1800-2000',
           2000 < total_gross ~ '2000+'))

clean_movies$bracket <- factor(clean_movies$bracket, levels = bracket_levels)
```

### Figures

![](Bechdel-Test_files/figure-gfm/pass_and_fail-1.png)<!-- -->

![](Bechdel-Test_files/figure-gfm/passes_fails_per_year-1.png)<!-- -->

![](Bechdel-Test_files/figure-gfm/lower_bracket-1.png)<!-- -->

![](Bechdel-Test_files/figure-gfm/upper_bracket-1.png)<!-- -->
