# Visualisation and plotting {#visualisation}



![Nothing new under the sun: Mediaeval line plot, circa 1010. Image: [wikimedia](https://commons.wikimedia.org/wiki/File:Clm_14436_ecliptic_diagram.png)](media/Clm_14436_ecliptic_diagram.png)

### In brief

> Visualising data is a core skill for all quantitative researchers, and is one of the most
> transferable skills taught on the course. Although sometimes underplayed, visualisation plays a
> **_much_** more important role in good science than, for example, statistical testing. Effective
> visualisations help scientists understand their data, spot errors, and build appropriate models.

> When plotting, we are often trading-off **information density** with **clarity**. A good analogy
> here is a map: adding detail may help us navigate; but if we add irrelevant features the map
> becomes less useful.

## Session 1 {-#session1}

![A land map and nautical chart of Plymouth Sound: Cartographers face similar challenges in selecting and displaying geographical information for different purposes.[^1] [Full size](media/plymouth-mapchart.png)](media/plymouth-mapchart.png)

[^1]:

    Images used for educational purposes under assumption of fair use. Nautical chart screenshot
    from
    [gpsnauticalcharts.com](http://fishing-app.gpsnauticalcharts.com/i-boating-fishing-web-app/fishing-marine-charts-navigation.html?title=MONTEREY+BAY+boating+app#14.04/50.3584/-4.1448),
    land map from maps.google.com.

<!--
## Learning outcomes

At the end of the session you should be able to:

1. Use graphical tools in R to visualise linear relationships and group
   differences.

2. Explore visualisation strategies for situations where linear relationships
   and group differences occur together in the same dataset (e.g. grouping with
   color).

3. Transform data, creating new columns using logical operators and string
   comparison.
 -->

## 200 countries, 200 years...

<iframe width="640" height="360"
src="https://www.youtube.com/embed/jbkSRLYSojo"
frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

If you're just getting started with data visualisation, Hans Rosling's "200 countries and 200 years
in 4 minutes" is something to aspire to.

Alongside his enthusiastic presentation, the visualisations in this clip support a clear narrative,
and help us understand the data better.

His plot is interesting because it uses many different features to express features in the data:

-   X and Y axes
-   Size of the points
-   Colour
-   Time (in the animation)

These features are carefully selected to highlight important features of the data and support the
story. Although we need to have integrity in our plotting (we'll see bad examples later), this
narrative aspect of a plot is important: we need to consider our audience.

## Into the third (and fourth, and fifth...) dimension

We can determine how complex a plot is by how many **dimensions** it has.

For example, this plot has one dimension representing how revolting particular fruits and vegatables
are:


```
Mango    Pear                 Aubergine                         Snozzcumber
|        |                    |                                 |
—————————————————————————————————————————————————————————————————
```



Scatter plots are _more_ complex because they have two dimensions --- that is, they show two
variables at once. The variables are represented by the position of each point on the X and Y axes:

<div class="figure">
<img src="02-plotting_files/figure-html/unnamed-chunk-2-1.png" alt="Life expectancy and GDP per capita in countries around the world in 2002" width="672" />
<p class="caption">(\#fig:unnamed-chunk-2)Life expectancy and GDP per capita in countries around the world in 2002</p>
</div>

And Rosling's plot is more complex still because it adds dimensions of colour and size, and uses a
special **logarithmic** scale for the x-axis ([more on this later](#ggplot-scales)).

<img src="02-plotting_files/figure-html/unnamed-chunk-3-1.png" width="672" />

---

### Dimensions/aesthetics in `ggplot`

As you have already seen, `ggplot` uses the term **aesthetics** to refer to different dimensions of
a plot. '_Aesthetics_' refers to 'what things look like', and the `aes()` command in `ggplot`
creates links variables (columns in the dataset) to visual features of the plot. This is called a
**mapping**.

There are x visual features (aesthetics) of plots we will use in this session:

-   `x` and `y` axes
-   `colour`
-   `size` (of a point, or thickness of a line)
-   `shape` (of points)
-   `linetype` (i.e. dotted/patterned or solid)

#### Task: Recreate the Rosling plot {.exercise}

To create a (slightly simplified) version of the plot above, the code would look something like
this:


```r
gapminder::gapminder %>%
  filter(BLANK==BLANK)  %>%
  ggplot(aes(x=BLANK, y=BLANK, size=BLANK, color=BLANK)) + geom_point()
```

I have removed some parts of the code. Your job is to edit the parts which say `<BLANK>` and replace
them with the names of variables from the `gapminder::gapminder` dataset (you don't need to load
this --- it's part of the `gapminder` package).

Some hints:

-   The dataset is called `gapminder::gapminder` and you need to write that in full
-   Check the title of the figure above to work out which rows of the data you need to plot (and so
    define the filter)
-   All the `BLANK`s represent variable names

### Summary of the section

-   Plots can have multiple dimensions; that is, they can display several variables at once
-   Colour, shape, size and line-type are common ways of displaying Dimensions
-   In ggplot, these visual features are called **aesthetics**
-   The mapping between visual features and variables is created using the `aes()` command

## Layers

In visualising data, there's always more than one way to do things. As well as plotting different
dimensions, different _types_ of plot can highlight different features of the data. In ggplot, these
different types of plots are called **geometries**, and multiple layers can be combined in the same
plot by adding together commands which start with `geom_`.

As we have already seen, we can use `geom_point(...)` to create a scatter plot:

<div class="figure">
<img src="02-plotting_files/figure-html/unnamed-chunk-5-1.png" alt="Life expectancy and GDP in Asia" width="672" />
<p class="caption">(\#fig:unnamed-chunk-5)Life expectancy and GDP in Asia</p>
</div>

To add additional layers to this plot, we can add extra `geom_<NAME>` functions. For example,
`geom_smooth` overlays a smooth line to any x/y plot:


```r
gapminder::gapminder %>%
  filter(continent=="Asia") %>%
  ggplot(aes(lifeExp, gdpPercap)) +
  geom_point() +
  geom_smooth()
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="02-plotting_files/figure-html/unnamed-chunk-6-1.png" width="672" />

**Explanation of the command**: We added `+ geom_smooth()` to our previous plot. This means we now
have two geometries added to the same plot: `geom_point` and `geom_smooth`.

**Explanation of the output**: If you run the command above you will see some warning messages which
say `geom_smooth() using method = 'gam' and formula 'y ~ s(x, bs = "cs")'`. You can ignore this for
the moment. The plot shown is the same as the scatterplot before, but now has a smooth blue line
overlaid. This represents the local-average of GDP, for each level of `lifeExp`. There is also a
grey-shaded area, which represents the standard error of the local average (again there will be
[more on this later](#plotting-intervals)).

#### Task: Make a smoothed-line plot {.exercise}

1. Reopen the `cps2.csv` data, or use the `mtcars` or `iris` data. Create a scatter plot of any two
   continuous variables.
2. Add a smoothed line to the plot using `geom_smooth`

Optional extension task:

1. Make a plot which adds colour or size aesthetics to the plot above.

### Summary of this section

-   GGplot doesn't restrict you to a single view of the data
-   Plots can have multiple layers, presenting the same data different ways
-   Each layer is called a 'geometry', and the functions to add layers all start with `geom_`
-   Smoothed-line plots show the local average

## Facets

As we add layers, plots become more complex. We run into tradeoffs between information density and
clarity.

To give one example, this plot shows life expectancies for each country in the gapminder data,
plotted by year:


```r
gapminder::gapminder %>%
  ggplot(aes(year, lifeExp, group=country)) +
  geom_smooth(se=FALSE)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="02-plotting_files/figure-html/unnamed-chunk-7-1.png" width="672" />

**Explanation**: This is another x/y plot. This time though we have not added points, but rather
smoothed lines (one for each country).

**Explanation of the code**:We have created an x/y plot as before, but this time we only added
`geom_smooth` (and not `geom_point`), so we can't see the individual datapoints. We have also added
the text `group=country` which means we see one line per-country in the dataset. Finally, we also
added `se=FALSE` which hides the shaded area that `geom_smooth` adds by default.

**Comment on the result**: It's pretty hard to read!

To increase the information density, and explore patterns within the data, we might add another
dimension and aesthetic. The next plot colours the lines by continent:


```r
gapminder::gapminder %>%
  ggplot(aes(year, lifeExp, colour=continent, group=country)) +
  geom_smooth(se=FALSE)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="02-plotting_files/figure-html/unnamed-chunk-8-1.png" width="672" />

However, even with colours added it's still a bit of a mess. We can't see the differences between
continents easily. To clean things up we can use a technique called **facetting**:


```r
gapminder::gapminder %>%
  ggplot(aes(year, lifeExp, group=country)) +
  geom_smooth(se=FALSE) +
  facet_wrap(~continent)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="02-plotting_files/figure-html/unnamed-chunk-9-1.png" width="672" />

**Explanation**: We added the text `+ facet_grid(~continent)` to our earlier plot, but removed the
part that said `color=continent`. This made `ggplot` create individual _panels_ for each continent.
Splitting the graph this way makes it somewhat easier to compare the differences _between_
continents.

#### Task: Use facetting {.exercise}

Use the `iris` dataset which is built into R.

1. Try to recreate this plot, by adapting the code from the example above:


```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="02-plotting_files/figure-html/unnamed-chunk-10-1.png" width="672" />

Optional extension tasks:

1. Create a new plot which uses colours to distinguish species and does not use Facets
2. In this example, which plot do you prefer? What influences when facets are more useful the just
   using colour?


<div class='solution'><button>Show answer</button>


There's no right answer here, but for this example I prefer the coloured plot to the facetted one.
The reason is that there are only 3 species in this dataset, and the points for each don't overlap
much. This means it is easy to distinguish them, even in the combined plot. But, if there were
_many_ different species it might be helpful to use facets instead.

Our decisions should be driven by what we are trying to communicate with the plot. What was the
research question that motivated us to draw it?


</div>


3. Try replacing `facet_grid(~continent)` with `facet_grid(continent~.)`. What happens?

4. With the `gapminder` example from above, try replacing `facet_grid` with
   `facet_wrap(~continent)`. What happens?

5. To see more facetting examples, see
   [the documentation](<http://www.cookbook-r.com/Graphs/Facets_(ggplot2)/>).

### Summary of this section

-   Information density is good, but can harm clarity
-   Using facets can help us make comparisons between groups in the data
-   It can be helpful when adding another aesthetic would clutter our plot (e.g. we have too many
    groups to use a different colour for each)

## Scales {#ggplot-scales}

As we've already seen, plots can include multiple dimensions, and these dimensions can be displayed
using position (on the x/y axes) or colour, size etc.

`ggplot` does a really good job of picking good defaults when it converts the numbers in your
dataset to positions, colours or other visual features of plots. However in some cases it is useful
to know that you can change the default scales used.

### Continuous vs. categorical scales

So far, when we have used colour to display information we have always used **categorical**
variables. This means the colour scales in our plots have looked something like this:

<img src="02-plotting_files/figure-html/unnamed-chunk-11-1.png" width="672" />

For example, this plot shows the relationship between life expectancy and GDP in 2002, coloured by
continent (using the `gapminder` data):


```r
gapminder::gapminder %>%
  filter(year==2002) %>%
  ggplot(aes(gdpPercap, lifeExp, colour=continent)) +
  geom_point()
```

<img src="02-plotting_files/figure-html/unnamed-chunk-12-1.png" width="672" />

However in other cases we might want to use colour to display a continuous variable. If we want to
plot continuous data in colour, we need a scale like this:

![A continuous colour scale](media/redyel.png)

In the plot below the x and y axes show the relationship between fuel economy (mpg) and weight (wt,
recorded in 1000s of lbs). Colours are used to add information about how powerful (hp, short for
horsepower) each car was:


```r
mtcars %>%
  ggplot(aes(wt, mpg, color=hp)) +
  geom_point()
```

<img src="02-plotting_files/figure-html/unnamed-chunk-13-1.png" width="672" />

:::{.exercise}

Did high-powered cars tend to have good or poor fuel economy?

:::

---

Sometimes variables can be stored in the 'wrong' format in R.

To give one example, the `mtcars` dataset contains a column called `am`, which indicates if a car
had an automatic or manual transmission. The variable is coded as either 0 (=automatic transmission)
or 1 (=manual).

If we use the `am` variable for the colour aesthetic of a plot you will notice that `ggplot` wrongly
uses a continuous colour scale, suggesting that there are values between 0 and 1:


```r
mtcars %>%
  ggplot(aes(wt, mpg, color=am)) +
  geom_point()
```

<img src="02-plotting_files/figure-html/unnamed-chunk-14-1.png" width="672" />

To fix this, and draw `am` as a categorical variable, we can use the `factor` command:


```r
mtcars %>%
  ggplot(aes(wt, mpg, color=factor(am))) +
  geom_point()
```

<img src="02-plotting_files/figure-html/unnamed-chunk-15-1.png" width="672" />

**Explanation**: We replaced `colour=am` in the previous plot with `color=factor(am)`. The `factor`
command forces R to plot `am` as a categorical variable. This means we now see only two distinct
colours in the plot for values of 0 and 1, rather than a gradation for values between 0 and 1.

:::{.exercise}

The `mtcars` dataset contains another variable, `cyl`, which records how many cylinders each car
had.

1. Create a scatterplot of `mpg` and `wt`, with `cyl` as the colour aesthetic, treated as a
   categorical variable.
2. Repeat this, but now use `cyl` as a continuous or numeric variable.

Extension task:

3. Do the same again, but using a facet rather than the colour aesthetic.

:::

### Logarithmic scales

Another common problem with plotting data is dealing with extreme values, or skewed distributions.

[We already saw in the `cps2` dataset that income is very unevenly distributed](#group-density), and
a small number of people earn **_much_** more than the average: the distribution is skewed rather
than normal:


```r
cps2 <- read_csv('cps2.csv')
```

```
## Parsed with column specification:
## cols(
##   ID = col_double(),
##   sex = col_character(),
##   native = col_character(),
##   blind = col_character(),
##   hours = col_double(),
##   job = col_character(),
##   income = col_double(),
##   education = col_character()
## )
```

```r
cps2  %>%
  ggplot(aes(income, color=sex, y=..scaled..)) + geom_density()
```

<img src="02-plotting_files/figure-html/unnamed-chunk-16-1.png" width="672" />

In the [previous worksheet](#group-density) we dealt with this by using `filter` to remove cases
where people earned more than \$150000.

However there is another way to replot all of the data, but still see the gender pay gap: we can
change the scale so that the units on the x axis are not evenly spaced: we can make it so that each
marker represents an increasingly large difference:


```r
cps2 %>%
  filter(income>500) %>%
  ggplot(aes(income, color=sex, y=..scaled..)) +
  geom_density() +
  scale_x_log10()
```

<img src="02-plotting_files/figure-html/unnamed-chunk-17-1.png" width="672" />

**Explanation of the code**: We added `+ scale_x_log10()` to our previous density plot. This command
makes each unit on the x axis increase in size by a factor of 10. For this example I also filtered
out individuals earning < \$500 (there were very few of them, and it wasted space on the plot).

**Explanation of the output**: Two warnings are shown about 'infinite values' and 'removing
non-finite values'; you can ignore these for now. The y axis of the graph has stayed the same, but
the x axis has now changed. Rather than being equally-sized, the gaps in income represented by the
gridlines are now uneven. Specifically, the difference between each vertical grid line is 10 times
bigger than the previous one (you can find out
[more about what `logs and log10` means here if you are interested](https://www.khanacademy.org/math/algebra2/x2ec2f6f830c9fb89:logs/x2ec2f6f830c9fb89:log-intro/v/logarithms)).
R has (somewhat unhelpfully) switched to using
[scientific notation](https://www.khanacademy.org/math/pre-algebra/pre-algebra-exponents-radicals/pre-algebra-scientific-notation/v/scientific-notation-old).
This means that `1e+02` is equal to $1 \times$ 10^2$, or 100 to you an me. `1e+04` is  $1
\times$ 10^4$, or 10,000, and so on. We can now see the gender pay gap much as we did before when we
filtered out high earners.

**Comments on interpreting the log-scaled graph**: Although the log scale helps us see the
differences between men and women, we must remember that we are interpreting a log-scaled plot. You
will notice that --- in contrast to the previous plot where we simply removed very high earners ---
the gender differences in this plot are more obvious at lower levels of income, **even though the
absolute size of the difference in dollars is just as large for high as for low earners**. This is
an artefact of the plotting method because the scale is unevenly spaced: For a fixed difference in
income between men and women (say
$500) it will be easier to see at the bottom than at the top of the scale. Of course, the counter argument is that a $500
difference is _more important_ if you earn less than $10,000 than if you earn > $200,000, so this
extra emphasis is helpful. But there is no _correct_ answer: the different plots emphasise different
aspects of the data.

#### Task: Use a log scale {.exercise}

Use the `gapminder` dataset again.

1. Filter the data so you are only using observations from a single year
2. Plot GDP per capita (x axis) against life expectancy (y axis) using a normal scale for GDP.
3. Now replot using a log scale for GDP.
4. Discuss with others working near you: what are the advantages of using a log scale in this
   instance?

#### Optional extension task {.exercise}



This paper presents data on reaction times in a cross sectional sample of participants of different
ages: https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0189598#sec016

The data are available in Excel format from PlosOne, but I have provided a (tidied up) subset of the
data here: [subset of RT data](data/journal_pone_0189598_subset.csv).

1. Import the subset of the data, and recreate the scatterplot from Figure 1 in the paper (use
   `geom_smooth` for the lines, and don't worry if the lines are not exactly the same).

2. Use the `scale_y_log10` command to adjust the scale of the Y axis. The result should look
   something like this:


```
## Parsed with column specification:
## cols(
##   gender = col_double(),
##   rt_hand_dominant = col_double(),
##   age_years = col_double()
## )
```


```r
rtsubset  %>%
  ggplot(aes(age_years, rt_hand_dominant, colour=factor(gender))) +
  geom_point() +
  geom_smooth(se=FALSE) +
  scale_y_log10()
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

```
## Warning: Removed 1 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

<img src="02-plotting_files/figure-html/unnamed-chunk-20-1.png" width="672" />

### Summary of this section

-   Colour scales can be either categorical or continuous
-   Sometimes data are stored in the 'wrong' format. We can use `factor(<VAR>)` to force a variable
    to be categorical
-   Logarithmic (log) scales create uneven spacing on the axes.
-   Log scales can be useful when data have a skewed distribution, but we need to be careful when
    interpreting them

## Comparing categories

In the examples above we have been plotting continuous variables (and adding colours etc). We've
used density, scatter and smoothed line plots to do this.

Another common requirement is to use plots to compare summary statistics for different groups or
categories. For example, the classic plot in a psychology study looks like this:


```
## Parsed with column specification:
## cols(
##   Condition = col_character(),
##   stimuli = col_character(),
##   p = col_double(),
##   RT = col_double()
## )
```

<img src="02-plotting_files/figure-html/unnamed-chunk-21-1.png" width="672" />

However, there is evidence that readers often misinterpret bar plots. Specifically, the problem is
that we percieve values _within_ the bar area as more _likely_ than those just above, even though
this is not in fact the case.

A better choice is (almost always) to use a boxplot:


```r
expdata  %>%
  ggplot(aes(x=stimuli, y=RT)) + geom_boxplot()
```

<img src="02-plotting_files/figure-html/unnamed-chunk-22-1.png" width="672" />

**Explanation**: We used `Condition`, a category, as our x axis, and reaction times as the y axis.
We added `geom_boxplot` to show a boxplot.

:::{.tip}

If you're not familiar with boxplots, there are more details in the help files (type `?geom_boxplot`
into the console) or use the [wikipedia page here](https://en.wikipedia.org/wiki/Box_plot)

:::

:::{.exercise}

Load this (simulated) dataset here: [dummy experiment data](data/expdata.csv). Either download it
and upload to rstudio, or read it directly from that url ([reminder of how]](#read-data-from-url)).

-   Recreate the boxplot above
-   Adjust it so that `stimuli` is the variale on the x-axis
-   Use a facet to recreate the plot you saw above, combining both `Condition` and `Stimuli`

:::

---

If you _really_ need to plot the mean and standard error of different categories, ggplot has the
`stat_summary` command:


```r
expdata %>%
  ggplot(aes(Condition, RT)) + stat_summary()
```

```
## No summary function supplied, defaulting to `mean_se()
```

<img src="02-plotting_files/figure-html/unnamed-chunk-23-1.png" width="672" />

**Explanation**: We used `Condition` and `RT` as our x and y axes, as before. This time we added
`stat_summary()` instead of `geom_boxplot()`. By default this plots the mean and standard error (a
measure of variability) in each group, using a **point-range plot**. This is better than a bar chart
because it avoids multiple a known bias in how we read them. You can ignore the warning about
`No summary function supplied, defaulting to mean_se()` for now.

:::{.exercise}

As an extension exercise:

-   Adapt your facetted boxplot from above to show the mean and standard error instead
-   Can you combine both boxplot and summary in a single plot?

:::

## Spit and polish

Ggplot is great because it sets sensible defaults for most things (axes, colours etc). When you are
exploring your data these defaults typically suffice. However for publication you will often need to
polish up your plots, perhaps including:

-   Label your plot axes
-   Add lines or text
-   Change plot colours etc
-   Saving to a pdf or other output format

### Labelling axes

By default, ggplot uses variable names and the values in your data to label plots. Sometimes these
are abbreviations, or otherwise need changing.

To relabel axes we simply add `+ xlab("TEXT")` or `+ ylab("TEXT")` to an existing plot:


```r
mtcars %>% ggplot(aes(wt, mpg)) +
  geom_point() +
  xlab("Weight (1000s of lbs)") +
  ylab("Fuel economy (miles per gallon)")
```

<img src="02-plotting_files/figure-html/unnamed-chunk-24-1.png" width="672" />

:::{.exercise}

Try adding axis labels to one of your existing plots.

:::

### Changing the label of color/shape guidelines

:::{.tip}

If you are short of time you can treat the rest of this section like an extension exercise. It might
be useful for your own work, but won't form part of the assessment.

:::

When adding the colour aesthetic, ggplot uses the variable name to label the plot legend. For
example:


```r
mtcars %>%
  ggplot(aes(wt, mpg, colour=factor(cyl))) +
  geom_point()
```

<img src="02-plotting_files/figure-html/unnamed-chunk-25-1.png" width="288" />

The generated legend label sometimes looks ugly (like above) but this is easy to fix:


```r
mtcars %>%
  ggplot(aes(wt, mpg, colour=factor(cyl))) +
  geom_point() +
  labs(color="Cylinders")
```

<img src="02-plotting_files/figure-html/unnamed-chunk-26-1.png" width="288" />

**Explanation**: We added `labs(color="Cylinders")` to the plot to change the legend label.

:::{.exercise}

Try relabelling the colour legend of one of your existing plots.

:::

### Adding lines

Sometimes it can be helpful to add lines to a plot: for example to show a clinically meaningful
cutoff, or the mean of a sample.

For example, let's say we want to make a scatter plot of income in the `cps2` data, but adding a
line showing the median income. First we calculate the median:


```r
median_income = cps2 %>% summarise(median(income)) %>% pull(1)
```

**Explanation**: First, we are defining a new variable to equal the mean income in the sample. We do
this by using `summarise(mean(income))`. The part which reads `pull(1)` says "take the first
column". We need to do this because `summarise()` creates a new table, rather than a single value or
sequence of values (which we need below).


```r
cps2 %>%
  filter(income < 150000) %>%
  ggplot(aes(income, y=..scaled..)) +
    geom_density() +
    geom_vline(xintercept = median_income, color="red")
```

<img src="02-plotting_files/figure-html/unnamed-chunk-28-1.png" width="672" />

**Explanation**: We have regular density plot. This time we have added `geom_vline` which draws a
vertical line. The `xintercept` is the place on the x axis where our line should cross.

:::{.exercise}

Add a `geom_vline` to a plot you have already created. This could be either:

-   A calculated value (e.g. `mean(var)`) or
-   A fixed value (e.g. `xintercept = 20`)

:::

### Saving plots to a file

So far we have created plots in the RStudio web interface. This is fine when working interactively,
but sometimes you will need to send a high-quality plot to someone (perhaps a journal).

The `ggsave` function lets us do this.

The first step is to make a plot, and save it (give it a name).


```r
myfunkyplot <- mtcars  %>% ggplot(aes(wt, mpg, color=factor(cyl))) + geom_point()
```

**Explanation**: We used the assignment operator `<-` to save our plot to a new name
(`myfunkyplot`). This means that when we run the code RStudio won't geneate any output immediately,
so we don't see the plot yet.

---

Next, we use `ggsave` to save the plot to a particular file:


```r
ggsave('myfunkyplot.pdf', myfunkyplot, width=8, height=4)
```

You can see the output of the `ggsave` command by downloading the file here:
[myfunkyplot.pdf](myfunkyplot.pdf)

### Publication-ready plots

Some journals have specific requirements for submitting journals; common ones include submitting in
particular formats (e.g. pdf or tiff), using particular fonts etc.

There are also some common types of plots which ggplot almost, but not quite, makes out of the box.

When trying to go the last mile and polish plots for publication several additional packages may be
useful. If you have time, you could work through some of the examples on this page

-   ggpubr:
    http://www.sthda.com/english/articles/24-ggpubr-publication-ready-plots/78-perfect-scatter-plots-with-correlation-and-marginal-histograms/

As mentioned in the session, Edward Tufte's books have been influential in the field of data
visualisation. His book 'The displayt of quantitative information' [@tufte2001visual] is a great
resource and guide.

-   http://motioninsocial.com/tufte/ shows how to implement many of Tufte's ideas in ggplot. It
    would be a nice exercise to work through this, and attempt to plot some of your own data in this
    style.

## Session 2: Real world plots

So far we have learned about ggplot and worked through lots of examples. You might have noticed
though: we focussed mostly on the technique and didn't really focus on what the data meant.

In reality, you are normally presented with a dataset need to work creatively to explore patterns of
results. Your decisions will be informed by:

-   Your research questions
-   Prior knowledge about the domain
-   Prior knowledge about the research design and the data collection process
-   What your learn about the data as you work (this is an interative process)

---

In this session we are going to work through a series of examples. Each time we will start with a
scenario which describes the domain and data collection, and some research questions we may have
had.

You should work in groups of 3 to:

-   Explore the dataset
-   Develop one or two plots which illustrate key features of the data

We will then join to form larger groups to share findings, and justify decisions made.

### Scenario 1: Secret agent

You are a MI6 agent, and have been sent a mystery dataset by one of your spies. She said it contains
highly important information which will be of great interest to your superiors. Use your ggplot
wizardry to recover this classified information.



The data are available to download here: [data/mystery.csv](data/mystery.csv)

### Scenario 2

In the 1970s the University of California, Berkley, was concerned about the fairness of their
admissions procedures. They collected data from across the university for a number of years,
recording the:

-   Number of applicants
-   The department the student applied to
-   The students' gender
-   Number of students accepted
-   The percentage students of each gender who were accepted in each department

A summary of these data are available at this link: [data/berkley.csv](data/berkley.csv).

Your job is to:

-   Describe the pattern of applications
-   Decide if the university was fair in it's admissions procedures
-   Prepare a short presentation for the university governors which includes plots

Techniques/commands you might want to use:

-   `filter`
-   `group_by` and `summarise`
-   `stat_summary` to plot means and standard errors or deviations
-   `facet_wrap(~VARNAME)` to split a plot by a categorical variable

form











