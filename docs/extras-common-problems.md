## Common problems {#common-problems}


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

Some of this material was adapted from
[Andy Wills' RminR](https://ajwills72.github.io/rminr/using_rstudio.html).

### Try these things first...

**Check your typing** - The command has to be exactly as it appears on the
worksheet. In particular, check your brackets `( )`, and check that you haven't
missed off the speech marks `" "`. Check you haven't missed out any commas `,`.
Check you have typed captial letters as shown, e.g. `bayesfactor` is not the
same as `BayesFactor`. Check whether your command uses `=` or `==`, these are
not the same thing (see below for explanation).

**Restart R** - If the output you get is very different to what is shown in this
worksheet, go to "Session" on the RStudio menu, and select "Restart R". If that
doesn't fix your problem, see below.

### Errors when loading a package

If you've installed R on your own machine, and you get a message like:

`Error in library(tidyverse) : there is no package called ‘tidyverse’`

Have you installed this package? You need to install it before you can use it,
see the [cheat sheet](https://ajwills72.github.io/rminr/cheat-sheet.html).
<!-- TODO fix this and replace with datafluency cheat sheet -->

### Errors when loading data

If you get a message like:

`Error in open.connection(con, "rb") : Could not resolve host: www.willslab.org.uk`

Check your internet connection (e.g. by using the web browser to look at your
Twitter feed). If you have an internet connection, try the command again.

### Is it `=` or `==` ?

In R, `=` means the same as `<-`. Computers don't cope well with situations were
the same symbol means two different things, so we use `==` to mean "equal to".
For example `filter(education == "master")` in our income data set means keep
the people whose education level equal to "master".

### When trying to knit Rmd files {#common-rmd-problems}

-   Check you have the right number of backticks?

-   Have you used the `View()` function in your code? This only works when
    inside RStudio, not when knitting documents.

-   Have you loaded your packages at the top of the file? If you try to call
    functions from tidyverse or other packages without loading them first you
    will get errors. Sometimes this is not apparent when working in an
    interactive session, but creates an error when a package is loaded after it
    is used in the code.



### My variables have spaces of other special characters in their names!!!

Try to avoid spaces or punctuation in your variable names if possible. If you do end up with spaces in your column names, you can still access them by putting 'backticks' around the name.



Our annoying dataframe might be like this:


```r
annoying_dataframe  %>% head
```

```
## # A tibble: 3 x 1
##   `What is your favourite colour?`
##   <chr>                           
## 1 Red                             
## 2 Blue                            
## 3 Green
```

We can rename the column:


```r
annoying_dataframe %>%
  rename('favourite_colour' = `What is your favourite colour?`) %>%
  head
```

```
## # A tibble: 3 x 1
##   favourite_colour
##   <chr>           
## 1 Red             
## 2 Blue            
## 3 Green
```