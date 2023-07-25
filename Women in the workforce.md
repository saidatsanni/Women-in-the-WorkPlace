Women in the workforce
================

  - [Introduction](#introduction)
  - [Packages Required](#packages-required)
  - [Data Preparation](#data-preparation)
      - [Data Source](#data-source)
      - [Data Import and Description](#data-import-and-description)
      - [Data Cleaning](#data-cleaning)
      - [Data Dictionary](#data-dictionary)
  - [Exploratory Data Analysis](#exploratory-data-analysis)
      - [Industry based analysis](#industry-based-analysis)
      - [Gender based Analysis](#gender-based-analysis)
      - [Time series analysis](#time-series-analysis)
  - [Summary](#summary)

## Introduction

**Problem Statement**

Gender bias in earnings is an ongoing issue. In 2019, the [American
Association of University Women
(AAUW)](https://www.aauw.org/research/the-simple-truth-about-the-gender-pay-gap/)
stated that women who work full-time earn about 80 percent of what their
male counterparts make. This report identified the following problems
which we attempt to investigate in our study:

  - The gender wage gap continues to grow over time.
  - Women of all age groups face gender pay gap.
  - Women face a wider pay gap as they get older.
  - Women are still faced with the gender pay gap in occupations with an
    equal representation of male and female workers.

**Data and Methodology**

For this study, we would use the [Women in
Workforce](https://github.com/rfordatascience/tidytuesday/blob/master/data/2019/2019-03-05/readme.md)
data which is historical data about womens’ earnings and employment
status, specific occupation and earnings from 2013-2016, compiled from
the [Bureau of Labor Statistics](https://www.bls.gov/) and the [Census
Bureau](https://www.census.gov/). We intend to analyze the data using
the following methodologies: *Trend analysis, descriptive analysis, data
visualization techniques, and inferential analysis*.

**Our Contribution**

Our results would provide more insights on the gender pay gap across
fields, occupations, and age groups. Our findings would shed some light
on the gender pay gap faced by women across all age groups. Finally, we
hope that our study would contribute to the discussion on the gender pay
gap.

## Packages Required

**We used the following packages:**

``` r
library(readxl) #used to import Excel files into R
library(tidyverse) #used for data manipulation 
library(dplyr) # used for data manipulation
library(DT) # used for displaying R data objects (matrices or data frames) as tables on HTML pages
library(knitr) #used to display an aligned table on the screen
library(kableExtra)#used to build with straightforward formatting options
library(ggplot2) # for data visualization
library(scales)     # for scale_y_continuous(label = percent)
library(ggthemes)   # for scale_fill_few('medium')
library(ggalt)    #for the dumbbell plot
```

## Data Preparation

### Data Source

For this study, we use the [Women in
Workforce](https://github.com/rfordatascience/tidytuesday/blob/master/data/2019/2019-03-05/readme.md)
data which is historical data about womens’ earnings and employment
status, specific occupation and earnings from 2013-2016, compiled from
the [Bureau of Labor Statistics](https://www.bls.gov/) and the [Census
Bureau](https://www.census.gov/). The data was provided in March 2019 as
part of the
[\#TidyTuesday](https://thomasmock.netlify.com/post/tidytuesday-a-weekly-social-data-project-in-r/)
project to celebrate the Women’s History month.

The entire data is spread into 3 files: **jobs\_gender.csv**,
**earnings\_female.csv**, **employed\_gender.csv** and are described
below.

### Data Import and Description

The three datasets are first imported from csv files into dataframes
named **jobs\_gender**, **earnings\_female**,and **employed\_gender**

#### **Dataset 1 - jobs\_gender**

This dataset contains information on the total number of male and female
workers and the total estimated median earnings for all employees at
various occupation levels, from 2013-2016. The dataset has 12 variables,
with a total of 2,088 data points, across 8 major job categories, 23
minor job categories, and 522 occupation types. There are some missing
values recorded as “NA” and are taken care of during the data cleaning
process.

More information on this dataset can be found
[here](https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-03-05/jobs_gender.csv)

*Checking the names of the data columns*

    ##  [1] "year"                  "occupation"           
    ##  [3] "major_category"        "minor_category"       
    ##  [5] "total_workers"         "workers_male"         
    ##  [7] "workers_female"        "percent_female"       
    ##  [9] "total_earnings"        "total_earnings_male"  
    ## [11] "total_earnings_female" "wage_percent_of_male"

*Check the dimension of the dataset*

    ## [1] 2088   12

*Counting the number of distinct values/observations*

    ##   occupation minor_category major_category
    ## 1        522             23              8

*Checking the number of missing values per column*

    ##                  year            occupation        major_category 
    ##                     0                     0                     0 
    ##        minor_category         total_workers          workers_male 
    ##                     0                     0                     0 
    ##        workers_female        percent_female        total_earnings 
    ##                     0                     0                     0 
    ##   total_earnings_male total_earnings_female  wage_percent_of_male 
    ##                     4                    65                   846

#### **Dataset 2 - earnings\_female**

This dataset contains the historic information of female salary as a
percent of male salary for various age groups, from year 1979 to 2011.
The dataset has 3 variables with 264 observations. This dataset has no
missing values.

The dataset can be found
[here](https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-03-05/earnings_female.csv)

*Checking the names of the data columns*

    ## [1] "Year"    "group"   "percent"

*Check the dimension of the dataset*

    ## [1] 264   3

*Checking the number of missing values per column*

    ##    Year   group percent 
    ##       0       0       0

#### **Dataset 3 - employed\_gender**

This dataset shows the percentage of part-time and full-time employees
for each year at the gender level. The dataset has 7 variables with 49
observations each, from year 1968 to 2016. This dataset has no missing
values and can be accessed
[here](https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-03-05/employed_gender.csv)

*Checking the names of the data columns*

    ## [1] "year"             "total_full_time"  "total_part_time" 
    ## [4] "full_time_female" "part_time_female" "full_time_male"  
    ## [7] "part_time_male"

*Check the dimension of the dataset*

    ## [1] 49  7

*Checking the number of missing values per column*

    ##             year  total_full_time  total_part_time full_time_female 
    ##                0                0                0                0 
    ## part_time_female   full_time_male   part_time_male 
    ##                0                0                0

### Data Cleaning

#### Variable Types

We investigate the variable types in the dataset to see if they are
accurate or need to be changed.The outputs below indicate that all of
the variable types in all three datasets are correct.

**Dataset 1 - jobs\_gender**

    ## Observations: 2,088
    ## Variables: 12
    ## $ year                  <int> 2013, 2013, 2013, 2013, 2013, 2013, 2013...
    ## $ occupation            <fct> "Chief executives", "General and operati...
    ## $ major_category        <fct> "Management, Business, and Financial", "...
    ## $ minor_category        <fct> Management, Management, Management, Mana...
    ## $ total_workers         <int> 1024259, 977284, 14815, 43015, 754514, 4...
    ## $ workers_male          <int> 782400, 681627, 8375, 17775, 440078, 161...
    ## $ workers_female        <int> 241859, 295657, 6440, 25240, 314436, 280...
    ## $ percent_female        <dbl> 23.6, 30.3, 43.5, 58.7, 41.7, 63.5, 33.6...
    ## $ total_earnings        <int> 120254, 73557, 67155, 61371, 78455, 7411...
    ## $ total_earnings_male   <int> 126142, 81041, 71530, 75190, 91998, 9007...
    ## $ total_earnings_female <int> 95921, 60759, 65325, 55860, 65040, 66052...
    ## $ wage_percent_of_male  <dbl> 76.04208, 74.97316, 91.32532, 74.29179, ...

**Dataset 2 - earnings\_female**

    ## Observations: 264
    ## Variables: 3
    ## $ Year    <int> 1979, 1980, 1981, 1982, 1983, 1984, 1985, 1986, 1987, ...
    ## $ group   <fct> "Total, 16 years and older", "Total, 16 years and olde...
    ## $ percent <dbl> 62.3, 64.2, 64.4, 65.7, 66.5, 67.6, 68.1, 69.5, 69.8, ...

**Dataset 3- employed\_gender**

    ## Observations: 49
    ## Variables: 7
    ## $ year             <int> 1968, 1969, 1970, 1971, 1972, 1973, 1974, 197...
    ## $ total_full_time  <dbl> 86.0, 85.5, 84.8, 84.4, 84.3, 84.4, 84.2, 83....
    ## $ total_part_time  <dbl> 14.0, 14.5, 15.2, 15.6, 15.7, 15.6, 15.8, 16....
    ## $ full_time_female <dbl> 75.1, 74.9, 73.9, 73.2, 73.1, 73.2, 73.2, 72....
    ## $ part_time_female <dbl> 24.9, 25.1, 26.1, 26.8, 26.9, 26.8, 26.8, 27....
    ## $ full_time_male   <dbl> 92.2, 91.8, 91.5, 91.2, 91.1, 91.4, 91.2, 90....
    ## $ part_time_male   <dbl> 7.8, 8.2, 8.5, 8.8, 8.9, 8.6, 8.8, 9.4, 9.4, ...

#### Missing Values & Data Imputation

The next stage of the data cleaning process is to check for missing
values and make decisions on removal or data imputation, where
appropriate. As shown in the **Data Import and Description tab**, the
only data set with some missing values is **Dataset 1 - jobs\_gender**.
So, we focus on this dataset.

1.  There are **65 cases with missing values** for the total earnings
    for female workers. Of this 65 cases, there are **19 cases** where
    the total number of female workers in the field or occupation types
    is zero and the total earnings for female is missing “NA”. We impute
    these missing values as zero because no female workers implies no
    total female earnings.

2.  For the **remaining 46 observations** with missing values for the
    total female earnings,we observe that the number of female workers
    is reported, indicating that these values are non-zero and missing.
    Hence, we remove the entire row from the dataset. We believe that
    this removal would not affect our analysis and goals for this study
    because, it’s just about 2 percent of the entire dataset.

3.  There are **4 observations** where the total earnings for male
    workers is missing. In 3 of these cases, the number of male workers
    is zero and the total earnings for male is missing “NA”. A closer
    look at these three observations revealed that in 2013, 2014, and
    2016, there were no male workers who worked as “Nurse Midwives”.
    Therefore, we imputed the missing total male earnings as zero
    because no male workers implies no male earnings. Consequently, the
    percentage of female earnings to male earnings is imputed as zero.

4.  In 2015, a total of 53 male workers was reported for Nurse midwives.
    This is very different from what was reported in 2013, 2014, and
    2016. This could mean that the occupation type (Nurse Midwives) had
    male workers in 2015 or an error occurred during the data reporting
    process. We decide to remove the entire row associated with this
    observation not because of the anomaly, but because the male total
    earnings is missing and reported as “NA”.

Finally, we notice that the column “wage\_percent\_of\_male” showing the
percentage of the total female earnings to total male earnings has a
total of 846 missing values. Given that this is the percentage of the
total female earnings to the total male earnings, we calculate and
impute this variable using the total\_earnings\_male and
total\_earning\_female columns.

#### Outliers

We examine the presence of outliers using box plots in two parts. First
we look at the number of workers variables. This shows the presence of
outliers. However, this is not surprising because the number of workers
vary by occupation.

![](Women-in-the-Workforce_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

Similarly the box plot of the earnings variables show the presence of
outliers. This is also unsurprising because the median earnings vary
across different occupations. Hence, we do nothing to the outliers and
proceed with the rest of the analysis.

![](Women-in-the-Workforce_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->

### Data Dictionary

**Dataset 1 - jobs\_gender**

<table class="table table-striped table-condensed table-responsive" style="width: auto !important; margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:left;">

Variable

</th>

<th style="text-align:left;">

Description

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

year

</td>

<td style="text-align:left;">

Year

</td>

</tr>

<tr>

<td style="text-align:left;">

occupation

</td>

<td style="text-align:left;">

Specific job/career

</td>

</tr>

<tr>

<td style="text-align:left;">

major\_category

</td>

<td style="text-align:left;">

Broad category of occupation

</td>

</tr>

<tr>

<td style="text-align:left;">

minor\_category

</td>

<td style="text-align:left;">

Fine category of occupation

</td>

</tr>

<tr>

<td style="text-align:left;">

total\_workers

</td>

<td style="text-align:left;">

Total estimated full-time workers \> 16 years old

</td>

</tr>

<tr>

<td style="text-align:left;">

workers\_male

</td>

<td style="text-align:left;">

Estimated MALE full-time workers \> 16 years old

</td>

</tr>

<tr>

<td style="text-align:left;">

workers\_female

</td>

<td style="text-align:left;">

Estimated FEMALE full-time workers \> 16 years old

</td>

</tr>

<tr>

<td style="text-align:left;">

percent\_female

</td>

<td style="text-align:left;">

The percent of females for specific occupation

</td>

</tr>

<tr>

<td style="text-align:left;">

total\_earnings

</td>

<td style="text-align:left;">

Total estimated median earnings for full-time workers \> 16 years old

</td>

</tr>

<tr>

<td style="text-align:left;">

total\_earnings\_male

</td>

<td style="text-align:left;">

Estimated MALE median earnings for full-time workers \> 16 years old

</td>

</tr>

<tr>

<td style="text-align:left;">

total\_earnings\_female

</td>

<td style="text-align:left;">

Estimated FEMALE median earnings for full-time workers \> 16 years old

</td>

</tr>

<tr>

<td style="text-align:left;">

wage\_percent\_of\_male

</td>

<td style="text-align:left;">

Female wages as percent of male wages - NA for occupations with small
sample size

</td>

</tr>

</tbody>

</table>

**Dataset 2 - earnings\_female**

<table class="table table-striped table-condensed table-responsive" style="width: auto !important; margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:left;">

Variable

</th>

<th style="text-align:left;">

Description

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

Year

</td>

<td style="text-align:left;">

Year

</td>

</tr>

<tr>

<td style="text-align:left;">

group

</td>

<td style="text-align:left;">

Age group

</td>

</tr>

<tr>

<td style="text-align:left;">

percent

</td>

<td style="text-align:left;">

Female salary percent of male salary

</td>

</tr>

</tbody>

</table>

**Dataset 3 - employed\_gender**

<table class="table table-striped table-condensed table-responsive" style="width: auto !important; margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:left;">

Variable

</th>

<th style="text-align:left;">

Description

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

year

</td>

<td style="text-align:left;">

Year

</td>

</tr>

<tr>

<td style="text-align:left;">

total\_full\_time

</td>

<td style="text-align:left;">

Percent of total employed people usually working full time

</td>

</tr>

<tr>

<td style="text-align:left;">

total\_part\_time

</td>

<td style="text-align:left;">

Percent of total employed people usually working part time

</td>

</tr>

<tr>

<td style="text-align:left;">

full\_time\_female

</td>

<td style="text-align:left;">

Percent of employed women usually working full time

</td>

</tr>

<tr>

<td style="text-align:left;">

part\_time\_female

</td>

<td style="text-align:left;">

Percent of employed women usually working part time

</td>

</tr>

<tr>

<td style="text-align:left;">

full\_time\_male

</td>

<td style="text-align:left;">

Percent of employed men usually working full time

</td>

</tr>

<tr>

<td style="text-align:left;">

part\_time\_male

</td>

<td style="text-align:left;">

Percent of employed men usually working part time

</td>

</tr>

</tbody>

</table>

## Exploratory Data Analysis

Our analysis and findings are presented in 3 categories: Industry based,
Gender based, and Time series based . These categories reveal the
different levels of analysis considered in this study.

### Industry based analysis

**Do women earn more than men in certain industries?**

We examine the industries where female workers earn more than their male
counterparts. It is observed that there are occupations in all of the 8
major categories where women earn more than men. For a more in-depth
analysis, we dug deeper to ascertain the number of jobs within each
specific industry that fit the criteria (women earn more than male
workers). Then, the 8 industries are ranked based on the identified
number of jobs.

The Natural Resources, Construction and Maintenace and Production and
the Transportation and Material Moving industries are ranked first and
second, respectively. Although these industries are traditionally
thought to be male-dominated, our findings reveal that female workers do
earn way more than men in certain jobs from these industries.

![](Women-in-the-Workforce_files/figure-gfm/unnamed-chunk-28-1.png)<!-- -->

**Are these industries male-dominated or female-dominated?**

An examination of the gender ratio in 8 industries where female workers
earn more than male workers show that these industries are
male-dominated. Also, female workers have a very small representation in
the top two industries- Natural Resources, Construction and Maintenace
and Production and Transportation and Material Moving.

Based on these findings, it could be suggested that the industries are
making active efforts to attract more women employees by offering higher
salaries.

![](Women-in-the-Workforce_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->

**How do women fare in the top paying jobs**

We wanted to examine, for the year 2016, women representation in jobs
that were considered the highest paying jobs of that year (The list of
the top-paying jobs were consolidated from sites such as Forbes and
Business Insider).

We notice that most of these jobs, barring 3 such as Pharmacists, Nurse
anesthetists, and Financial managers, are all predominantly male-driven.
Noticeably over 50% of these jobs have very poor female representation
(50% percent or under).

![](Women-in-the-Workforce_files/figure-gfm/unnamed-chunk-30-1.png)<!-- -->

### Gender based Analysis

**Female only vs male only occupations**

Historically there have always been jobs that have been influenced by
gender biasing. Jobs that involved heavy manual labor were always
male-driven whereas occupations such as midwifery were relegated to
women.

We wanted to capture if these biases still exist or if the gender gap
has been closed in recent years.

In 2013, 2014, and 2016, the Nurse midwives occupation only had female
workers. Meanwhile, five major job categories - Construction and
Extraction, material moving, Installation, Maintenance, and Repair,
Transportation, and protective services had only male workers during the
same period.

These observations align with the traditional gender-based roles and
unfortunately haven’t changed over the years.

Female only occupations:

    ##       occupation
    ## 1 Nurse midwives

Male only occupations:

    ##                          minor_category
    ## 1           Construction and Extraction
    ## 2                       Material Moving
    ## 3 Installation, Maintenance, and Repair
    ## 4                        Transportation
    ## 5                    Protective Service

**Does Equal representation indicate Equal Pay?**

For a more accurate comparison of the gender pay gap, we focus on
occupations with approximately equal gender representation in the
workforce. We see that four occupation types have equal representation
of men and women in the workforce. Despite this, women earn less than
men across all four occupations.

In this context, female workers who are food cooking machine operators
and tenders face the widest gender pay gap. Their wages are about 62.3
percent of male workers’ wages. These findings suggest that an **equal
representation may not necessarily mean equal earnings** or a lack of
gender pay gap.

<table class="table table-striped table-condensed table-responsive" style="width: auto !important; margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:left;">

occupation

</th>

<th style="text-align:right;">

male\_representation

</th>

<th style="text-align:right;">

female\_representation

</th>

<th style="text-align:right;">

wage\_percent\_of\_male

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

Operations research analysts

</td>

<td style="text-align:right;">

50

</td>

<td style="text-align:right;">

50

</td>

<td style="text-align:right;">

88.42

</td>

</tr>

<tr>

<td style="text-align:left;">

Postal service clerks

</td>

<td style="text-align:right;">

50

</td>

<td style="text-align:right;">

50

</td>

<td style="text-align:right;">

95.03

</td>

</tr>

<tr>

<td style="text-align:left;">

Postal service mail sorters, processors, and processing machine
operators

</td>

<td style="text-align:right;">

50

</td>

<td style="text-align:right;">

50

</td>

<td style="text-align:right;">

91.35

</td>

</tr>

<tr>

<td style="text-align:left;">

Food cooking machine operators and tenders

</td>

<td style="text-align:right;">

50

</td>

<td style="text-align:right;">

50

</td>

<td style="text-align:right;">

62.26

</td>

</tr>

</tbody>

</table>

### Time series analysis

**Do women face wider pay gap as they get older?**

The chart below provides insights into three of our objectives.

1.  We see an upward trend in the percentage of female earnings to male
    earnings, despite some observed fluctuations. This shows that the
    gender pay gap has decreased over the years.

2.  Women of all age groups earn lesser than their male counterparts.
    The chart reveals that none of the reported percentages was equal to
    100 percent or more. This suggests that no matter what age group a
    woman belonged to, she is still subjected to gender-based
    differences in earnings.

3.  Younger female workers between the ages of 16 and 24 face lesser
    gender-based pay differences compared to older female workers. This
    suggests that as women grow older, they tend to make lesser than
    male workers. Based on this finding, it may be assumed that for
    jobs, with a younger workforce, the earnings for both genders is
    closer than jobs or positions with older workers.

![](Women-in-the-Workforce_files/figure-gfm/unnamed-chunk-35-1.png)<!-- -->

**Full-time/ Part-time represenation based on gender**

The proportion of full-/part-time workers is examined across both
genders to gain some insights on how the increase/decrease in full-time
workers may have contributed to the gender pay gap. From 2000-2016, we
notice that the percentage of men working full time is greater than the
percentage of women working full time.

We see a dip in the percentage of both male and female full-time
percentage between 2008 and 2009 and an increase in part-time percentage
between 2008 and 2009, suggesting the likely effect of the 2008
financial crisis.

On the other hand, female workers are more represented as part-time
workers than male workers. This may be an important contributor to the
lower total female median earnings. Although beyond the scope of our
study, this may be worth investigating.

![](Women-in-the-Workforce_files/figure-gfm/unnamed-chunk-36-1.png)<!-- -->
![](Women-in-the-Workforce_files/figure-gfm/unnamed-chunk-37-1.png)<!-- -->

## Summary

This study examined the gender bias in earnings in the US, with a focus
on female workers. Using the [Women in
Workforce](https://github.com/rfordatascience/tidytuesday/blob/master/data/2019/2019-03-05/readme.md)
data, we explored the trend and how women of different age groups are
affected.In addition, we highlighted industries in which occupations
where female workers earn more than male workers. We ranked these
industries based on the number of occupation types with female earnings
greater than male earnings and indicated if the industries are male- or
female-dominated.

Our results show that the gender pay gap has decreased over the years,
but women workers still earn lesser than their male counterparts across
all the age categories. Younger female workers between the ages of 16
and 24 face lesser gender-based pay differences compared to older female
workers. Over the years, female workers have had more representation as
part-time workers than male workers, which may be an important
contributor to the lower total female median earnings.

The **Natural Resources, Construction and Maintenace and Production and
the Transportation and Material Moving industries** have the two highest
number of occupation groups where women earn more than men. In
conclusion, an equal representation of both genders in an occupation
type may not necessarily mean equal earnings or a lack of gender pay
gap.

**Practical Implications**

Our study provides workers and stakeholders with more insights on
gender-based earning differences across industries, occupations, and age
groups in the US. We highlight how women of all ages are affected by
this bias. We hope that other industries emulate the Natural Resources,
Construction and Maintenace and Production and the Transportation and
Material Moving industries in eliminating the gender pay gap and
rewarding workers based on their abilities and contributions, rather
than their gender.

**Limitations of Study**

The absence of extensive years of data was a major limitation of our
study. The data that we worked with was limited to a certain number of
variables which made it impossible to draw generalizable inferences. In
addition, we didn’t have access to up to date data, hence our findings
can only be extended to 2016, without a good picture of what has
happened in the most recent years.

**Future Research Direction**

Though we found that female workers are more represented as part-time
workers than male workers, we couldn’t justify if this is an important
contributor to the lower total female median earnings because this is
beyond the scope of our study and data. This is worth investigating and
could be an area of improvement and future research.
