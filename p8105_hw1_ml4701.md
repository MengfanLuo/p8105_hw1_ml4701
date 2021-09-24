P8105 Homework 1
================
Mengfan Luo (ml4701)

### Problem 1

The following data frame `df` is constructed using `tibble()`. It is
comprised of:

-   random sample `norm_samp` of size 10 from a standard Normal
    distribution. `set.seed(1234)` is used to fix the random generation.
-   logical vector `vec_logic` indicating whether elements of the sample
    are greater than 0
-   character vector `vec_char` of length 10
-   factor vector `vec_factor` of length 10, with 3 different factor
    levels `H`,`M` and `L`

``` r
set.seed(1234)
df = tibble(
  norm_samp = rnorm(10),
  vec_logic = norm_samp > 0,
  vec_char = c("a","b","c","d","e","f","g","h","i","j"),
  vec_factor = factor(c("H","M","L","L","H","M","L","M","M","H"))
)

#Visualize as a table
knitr::kable(df)
```

| norm\_samp | vec\_logic | vec\_char | vec\_factor |
|-----------:|:-----------|:----------|:------------|
| -1.2070657 | FALSE      | a         | H           |
|  0.2774292 | TRUE       | b         | M           |
|  1.0844412 | TRUE       | c         | L           |
| -2.3456977 | FALSE      | d         | L           |
|  0.4291247 | TRUE       | e         | H           |
|  0.5060559 | TRUE       | f         | M           |
| -0.5747400 | FALSE      | g         | L           |
| -0.5466319 | FALSE      | h         | M           |
| -0.5644520 | FALSE      | i         | M           |
| -0.8900378 | FALSE      | j         | H           |

Taking the mean of each variable in data frame `df`, numeric variable
`norm_samp` has the mean of -0.3831574, and logical vector `vec_logic`
has the mean of 0.4, while for character vector `vec_char` and factor
vector `vec_factor`, taking mean of them will result in a warning
message and get `NA` for the mean.

``` r
mean(pull(df,norm_samp))
## [1] -0.3831574
mean(pull(df,vec_logic))
## [1] 0.4
mean(pull(df,vec_char))
## Warning in mean.default(pull(df, vec_char)): argument is not numeric or logical:
## returning NA
## [1] NA
mean(pull(df,vec_factor))
## Warning in mean.default(pull(df, vec_factor)): argument is not numeric or
## logical: returning NA
## [1] NA
```

Using `as.numeric` function to the logical, character, and factor
variables, logical variable `vec_logic` was converted to numeric numbers
`0` and `1`, with original argument `FALSE` being converted to `0` and
`TRUE` to `1`; factor vector `vec_factor` was also converted to numeric,
with the 3 levels being converted to `1`, `2` and `3`, while character
vector `vec_char` cannot be converted.

``` r
as.numeric(pull(df,vec_logic))
as.numeric(pull(df,vec_char))
as.numeric(pull(df,vec_factor))
```

Only logical and factor variables can be converted to numeric variables.
This may because logical and factor variables stand for 2 or several
categories and have limited numbers of value. logical values have only
two values `TRUE` and `FALSE`, which can be easily encoded into `1` and
`0`. And it is reasonable and somewhat instinct to take a mean of the
encoded values – reflecting the fraction of `TRUE`(`1`) among all
values.

Factor variables should also have a predefined number of categories, and
thus can be converted into the same amount of integers to present.
However, factor values may or may not have levels and orders, and it’s
meaningless to take mean to the encoded values if there’s no order, and
thus we cannot take mean for factor variables.

For character variables, the value can be arbitrary input and can be
infinite, and they don’t have orders, so it’s difficult and meaningless
to represent them as numeric values, nor to calculate the mean.

### Problem 2

package `palmerpenguins` has been installed and here I load the dataset
`penguins` and view the first 10 lines of it.

``` r
data("penguins", package = "palmerpenguins")
knitr::kable(head(penguins,10))
```

| species | island    | bill\_length\_mm | bill\_depth\_mm | flipper\_length\_mm | body\_mass\_g | sex    | year |
|:--------|:----------|-----------------:|----------------:|--------------------:|--------------:|:-------|-----:|
| Adelie  | Torgersen |             39.1 |            18.7 |                 181 |          3750 | male   | 2007 |
| Adelie  | Torgersen |             39.5 |            17.4 |                 186 |          3800 | female | 2007 |
| Adelie  | Torgersen |             40.3 |            18.0 |                 195 |          3250 | female | 2007 |
| Adelie  | Torgersen |               NA |              NA |                  NA |            NA | NA     | 2007 |
| Adelie  | Torgersen |             36.7 |            19.3 |                 193 |          3450 | female | 2007 |
| Adelie  | Torgersen |             39.3 |            20.6 |                 190 |          3650 | male   | 2007 |
| Adelie  | Torgersen |             38.9 |            17.8 |                 181 |          3625 | female | 2007 |
| Adelie  | Torgersen |             39.2 |            19.6 |                 195 |          4675 | male   | 2007 |
| Adelie  | Torgersen |             34.1 |            18.1 |                 193 |          3475 | NA     | 2007 |
| Adelie  | Torgersen |             42.0 |            20.2 |                 190 |          4250 | NA     | 2007 |

``` r
colnames_peng = colnames(penguins) # create a new instance to store column names in dataset panguins
```

``` r
skimr::skim(penguins)
```

Further looking into the dataset using `skim`, it has `344` rows and `8`
columns. There are `19` numbers missing. The 8 variables including:

-   `species`: factor variable with 3 distinct values. It has `0`
    missing values.

-   `island`: factor variable with 3 distinct values. It has `0` missing
    values.

-   `bill_length_mm`: numeric variable with mean `43.9219298` and range
    `[32.1, 59.6]`. It has `2` missing values.

-   `bill_depth_mm`: numeric variable with mean `17.1511696` and range
    `[13.1, 21.5]`. It has `2` missing values.

-   `flipper_length_mm`: numeric variable with mean `200.9152047` and
    range `[172, 231]`. It has `2` missing values.

-   `body_mass_g`: numeric variable with mean `4201.754386` and range
    `[2700, 6300]`. It has `2` missing values.

-   `sex`: factor variable with 2 distinct values. It has `11` missing
    values.

-   `year`: numeric variable of integers `2007`,`2008` and `2009`. It
    has `0` missing values.

Looking in to the variable `flipper_length_mm`, the mean flipper length
(stripping `NA` values) is `200.9152047` mm.

The following is a scatterplot of `flipper_length_mm` (y) vs
`bill_length_mm` (x), and the plot is saved in the project directory as
`hw1_scatterplot.pdf`.

``` r
ggplot(penguins, aes(x = bill_length_mm, y = flipper_length_mm, color = species)) + geom_point()
```

![](p8105_hw1_ml4701_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
ggsave("hw1_scatterplot.pdf",height = 4, width = 6)
```
