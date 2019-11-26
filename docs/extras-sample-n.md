## More sampling {#sample_n}

In the beginner session we used `sample` to shuffle a sequence of numbers, and make random samples:


```r
sample(1:10, size=5)
```

```
## [1]  2  7  6  4 10
```

```r
sample(1:2, size=10, replace=TRUE)
```

```
##  [1] 1 2 1 1 1 1 1 2 1 1
```

We also saw how to use `expand.grid` to create experimental designs:


```r
colours=c("Red", "Green")
positions=c("Top", "Bottom")
words = c("Nobble", "Wobble", "Hobble")
expand.grid(colour=colours, position=positions, words = words)
```

```
##    colour position  words
## 1     Red      Top Nobble
## 2   Green      Top Nobble
## 3     Red   Bottom Nobble
## 4   Green   Bottom Nobble
## 5     Red      Top Wobble
## 6   Green      Top Wobble
## 7     Red   Bottom Wobble
## 8   Green   Bottom Wobble
## 9     Red      Top Hobble
## 10  Green      Top Hobble
## 11    Red   Bottom Hobble
## 12  Green   Bottom Hobble
```

---

Tidyverse contains a function which lets us sample raws from our design dataframe directly. First we make sure tidyverse is loaded:


```r
library(tidyverse)
```

```
## ── Attaching packages ────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.2.0           ✔ purrr   0.3.2      
## ✔ tibble  2.1.3           ✔ dplyr   0.8.3      
## ✔ tidyr   0.8.99.9000     ✔ stringr 1.4.0      
## ✔ readr   1.3.1           ✔ forcats 0.4.0
```

```
## ── Conflicts ───────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

And then we can use `sample_n`


```r
design <- expand.grid(colour=colours, position=positions, words = words)
design %>% sample_n(100, replace=TRUE) %>% head
```

```
##   colour position  words
## 1    Red   Bottom Hobble
## 2  Green      Top Nobble
## 3    Red   Bottom Wobble
## 4    Red   Bottom Wobble
## 5  Green      Top Wobble
## 6    Red      Top Nobble
```


**Explanation**: `sample_n` has randomly sampled rows from our design. In this case we sampled 100, but show only the first 6 (by using `head`).