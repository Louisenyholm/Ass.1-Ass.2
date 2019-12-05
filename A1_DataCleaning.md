Assignment 1, Language development in Autism Spectrum Disorder (ASD) - Brushing up your code skills
===================================================================================================

In this first part of the assignment we will brush up your programming
skills, and make you familiar with the data sets you will be analysing
for the next parts of the assignment.

In this warm-up assignment you will: 1) Create a Github (or gitlab)
account, link it to your RStudio, and create a new repository/project 2)
Use small nifty lines of code to transform several data sets into just
one. The final data set will contain only the variables that are needed
for the analysis in the next parts of the assignment 3) Warm up your
tidyverse skills (especially the sub-packages stringr and dplyr), which
you will find handy for later assignments.

N.B: Usually you’ll also have to doc/pdf with a text. Not for Assignment
1.

Learning objectives:
--------------------

-   Become comfortable with tidyverse (and R in general)
-   Test out the git integration with RStudio
-   Build expertise in data wrangling (which will be used in future
    assignments)

0. First an introduction on the data
------------------------------------

Language development in Autism Spectrum Disorder (ASD)
======================================================

Reference to the study:
<a href="https://www.ncbi.nlm.nih.gov/pubmed/30396129" class="uri">https://www.ncbi.nlm.nih.gov/pubmed/30396129</a>

Background: Autism Spectrum Disorder (ASD) is often related to language
impairment, and language impairment strongly affects the patients
ability to function socially (maintaining a social network, thriving at
work, etc.). It is therefore crucial to understand how language
abilities develop in children with ASD, and which factors affect them
(to figure out e.g. how a child will develop in the future and whether
there is a need for language therapy). However, language impairment is
always quantified by relying on the parent, teacher or clinician
subjective judgment of the child, and measured very sparcely (e.g. at 3
years of age and again at 6).

In this study we videotaped circa 30 kids with ASD and circa 30
comparison kids (matched by linguistic performance at visit 1) for ca.
30 minutes of naturalistic interactions with a parent. We repeated the
data collection 6 times per kid, with 4 months between each visit. We
transcribed the data and counted: i) the amount of words that each kid
uses in each video. Same for the parent. ii) the amount of unique words
that each kid uses in each video. Same for the parent. iii) the amount
of morphemes per utterance (Mean Length of Utterance) displayed by each
child in each video. Same for the parent.

Different researchers involved in the project provide you with different
datasets: 1) demographic and clinical data about the children (recorded
by a clinical psychologist) 2) length of utterance data (calculated by a
linguist) 3) amount of unique and total words used (calculated by a
fumbling jack-of-all-trade, let’s call him RF)

Your job in this assignment is to double check the data and make sure
that it is ready for the analysis proper (Assignment 2), in which we
will try to understand how the children’s language develops as they grow
as a function of cognitive and social factors and which are the “cues”
suggesting a likely future language impairment.

1. Let’s get started on GitHub
------------------------------

In the assignments you will be asked to upload your code on Github and
the GitHub repositories will be part of the portfolio, therefore all
students must make an account and link it to their RStudio (you’ll thank
us later for this!).

Follow the link to one of the tutorials indicated in the syllabus: \*
Recommended:
<a href="https://happygitwithr.com/" class="uri">https://happygitwithr.com/</a>
\* Alternative (if the previous doesn’t work):
<a href="https://support.rstudio.com/hc/en-us/articles/200532077-Version-Control-with-Git-and-SVN" class="uri">https://support.rstudio.com/hc/en-us/articles/200532077-Version-Control-with-Git-and-SVN</a>
\* Alternative (if the previous doesn’t work):
<a href="https://docs.google.com/document/d/1WvApy4ayQcZaLRpD6bvAqhWncUaPmmRimT016-PrLBk/mobilebasic" class="uri">https://docs.google.com/document/d/1WvApy4ayQcZaLRpD6bvAqhWncUaPmmRimT016-PrLBk/mobilebasic</a>

N.B. Create a GitHub repository for the Assignment 1 and link it to a
project on your RStudio.

2. Now let’s take dirty dirty data sets and make them into a tidy one
---------------------------------------------------------------------

If you’re not in a project in Rstudio, make sure to set your working
directory here. If you created an RStudio project, then your working
directory (the directory with your data and code for these assignments)
is the project directory.

``` r
pacman::p_load(tidyverse,janitor)
```

Load the three data sets, after downloading them from dropbox and saving
them in your working directory: \* Demographic data for the
participants:
<a href="https://www.dropbox.com/s/w15pou9wstgc8fe/demo_train.csv?dl=0" class="uri">https://www.dropbox.com/s/w15pou9wstgc8fe/demo_train.csv?dl=0</a>
\* Length of utterance data:
<a href="https://www.dropbox.com/s/usyauqm37a76of6/LU_train.csv?dl=0" class="uri">https://www.dropbox.com/s/usyauqm37a76of6/LU_train.csv?dl=0</a>
\* Word data:
<a href="https://www.dropbox.com/s/8ng1civpl2aux58/token_train.csv?dl=0" class="uri">https://www.dropbox.com/s/8ng1civpl2aux58/token_train.csv?dl=0</a>

``` r
LU <- read_csv("LU_train.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   SUBJ = col_character(),
    ##   VISIT = col_character(),
    ##   MOT_MLU = col_double(),
    ##   MOT_LUstd = col_double(),
    ##   MOT_LU_q1 = col_double(),
    ##   MOT_LU_q2 = col_double(),
    ##   MOT_LU_q3 = col_double(),
    ##   CHI_MLU = col_double(),
    ##   CHI_LUstd = col_double(),
    ##   CHI_LU_q1 = col_double(),
    ##   CHI_LU_q2 = col_double(),
    ##   CHI_LU_q3 = col_double()
    ## )

``` r
demo <- read_csv("demo_train.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   Child.ID = col_character(),
    ##   Ethnicity = col_character(),
    ##   Diagnosis = col_character(),
    ##   Birthdate = col_character()
    ## )

    ## See spec(...) for full column specifications.

``` r
token <- read_csv("token_train.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   SUBJ = col_character(),
    ##   VISIT = col_character(),
    ##   types_MOT = col_double(),
    ##   types_CHI = col_double(),
    ##   types_shared = col_double(),
    ##   tokens_MOT = col_double(),
    ##   tokens_CHI = col_double(),
    ##   X = col_logical()
    ## )

Explore the 3 datasets (e.g. visualize them, summarize them, etc.). You
will see that the data is messy, since the psychologist collected the
demographic data, the linguist analyzed the length of utterance in May
2014 and the fumbling jack-of-all-trades analyzed the words several
months later. In particular: - the same variables might have different
names (e.g. participant and visit identifiers) - the same variables
might report the values in different ways (e.g. participant and visit
IDs) Welcome to real world of messy data :-)

Before being able to combine the data sets we need to make sure the
relevant variables have the same names and the same kind of values.

So:

2a. Identify which variable names do not match (that is are spelled
differently) and find a way to transform variable names. Pay particular
attention to the variables indicating participant and visit.

Tip: look through the chapter on data transformation in R for data
science
(<a href="http://r4ds.had.co.nz" class="uri">http://r4ds.had.co.nz</a>).
Alternatively you can look into the package dplyr (part of tidyverse),
or google “how to rename variables in R”. Or check the janitor R
package. There are always multiple ways of solving any problem and no
absolute best method.

``` r
# Changing colnames/variable names to make them consistent
colnames(demo)[1] <- "SUBJ"
colnames(demo)[2] <- "VISIT"

# Making all variables uppercase in all the dataframes
x <- c(1:36)
names(demo)[x] <- toupper(names(demo)[x])

y <- c(1:12)
names(LU)[y] <- toupper(names(LU)[y])

z <- c(1:8)
names(token)[z] <- toupper(names(token)[z])
```

2b. Find a way to homogeneize the way “visit” is reported (visit1
vs. 1).

Tip: The stringr package is what you need. str\_extract () will allow
you to extract only the digit (number) from a string, by using the
regular expression \\d.

``` r
#extracts everything the the given row(s) besides the digit (d), one d = one digit
token$VISIT <- str_extract(token$VISIT, "\\d")
LU$VISIT <- str_extract(token$VISIT, "\\d")
```

2c. We also need to make a small adjustment to the content of the
Child.ID coloumn in the demographic data. Within this column, names that
are not abbreviations do not end with “.” (i.e. Adam), which is the case
in the other two data sets (i.e. Adam.). If The content of the two
variables isn’t identical the rows will not be merged. A neat way to
solve the problem is simply to remove all “.” in all datasets.

Tip: stringr is helpful again. Look up str\_replace\_all Tip: You can
either have one line of code for each child name that is to be changed
(easier, more typing) or specify the pattern that you want to match
(more complicated: look up “regular expressions”, but less typing)

``` r
#Function looks for "punktummer" ("\\.") and then replace them with nothing ("").
token$SUBJ <- str_replace_all(token$SUBJ, "\\.", "")

demo$SUBJ <- str_replace_all(demo$SUBJ, "\\.", "")

LU$SUBJ <- str_replace_all(LU$SUBJ, "\\.", "")
```

2d. Now that the nitty gritty details of the different data sets are
fixed, we want to make a subset of each data set only containig the
variables that we wish to use in the final data set. For this we use the
tidyverse package dplyr, which contains the function select().

The variables we need are: \* Child.ID, \* Visit, \* Diagnosis, \*
Ethnicity, \* Gender, \* Age, \* ADOS,  
\* MullenRaw, \* ExpressiveLangRaw, \* Socialization \* MOT\_MLU, \*
CHI\_MLU, \* types\_MOT, \* types\_CHI, \* tokens\_MOT, \* tokens\_CHI.

Most variables should make sense, here the less intuitive ones. \* ADOS
(Autism Diagnostic Observation Schedule) indicates the severity of the
autistic symptoms (the higher the score, the worse the symptoms). Ref:
<a href="https://link.springer.com/article/10.1023/A:1005592401947" class="uri">https://link.springer.com/article/10.1023/A:1005592401947</a>
\* MLU stands for mean length of utterance (usually a proxy for
syntactic complexity) \* types stands for unique words (e.g. even if
“doggie” is used 100 times it only counts for 1) \* tokens stands for
overall amount of words (if “doggie” is used 100 times it counts for
100) \* MullenRaw indicates non verbal IQ, as measured by Mullen Scales
of Early Learning (MSEL
<a href="https://link.springer.com/referenceworkentry/10.1007%2F978-1-4419-1698-3_596" class="uri">https://link.springer.com/referenceworkentry/10.1007%2F978-1-4419-1698-3_596</a>)
\* ExpressiveLangRaw indicates verbal IQ, as measured by MSEL \*
Socialization indicates social interaction skills and social
responsiveness, as measured by Vineland
(<a href="https://cloudfront.ualberta.ca/-/media/ualberta/faculties-and-programs/centres-institutes/community-university-partnership/resources/tools---assessment/vinelandjune-2012.pdf" class="uri">https://cloudfront.ualberta.ca/-/media/ualberta/faculties-and-programs/centres-institutes/community-university-partnership/resources/tools---assessment/vinelandjune-2012.pdf</a>)

Feel free to rename the variables into something you can remember
(i.e. nonVerbalIQ, verbalIQ)

``` r
#Now only the needed variables (stated in etxt above) are selected to be included in the dataframe, to avoid unnecessary variables.
demo_select <- demo %>% select(c(1:4, 7, 9, 14, 22, 24, 33))
LU_select <- LU %>% select(c(1:3, 8))
token_select <- token %>% select(c(1:4, 6, 7))
```

2e. Finally we are ready to merge all the data sets into just one.

Some things to pay attention to: \* make sure to check that the merge
has included all relevant data (e.g. by comparing the number of rows) \*
make sure to understand whether (and if so why) there are NAs in the
dataset (e.g. some measures were not taken at all visits, some
recordings were lost or permission to use was withdrawn)

``` r
# Merging by subject and visit - (all = TRUE (el. all.x = T og all.y = T) --> matches everything that can be matched, and the remaining empty spaces there might be are filled out with NAs.

df <- merge(demo_select, LU_select, by = c("SUBJ", "VISIT"), all = TRUE)
df2 <- merge(df, token_select, by = c("SUBJ", "VISIT"), all = TRUE)
```

2f. Only using clinical measures from Visit 1 In order for our models to
be useful, we want to miimize the need to actually test children as they
develop. In other words, we would like to be able to understand and
predict the children’s linguistic development after only having tested
them once. Therefore we need to make sure that our ADOS, MullenRaw,
ExpressiveLangRaw and Socialization variables are reporting (for all
visits) only the scores from visit 1.

A possible way to do so: \* create a new dataset with only visit 1,
child id and the 4 relevant clinical variables to be merged with the old
dataset \* rename the clinical variables (e.g. ADOS to ADOS1) and remove
the visit (so that the new clinical variables are reported for all 6
visits) \* merge the new dataset with the old

``` r
# Creating dataset, which only contains data from visit 1s
visit1 <- df2 %>% 
  filter(VISIT == 1) %>% # Filtering the rows containing 1 in the "VISIT"-column.
  select(c(1, 7:10)) # Selecting the variables of interest (and ID).

#renaming columns to show they contain data only from visit 1
colnames(visit1) <- c("SUBJ", sapply(2:5, function(x) paste(colnames(visit1)[x], "1", sep = "")))

# Merging the new dataset with the old
df3 <- merge(visit1, df2, by = "SUBJ", all = T)
```

2g. Final touches

Now we want to \* anonymize our participants (they are real children!).
\* make sure the variables have sensible values. E.g. right now gender
is marked 1 and 2, but in two weeks you will not be able to remember,
which gender were connected to which number, so change the values from 1
and 2 to F and M in the gender variable. For the same reason, you should
also change the values of Diagnosis from A and B to ASD (autism spectrum
disorder) and TD (typically developing). Tip: Try taking a look at
ifelse(), or google “how to rename levels in R”. \* Save the data set
using into a csv file. Hint: look into write.csv()

``` r
# Anonymising participants
df3$SUBJ <- as.numeric(as.factor(df3$SUBJ)) # Assigning each participant a number by making the name a factor, and then a number.

# Making sure, variables have sensible values
df3$GENDER <- ifelse(df3$GENDER == 1, "M", "F") # "M" instead of 1 - indicating "Male"

df3$DIAGNOSIS <- ifelse(df3$DIAGNOSIS == "A", "ASD", "TD") # "ASD" instead of "A" - indicating "Autism Spectrum Disorder", "TD" instead of "B" - indicating Typical Development.

# Saving the data set into a csv file
write.csv(df3, "LangDevASD.csv")
```

1.  BONUS QUESTIONS The aim of this last section is to make sure you are
    fully fluent in the tidyverse. Here’s the link to a very helpful
    book, which explains each function:
    <a href="http://r4ds.had.co.nz/index.html" class="uri">http://r4ds.had.co.nz/index.html</a>

2.  USING FILTER List all kids who:

<!-- -->

1.  have a mean length of utterance (across all visits) of more than 2.7
    morphemes.
2.  have a mean length of utterance of less than 1.5 morphemes at the
    first visit
3.  have not completed all trials. Tip: Use pipes to solve this

``` r
# Task 1 = 11
df3 %>% 
  group_by(SUBJ) %>% 
  summarise(mMLU = mean(CHI_MLU, na.rm = T)) %>% 
  filter(mMLU > 2.7)
```

    ## # A tibble: 11 x 2
    ##     SUBJ  mMLU
    ##    <dbl> <dbl>
    ##  1     3  3.31
    ##  2     4  3.02
    ##  3     5  3.13
    ##  4    14  2.75
    ##  5    20  2.84
    ##  6    28  2.73
    ##  7    29  3.50
    ##  8    45  2.75
    ##  9    54  2.94
    ## 10    58  3.16
    ## 11    65  3.26

``` r
# Task 2 = 47
df3 %>% 
  filter(VISIT == 1 & CHI_MLU < 1.5)
```

    ##    SUBJ ADOS1 SOCIALIZATION1 MULLENRAW1 EXPRESSIVELANGRAW1 VISIT
    ## 1     2     0            108         28                 14     1
    ## 2     6     9             82         34                 27     1
    ## 3     7    17             68         20                 17     1
    ## 4     8    18             65         24                 14     1
    ## 5    10     0            100         24                 18     1
    ## 6    11     3            104         27                 18     1
    ## 7    13     0            106         21                 15     1
    ## 8    14     0            104         30                 16     1
    ## 9    16     0             92         23                 17     1
    ## 10   17     0             86         24                 15     1
    ## 11   19    14             74         25                 11     1
    ## 12   21    11             88         28                 20     1
    ## 13   22     9             86         26                 14     1
    ## 14   23    21             72         22                  8     1
    ## 15   25     0            106         21                 19     1
    ## 16   26    14             65         25                 19     1
    ## 17   27    20             67         13                 11     1
    ## 18   30     0             96         26                 18     1
    ## 19   31     0            102         20                 16     1
    ## 20   32    17             72         26                 14     1
    ## 21   33    12             70         31                 13     1
    ## 22   36    14             76         25                 11     1
    ## 23   37    10             82         27                 22     1
    ## 24   38     1            102         23                 21     1
    ## 25   39     3             98         19                 13     1
    ## 26   40     7             70         33                 26     1
    ## 27   41     1            104         29                 28     1
    ## 28   42    11             88         26                 19     1
    ## 29   43    13             86         27                 13     1
    ## 30   44     0            102         25                 17     1
    ## 31   45     0             96         24                 19     1
    ## 32   47     3             94         30                 20     1
    ## 33   48     5            113         24                 20     1
    ## 34   50    20             75         21                  9     1
    ## 35   52     0            102         27                 20     1
    ## 36   53    17             82         28                 10     1
    ## 37   54     0            108         27                 27     1
    ## 38   55    19             64         17                 10     1
    ## 39   56     1            102         22                 14     1
    ## 40   57     0             94         29                 22     1
    ## 41   59     1            115         24                 22     1
    ## 42   60     0             96         26                 17     1
    ## 43   61    14             77         27                 11     1
    ## 44   62    15             66         28                 10     1
    ## 45   63    15             88         30                 24     1
    ## 46   64     4            108         29                 22     1
    ## 47   66    15             79         27                 16     1
    ##           ETHNICITY DIAGNOSIS GENDER   AGE ADOS SOCIALIZATION MULLENRAW
    ## 1             White        TD      M 19.80    0           108        28
    ## 2             White       ASD      M 34.03    9            82        34
    ## 3       Bangladeshi       ASD      F 26.17   17            68        20
    ## 4             White       ASD      F 41.00   18            65        24
    ## 5             White        TD      F 18.30    0           100        24
    ## 6             White        TD      M 19.27    3           104        27
    ## 7             White        TD      M 19.23    0           106        21
    ## 8             White        TD      M 20.07    0           104        30
    ## 9             White        TD      M 18.97    0            92        23
    ## 10            White        TD      M 19.27    0            86        24
    ## 11            White       ASD      M 34.80   14            74        25
    ## 12            White       ASD      M 35.80   11            88        28
    ## 13            White       ASD      M 18.77    9            86        26
    ## 14 African American       ASD      M 27.53   21            72        22
    ## 15            White        TD      F 18.93    0           106        21
    ## 16     White/Latino       ASD      M 27.37   14            65        25
    ## 17            White       ASD      M 37.47   20            67        13
    ## 18            White        TD      M 21.03    0            96        26
    ## 19            White        TD      M 19.93    0           102        20
    ## 20            White       ASD      M 34.87   17            72        26
    ## 21            White       ASD      M 36.53   12            70        31
    ## 22 African American       ASD      F 25.33   14            76        25
    ## 23            White       ASD      M 33.77   10            82        27
    ## 24            White        TD      M 19.30    1           102        23
    ## 25            White        TD      M 19.20    3            98        19
    ## 26            White       ASD      M 39.50    7            70        33
    ## 27            White        TD      F 19.87    1           104        29
    ## 28      White/Asian       ASD      M 33.20   11            88        26
    ## 29         Lebanese       ASD      M 24.90   13            86        27
    ## 30            White        TD      M 19.23    0           102        25
    ## 31            White        TD      M 19.37    0            96        24
    ## 32            White        TD      M 19.77    3            94        30
    ## 33            White        TD      M 20.03    5           113        24
    ## 34            White       ASD      M 36.73   20            75        21
    ## 35            White        TD      F 20.03    0           102        27
    ## 36            White       ASD      M 31.63   17            82        28
    ## 37            White        TD      M 23.07    0           108        27
    ## 38            White       ASD      M 37.47   19            64        17
    ## 39            Asian        TD      F 20.87    1           102        22
    ## 40            White        TD      M 22.57    0            94        29
    ## 41            White        TD      M 19.10    1           115        24
    ## 42            White        TD      M 19.97    0            96        26
    ## 43            White       ASD      M 35.50   14            77        27
    ## 44            White       ASD      F 41.07   15            66        28
    ## 45            White       ASD      M 26.00   15            88        30
    ## 46            White        TD      M 20.80    4           108        29
    ## 47            White       ASD      M 42.00   15            79        27
    ##    EXPRESSIVELANGRAW  MOT_MLU   CHI_MLU TYPES_MOT TYPES_CHI TOKENS_MOT
    ## 1                 14 3.621993 1.2522523       378        14       1835
    ## 2                 27 3.986357 1.3947368       324        57       2859
    ## 3                 17 2.618729 1.0000000       212         4        761
    ## 4                 14 2.244755 1.2641509       152        29        578
    ## 5                 18 3.544419 1.0378788       363        36       1408
    ## 6                 18 4.204846 1.0375000       289        15       1808
    ## 7                 15 3.380463 1.2168675       215        24       1136
    ## 8                 16 4.195335 1.0877193       235        17       1262
    ## 9                 17 3.420315 1.0396040       287         7       1625
    ## 10                15 3.967078 1.1647059       277        27       1643
    ## 11                11 3.182390 1.0277778       281         9       1418
    ## 12                20 2.539823 1.3595506       283        89       1019
    ## 13                14 2.524740 0.1857143       321        16       1787
    ## 14                 8 4.390879 1.0000000       485         8       2826
    ## 15                19 3.630476 1.2371134       343        36       1698
    ## 16                19 3.024548 1.4324324       328        41       2138
    ## 17                11 2.917355 1.0833333       193         6        654
    ## 18                18 3.616867 1.3661972       317        37       1361
    ## 19                16 2.788793 1.2758621       178        34        584
    ## 20                14 3.304189 1.0086207       295         6       1643
    ## 21                13 3.607088 0.9000000       366        11       2054
    ## 22                11 2.287293 1.2500000       206        13        788
    ## 23                22 2.743455 1.3766234       214        67        893
    ## 24                21 3.561364 1.2641509       291        24       1344
    ## 25                13 3.921109 1.2307692       281         8       1631
    ## 26                26 4.135036 0.4805825       381        39       1988
    ## 27                28 3.420975 1.3322785       342        96       2035
    ## 28                19 3.298748 1.2043011       274        36       1537
    ## 29                13 2.997050 1.0175439       252         9       1827
    ## 30                17 3.093146 1.0262009       333        32       1547
    ## 31                19 4.033333 1.3034483       373        47       2334
    ## 32                20 3.088757 1.2592593       275         9       1417
    ## 33                20 2.776181 0.5584416       258        20       1188
    ## 34                 9 4.883966 1.1666667       387        10       2144
    ## 35                20 3.943005 1.0761421       260        13       1347
    ## 36                10 3.765528 1.2500000       303        17       2147
    ## 37                27 4.030075 1.4258373       255        73       1452
    ## 38                10 3.704110 1.1000000       265         8       1215
    ## 39                14 3.435743 1.1818182       331         7       1503
    ## 40                22 5.344227 1.4086957       441        92       2267
    ## 41                22 3.487871 1.3139535       214        32       1118
    ## 42                17 3.509138 1.1846154       178        11       1144
    ## 43                11 2.548969 1.0444444       195         9        955
    ## 44                10 3.833770 0.0000000       386         0       2613
    ## 45                24 2.747100 1.1809045       338        98       2084
    ## 46                22 3.432387 1.1830065       345        62       1805
    ## 47                16 3.030405 1.0375000       303        15       1579
    ##    TOKENS_CHI
    ## 1         139
    ## 2         197
    ## 3          29
    ## 4         130
    ## 5         137
    ## 6          83
    ## 7         101
    ## 8          62
    ## 9         105
    ## 10         99
    ## 11         37
    ## 12        227
    ## 13        214
    ## 14        122
    ## 15        118
    ## 16        103
    ## 17         26
    ## 18         95
    ## 19        111
    ## 20        117
    ## 21         21
    ## 22         35
    ## 23        180
    ## 24        260
    ## 25         16
    ## 26        337
    ## 27        398
    ## 28        109
    ## 29         58
    ## 30        235
    ## 31        176
    ## 32         68
    ## 33         91
    ## 34         63
    ## 35        212
    ## 36         40
    ## 37        286
    ## 38         43
    ## 39         38
    ## 40        319
    ## 41        109
    ## 42        154
    ## 43         47
    ## 44          0
    ## 45        233
    ## 46        244
    ## 47        166

``` r
# Task 3 = 18
df3 %>%
  group_by(SUBJ) %>%
  summarise(na = sum(is.na(CHI_MLU)), visits = n()) %>% 
  filter(na > 0 | visits < 6)
```

    ## # A tibble: 18 x 3
    ##     SUBJ    na visits
    ##    <dbl> <int>  <int>
    ##  1     1     1      1
    ##  2     3     1      6
    ##  3     8     1      6
    ##  4     9     1      6
    ##  5    10     1      6
    ##  6    12     2      2
    ##  7    19     1      6
    ##  8    24     3      3
    ##  9    29     1      6
    ## 10    41     1      6
    ## 11    43     1      6
    ## 12    47     1      6
    ## 13    48     0      5
    ## 14    49     2      2
    ## 15    51     2      2
    ## 16    53     0      5
    ## 17    60     1      6
    ## 18    61     0      4

USING ARRANGE

1.  Sort kids to find the kid who produced the most words on the 6th
    visit
2.  Sort kids to find the kid who produced the least amount of words on
    the 1st visit.

``` r
# Task 1 = Subject 59 = 1294 words in total
df3 %>% 
  filter(VISIT == 6) %>%
  arrange(desc(TOKENS_CHI))
```

    ##    SUBJ ADOS1 SOCIALIZATION1 MULLENRAW1 EXPRESSIVELANGRAW1 VISIT
    ## 1    60     0             96         26                 17     6
    ## 2    29    11            100         32                 33     6
    ## 3    20     0            106         29                 33     6
    ## 4    28     0             90         29                 22     6
    ## 5    46    14             65         42                 27     6
    ## 6    40     7             70         33                 26     6
    ## 7    15     0            102         25                 17     6
    ## 8    25     0            106         21                 19     6
    ## 9    16     0             92         23                 17     6
    ## 10    5     8             82         31                 27     6
    ## 11   45     0             96         24                 19     6
    ## 12   65    13             87         30                 30     6
    ## 13   18     0            102         29                 26     6
    ## 14   44     0            102         25                 17     6
    ## 15    3    13             85         34                 27     6
    ## 16    2     0            108         28                 14     6
    ## 17   13     0            106         21                 15     6
    ## 18   64     4            108         29                 22     6
    ## 19   54     0            108         27                 27     6
    ## 20   14     0            104         30                 16     6
    ## 21   63    15             88         30                 24     6
    ## 22   38     1            102         23                 21     6
    ## 23   37    10             82         27                 22     6
    ## 24   22     9             86         26                 14     6
    ## 25   43    13             86         27                 13     6
    ## 26   39     3             98         19                 13     6
    ## 27    6     9             82         34                 27     6
    ## 28   30     0             96         26                 18     6
    ## 29   31     0            102         20                 16     6
    ## 30   59     1            115         24                 22     6
    ## 31    4     1             88         29                 18     6
    ## 32   57     0             94         29                 22     6
    ## 33   34     0            100         27                 22     6
    ## 34   17     0             86         24                 15     6
    ## 35   56     1            102         22                 14     6
    ## 36   58     0             98         30                 30     6
    ## 37   53    17             82         28                 10     6
    ## 38    7    17             68         20                 17     6
    ## 39   21    11             88         28                 20     6
    ## 40   26    14             65         25                 19     6
    ## 41   66    15             79         27                 16     6
    ## 42   32    17             72         26                 14     6
    ## 43    9     5            102         32                 31     6
    ## 44   33    12             70         31                 13     6
    ## 45   11     3            104         27                 18     6
    ## 46   27    20             67         13                 11     6
    ## 47   50    20             75         21                  9     6
    ## 48   42    11             88         26                 19     6
    ## 49   35    21             69         21                  9     6
    ## 50   36    14             76         25                 11     6
    ## 51   62    15             66         28                 10     6
    ## 52   52     0            102         27                 20     6
    ## 53   19    14             74         25                 11     6
    ## 54   61    14             77         27                 11     6
    ## 55   55    19             64         17                 10     6
    ## 56   23    21             72         22                  8     6
    ## 57    8    18             65         24                 14     6
    ## 58   10     0            100         24                 18     6
    ## 59   41     1            104         29                 28     6
    ## 60   47     3             94         30                 20     6
    ##           ETHNICITY DIAGNOSIS GENDER   AGE ADOS SOCIALIZATION MULLENRAW
    ## 1             White        TD      M 41.93   NA            95        45
    ## 2             White       ASD      M 51.00   NA            90        46
    ## 3             White        TD      M 44.07   NA           116        50
    ## 4             White        TD      M 42.47   NA           103        48
    ## 5             White       ASD      M 57.37   NA            77        49
    ## 6             White       ASD      M 58.77   NA           116        50
    ## 7             White        TD      M 39.40   NA            92        47
    ## 8             White        TD      F 39.23   NA           110        43
    ## 9             White        TD      M 39.43   NA            83        44
    ## 10     White/Latino       ASD      M 51.37   NA            74        44
    ## 11            White        TD      M 40.13   NA           101        46
    ## 12            White       ASD      M 54.73   NA            97        50
    ## 13            White        TD      M 42.93   NA            97        49
    ## 14            White        TD      M 39.93   NA           101        45
    ## 15            White       ASD      M 49.70   NA            81        48
    ## 16            White        TD      M 40.13   NA           100        42
    ## 17            White        TD      M 40.27   NA           101        42
    ## 18            White        TD      M 41.00   NA            NA        NA
    ## 19            White        TD      M 43.40   NA           101        45
    ## 20            White        TD      M 41.50   NA            97        44
    ## 21            White       ASD      M 46.07   NA            79        45
    ## 22            White        TD      M 39.07   NA            97        45
    ## 23            White       ASD      M 53.77   NA            92        44
    ## 24            White       ASD      M 37.30   NA           103        43
    ## 25         Lebanese       ASD      M 46.40   NA            94        41
    ## 26            White        TD      M 38.53   NA            97        46
    ## 27            White       ASD      M 54.13   NA            75        40
    ## 28            White        TD      M 41.17   NA           106        43
    ## 29            White        TD      M 39.43   NA            95        39
    ## 30            White        TD      M 39.30   NA           101        41
    ## 31            White        TD      F 45.07   NA            86        45
    ## 32            White        TD      M 43.03   NA            97        47
    ## 33            White        TD      M 43.80   NA           108        45
    ## 34            White        TD      M 40.30   NA            94        40
    ## 35            Asian        TD      F 42.10   NA           108        46
    ## 36            White        TD      M 44.43   NA           101        45
    ## 37            White       ASD      M    NA   NA            63        32
    ## 38      Bangladeshi       ASD      F 46.53   NA            74        39
    ## 39            White       ASD      M 56.73   NA            72        30
    ## 40     White/Latino       ASD      M 47.50   NA            65        30
    ## 41            White       ASD      M 62.33   NA           103        41
    ## 42            White       ASD      M 55.17   NA            72        39
    ## 43            White        TD      M 40.17   NA           108        43
    ## 44            White       ASD      M 56.43   NA            85        49
    ## 45            White        TD      M 40.43   NA           103        33
    ## 46            White       ASD      M 62.40   NA            68        30
    ## 47            White       ASD      M 57.43   NA            59        32
    ## 48      White/Asian       ASD      M 53.63   NA            63        30
    ## 49            White       ASD      M 54.63   NA            66        28
    ## 50 African American       ASD      F 46.17   NA            83        33
    ## 51            White       ASD      F 61.70   NA            68        32
    ## 52            White        TD      F 40.37   NA           101        43
    ## 53            White       ASD      M 54.43   NA            65        27
    ## 54            White       ASD      M    NA   NA            NA        34
    ## 55            White       ASD      M 62.40   NA            66        24
    ## 56 African American       ASD      M 48.97   NA            65        26
    ## 57            White       ASD      F 60.33   NA            59        32
    ## 58            White        TD      F    NA   NA            NA        NA
    ## 59            White        TD      F 40.23   NA           114        49
    ## 60            White        TD      M 39.93   NA           118        40
    ##    EXPRESSIVELANGRAW  MOT_MLU   CHI_MLU TYPES_MOT TYPES_CHI TOKENS_MOT
    ## 1                 38 3.957230 2.9092742       367       260       1731
    ## 2                 46 4.111413 3.3643411       452       273       3076
    ## 3                 50 4.013353 2.9095355       516       235       2576
    ## 4                 39 4.472993 3.0614525       555       237       2895
    ## 5                 45 4.250853 2.6794872       374       219       2227
    ## 6                 48 4.240798 3.0774194       429       217       2510
    ## 7                 41 4.847418 3.7011952       388       210       1959
    ## 8                 40 4.211321 3.0911950       478       221       2044
    ## 9                 37 4.664286 3.8114035       359       178       1802
    ## 10                44 3.532374 3.2781955       410       166       2171
    ## 11                46 3.391525 3.0727969       357       219       1764
    ## 12                50 4.080446 3.4415584       505       226       3072
    ## 13                46 4.445872 2.9482072       491       250       2264
    ## 14                36 4.061475 2.8526316       397       213       1741
    ## 15                48 4.588477 3.4135021       304       245        999
    ## 16                44 4.664013 2.8651685       595       210       2586
    ## 17                27 4.287582 2.7571429       260       168       1138
    ## 18                NA 4.113158 2.8480000       303       158       1460
    ## 19                48 5.247093 3.5950000       383       217       1701
    ## 20                47 4.468493 3.5049505       339       201       1498
    ## 21                30 3.320000 2.1553398       335       197       2221
    ## 22                34 4.239198 2.3815789       411       156       2438
    ## 23                32 3.370937 2.2745098       342       157       1540
    ## 24                48 5.379798 2.9027778       433       155       2389
    ## 25                37 3.926928 2.1586207       417       170       2508
    ## 26                31 4.388235 2.5614754       324       165       1978
    ## 27                37 4.587179 2.7665198       462       179       3182
    ## 28                35 4.254505 2.4800000       385       154       1788
    ## 29                32 4.239437 2.9607843       391       164       1889
    ## 30                41 4.186161 2.8921569       437       183       2347
    ## 31                45 5.229885 3.7103448       486       173       2564
    ## 32                44 5.587332 2.4292929       548       163       2887
    ## 33                45 3.603473 2.0717489       475       185       2363
    ## 34                40 4.347709 2.8690476       327       156       1395
    ## 35                40 5.153639 2.7612903       383       140       1866
    ## 36                36 3.706790 3.2439024       249       102       1024
    ## 37                21 3.370093 1.4734513       319        98       1565
    ## 38                26 2.483146 1.4967320       158        66        536
    ## 39                32 3.156695 2.1450382       256        55        995
    ## 40                18 3.534173 1.3052632       372        47       1748
    ## 41                31 3.514403 1.4012346       311        58       1481
    ## 42                21 4.349353 1.2883436       454        52       2391
    ## 43                39 4.366972 2.2258065       322        64       1352
    ## 44                22 4.341549 1.0843373       425       101       2219
    ## 45                33 4.235585 2.7051282       400        73       2271
    ## 46                12 3.860795 1.1680000       309        62       1349
    ## 47                10 3.460548 1.1610169       351        10       1918
    ## 48                29 4.100707 1.6447368       330        55       1987
    ## 49                10 4.003257 2.6315789       585        12       2202
    ## 50                18 3.965392 0.7536232       326        14       1852
    ## 51                 9 3.241422 0.0156250       444         2       2450
    ## 52                46 4.676101 2.7600000       322        34       1371
    ## 53                11 3.650235 1.0000000       281         3       1454
    ## 54                17 3.474725 1.0588235       284         6       1390
    ## 55                11 3.886842 1.3333333       370         4       1396
    ## 56                16 3.943636 0.5000000       388         2       2077
    ## 57                27       NA        NA        NA        NA         NA
    ## 58                NA       NA        NA        NA        NA         NA
    ## 59                43       NA        NA        NA        NA         NA
    ## 60                42       NA        NA        NA        NA         NA
    ##    TOKENS_CHI
    ## 1        1294
    ## 2        1249
    ## 3        1079
    ## 4        1010
    ## 5         921
    ## 6         897
    ## 7         864
    ## 8         847
    ## 9         793
    ## 10        738
    ## 11        719
    ## 12        713
    ## 13        710
    ## 14        702
    ## 15        698
    ## 16        686
    ## 17        666
    ## 18        659
    ## 19        646
    ## 20        640
    ## 21        618
    ## 22        611
    ## 23        605
    ## 24        590
    ## 25        571
    ## 26        568
    ## 27        538
    ## 28        530
    ## 29        521
    ## 30        517
    ## 31        460
    ## 32        436
    ## 33        418
    ## 34        410
    ## 35        395
    ## 36        358
    ## 37        306
    ## 38        300
    ## 39        274
    ## 40        236
    ## 41        210
    ## 42        204
    ## 43        197
    ## 44        195
    ## 45        189
    ## 46        143
    ## 47        137
    ## 48        110
    ## 49        100
    ## 50         79
    ## 51         64
    ## 52         61
    ## 53         37
    ## 54         36
    ## 55          8
    ## 56          2
    ## 57         NA
    ## 58         NA
    ## 59         NA
    ## 60         NA

``` r
# Task 2 = Subject 61 = 0 words in total
df3 %>% 
  filter(VISIT == 1) %>% 
  arrange(TOKENS_CHI)
```

    ##    SUBJ ADOS1 SOCIALIZATION1 MULLENRAW1 EXPRESSIVELANGRAW1 VISIT
    ## 1    62    15             66         28                 10     1
    ## 2    35    21             69         21                  9     1
    ## 3    39     3             98         19                 13     1
    ## 4    33    12             70         31                 13     1
    ## 5    27    20             67         13                 11     1
    ## 6     7    17             68         20                 17     1
    ## 7    36    14             76         25                 11     1
    ## 8    19    14             74         25                 11     1
    ## 9    56     1            102         22                 14     1
    ## 10   53    17             82         28                 10     1
    ## 11   55    19             64         17                 10     1
    ## 12   61    14             77         27                 11     1
    ## 13   43    13             86         27                 13     1
    ## 14   14     0            104         30                 16     1
    ## 15   50    20             75         21                  9     1
    ## 16   47     3             94         30                 20     1
    ## 17   11     3            104         27                 18     1
    ## 18   48     5            113         24                 20     1
    ## 19   30     0             96         26                 18     1
    ## 20   17     0             86         24                 15     1
    ## 21   13     0            106         21                 15     1
    ## 22   26    14             65         25                 19     1
    ## 23   16     0             92         23                 17     1
    ## 24   42    11             88         26                 19     1
    ## 25   59     1            115         24                 22     1
    ## 26   31     0            102         20                 16     1
    ## 27   32    17             72         26                 14     1
    ## 28   25     0            106         21                 19     1
    ## 29   23    21             72         22                  8     1
    ## 30    8    18             65         24                 14     1
    ## 31   10     0            100         24                 18     1
    ## 32    2     0            108         28                 14     1
    ## 33    9     5            102         32                 31     1
    ## 34   60     0             96         26                 17     1
    ## 35   66    15             79         27                 16     1
    ## 36   45     0             96         24                 19     1
    ## 37   37    10             82         27                 22     1
    ## 38    6     9             82         34                 27     1
    ## 39   52     0            102         27                 20     1
    ## 40   22     9             86         26                 14     1
    ## 41   21    11             88         28                 20     1
    ## 42   63    15             88         30                 24     1
    ## 43   44     0            102         25                 17     1
    ## 44   64     4            108         29                 22     1
    ## 45   15     0            102         25                 17     1
    ## 46   18     0            102         29                 26     1
    ## 47    4     1             88         29                 18     1
    ## 48   38     1            102         23                 21     1
    ## 49    5     8             82         31                 27     1
    ## 50   54     0            108         27                 27     1
    ## 51   34     0            100         27                 22     1
    ## 52   57     0             94         29                 22     1
    ## 53   40     7             70         33                 26     1
    ## 54   41     1            104         29                 28     1
    ## 55   20     0            106         29                 33     1
    ## 56   28     0             90         29                 22     1
    ## 57   46    14             65         42                 27     1
    ## 58    3    13             85         34                 27     1
    ## 59   65    13             87         30                 30     1
    ## 60   58     0             98         30                 30     1
    ## 61   29    11            100         32                 33     1
    ## 62    1    15            104         NA                 NA     1
    ## 63   12     0            105         28                 33     1
    ## 64   24    19             78         26                 16     1
    ## 65   49     0             92         27                 17     1
    ## 66   51     4             84         17                 16     1
    ##           ETHNICITY DIAGNOSIS GENDER   AGE ADOS SOCIALIZATION MULLENRAW
    ## 1             White       ASD      F 41.07   15            66        28
    ## 2             White       ASD      M 34.27   21            69        21
    ## 3             White        TD      M 19.20    3            98        19
    ## 4             White       ASD      M 36.53   12            70        31
    ## 5             White       ASD      M 37.47   20            67        13
    ## 6       Bangladeshi       ASD      F 26.17   17            68        20
    ## 7  African American       ASD      F 25.33   14            76        25
    ## 8             White       ASD      M 34.80   14            74        25
    ## 9             Asian        TD      F 20.87    1           102        22
    ## 10            White       ASD      M 31.63   17            82        28
    ## 11            White       ASD      M 37.47   19            64        17
    ## 12            White       ASD      M 35.50   14            77        27
    ## 13         Lebanese       ASD      M 24.90   13            86        27
    ## 14            White        TD      M 20.07    0           104        30
    ## 15            White       ASD      M 36.73   20            75        21
    ## 16            White        TD      M 19.77    3            94        30
    ## 17            White        TD      M 19.27    3           104        27
    ## 18            White        TD      M 20.03    5           113        24
    ## 19            White        TD      M 21.03    0            96        26
    ## 20            White        TD      M 19.27    0            86        24
    ## 21            White        TD      M 19.23    0           106        21
    ## 22     White/Latino       ASD      M 27.37   14            65        25
    ## 23            White        TD      M 18.97    0            92        23
    ## 24      White/Asian       ASD      M 33.20   11            88        26
    ## 25            White        TD      M 19.10    1           115        24
    ## 26            White        TD      M 19.93    0           102        20
    ## 27            White       ASD      M 34.87   17            72        26
    ## 28            White        TD      F 18.93    0           106        21
    ## 29 African American       ASD      M 27.53   21            72        22
    ## 30            White       ASD      F 41.00   18            65        24
    ## 31            White        TD      F 18.30    0           100        24
    ## 32            White        TD      M 19.80    0           108        28
    ## 33            White        TD      M 20.10    5           102        32
    ## 34            White        TD      M 19.97    0            96        26
    ## 35            White       ASD      M 42.00   15            79        27
    ## 36            White        TD      M 19.37    0            96        24
    ## 37            White       ASD      M 33.77   10            82        27
    ## 38            White       ASD      M 34.03    9            82        34
    ## 39            White        TD      F 20.03    0           102        27
    ## 40            White       ASD      M 18.77    9            86        26
    ## 41            White       ASD      M 35.80   11            88        28
    ## 42            White       ASD      M 26.00   15            88        30
    ## 43            White        TD      M 19.23    0           102        25
    ## 44            White        TD      M 20.80    4           108        29
    ## 45            White        TD      M 19.00    0           102        25
    ## 46            White        TD      M 21.67    0           102        29
    ## 47            White        TD      F 23.50    1            88        29
    ## 48            White        TD      M 19.30    1           102        23
    ## 49     White/Latino       ASD      M 31.03    8            82        31
    ## 50            White        TD      M 23.07    0           108        27
    ## 51            White        TD      M 23.13    0           100        27
    ## 52            White        TD      M 22.57    0            94        29
    ## 53            White       ASD      M 39.50    7            70        33
    ## 54            White        TD      F 19.87    1           104        29
    ## 55            White        TD      M 20.77    0           106        29
    ## 56            White        TD      M 21.77    0            90        29
    ## 57            White       ASD      M 37.03   14            65        42
    ## 58            White       ASD      M 28.80   13            85        34
    ## 59            White       ASD      M 34.00   13            87        30
    ## 60            White        TD      M 23.90    0            98        30
    ## 61            White       ASD      M 30.40   11           100        32
    ## 62            White        TD      M 18.07   15           104        NA
    ## 63            White       ASD      F 31.77    0           105        28
    ## 64           Latino       ASD      M 35.47   19            78        26
    ## 65            White        TD      M 23.13    0            92        27
    ## 66            White        TD      M    NA    4            84        17
    ##    EXPRESSIVELANGRAW  MOT_MLU   CHI_MLU TYPES_MOT TYPES_CHI TOKENS_MOT
    ## 1                 10 3.833770 0.0000000       386         0       2613
    ## 2                  9 3.686347 1.5000000       228         3        927
    ## 3                 13 3.921109 1.2307692       281         8       1631
    ## 4                 13 3.607088 0.9000000       366        11       2054
    ## 5                 11 2.917355 1.0833333       193         6        654
    ## 6                 17 2.618729 1.0000000       212         4        761
    ## 7                 11 2.287293 1.2500000       206        13        788
    ## 8                 11 3.182390 1.0277778       281         9       1418
    ## 9                 14 3.435743 1.1818182       331         7       1503
    ## 10                10 3.765528 1.2500000       303        17       2147
    ## 11                10 3.704110 1.1000000       265         8       1215
    ## 12                11 2.548969 1.0444444       195         9        955
    ## 13                13 2.997050 1.0175439       252         9       1827
    ## 14                16 4.195335 1.0877193       235        17       1262
    ## 15                 9 4.883966 1.1666667       387        10       2144
    ## 16                20 3.088757 1.2592593       275         9       1417
    ## 17                18 4.204846 1.0375000       289        15       1808
    ## 18                20 2.776181 0.5584416       258        20       1188
    ## 19                18 3.616867 1.3661972       317        37       1361
    ## 20                15 3.967078 1.1647059       277        27       1643
    ## 21                15 3.380463 1.2168675       215        24       1136
    ## 22                19 3.024548 1.4324324       328        41       2138
    ## 23                17 3.420315 1.0396040       287         7       1625
    ## 24                19 3.298748 1.2043011       274        36       1537
    ## 25                22 3.487871 1.3139535       214        32       1118
    ## 26                16 2.788793 1.2758621       178        34        584
    ## 27                14 3.304189 1.0086207       295         6       1643
    ## 28                19 3.630476 1.2371134       343        36       1698
    ## 29                 8 4.390879 1.0000000       485         8       2826
    ## 30                14 2.244755 1.2641509       152        29        578
    ## 31                18 3.544419 1.0378788       363        36       1408
    ## 32                14 3.621993 1.2522523       378        14       1835
    ## 33                31 3.265082 1.5600000       288        59       1564
    ## 34                17 3.509138 1.1846154       178        11       1144
    ## 35                16 3.030405 1.0375000       303        15       1579
    ## 36                19 4.033333 1.3034483       373        47       2334
    ## 37                22 2.743455 1.3766234       214        67        893
    ## 38                27 3.986357 1.3947368       324        57       2859
    ## 39                20 3.943005 1.0761421       260        13       1347
    ## 40                14 2.524740 0.1857143       321        16       1787
    ## 41                20 2.539823 1.3595506       283        89       1019
    ## 42                24 2.747100 1.1809045       338        98       2084
    ## 43                17 3.093146 1.0262009       333        32       1547
    ## 44                22 3.432387 1.1830065       345        62       1805
    ## 45                17 3.960254 1.5740741       329        69       2139
    ## 46                26 4.910190 1.5988372       375        90       2585
    ## 47                18 3.757269 1.8776978       334        51       2674
    ## 48                21 3.561364 1.2641509       291        24       1344
    ## 49                27 3.459370 2.0972222       379       102       2009
    ## 50                27 4.030075 1.4258373       255        73       1452
    ## 51                22 3.494327 1.5333333       322        91       1870
    ## 52                22 5.344227 1.4086957       441        92       2267
    ## 53                26 4.135036 0.4805825       381        39       1988
    ## 54                28 3.420975 1.3322785       342        96       2035
    ## 55                33 3.587855 1.7948718       467       108       2555
    ## 56                22 4.643200 1.5827338       373        71       2740
    ## 57                27 3.341418 1.9144981       271       101       1591
    ## 58                27 4.098446 2.2768595       317       146       1428
    ## 59                30 3.604140 2.8763441       400       149       2587
    ## 60                30 4.159159 1.9397163       236       120       1170
    ## 61                33 4.690751 3.4000000       278       119       1450
    ## 62                NA       NA        NA        NA        NA         NA
    ## 63                33       NA        NA        NA        NA         NA
    ## 64                16       NA        NA        NA        NA         NA
    ## 65                17       NA        NA        NA        NA         NA
    ## 66                16       NA        NA        NA        NA         NA
    ##    TOKENS_CHI
    ## 1           0
    ## 2           3
    ## 3          16
    ## 4          21
    ## 5          26
    ## 6          29
    ## 7          35
    ## 8          37
    ## 9          38
    ## 10         40
    ## 11         43
    ## 12         47
    ## 13         58
    ## 14         62
    ## 15         63
    ## 16         68
    ## 17         83
    ## 18         91
    ## 19         95
    ## 20         99
    ## 21        101
    ## 22        103
    ## 23        105
    ## 24        109
    ## 25        109
    ## 26        111
    ## 27        117
    ## 28        118
    ## 29        122
    ## 30        130
    ## 31        137
    ## 32        139
    ## 33        143
    ## 34        154
    ## 35        166
    ## 36        176
    ## 37        180
    ## 38        197
    ## 39        212
    ## 40        214
    ## 41        227
    ## 42        233
    ## 43        235
    ## 44        244
    ## 45        249
    ## 46        254
    ## 47        260
    ## 48        260
    ## 49        269
    ## 50        286
    ## 51        319
    ## 52        319
    ## 53        337
    ## 54        398
    ## 55        406
    ## 56        433
    ## 57        450
    ## 58        461
    ## 59        469
    ## 60        473
    ## 61        483
    ## 62         NA
    ## 63         NA
    ## 64         NA
    ## 65         NA
    ## 66         NA

USING SELECT

1.  Make a subset of the data including only kids with ASD, mlu and word
    tokens
2.  What happens if you include the name of a variable multiple times in
    a select() call?

``` r
# Task 1
df3 %>% 
  filter(DIAGNOSIS == "ASD") %>% 
  select(CHI_MLU, TOKENS_CHI)
```

    ##       CHI_MLU TOKENS_CHI
    ## 1   2.2768595        461
    ## 2   3.4530387        562
    ## 3   3.1193182        983
    ## 4   4.3023256        674
    ## 5          NA         NA
    ## 6   3.4135021        698
    ## 7   2.0972222        269
    ## 8   2.5630252        555
    ## 9   3.5180723        490
    ## 10  3.2571429        479
    ## 11  4.0434783        539
    ## 12  3.2781955        738
    ## 13  1.3947368        197
    ## 14  2.5743590        487
    ## 15  2.5324074        468
    ## 16  2.9072581        604
    ## 17  2.4232804        404
    ## 18  2.7665198        538
    ## 19  1.0000000         29
    ## 20  0.7606838        149
    ## 21  1.1698113         62
    ## 22  1.7142857        205
    ## 23  1.6363636        349
    ## 24  1.4967320        300
    ## 25  1.2641509        130
    ## 26  1.3211679        169
    ## 27  1.3974359        210
    ## 28  1.9115646        494
    ## 29  1.5299145        194
    ## 30         NA         NA
    ## 31         NA         NA
    ## 32         NA         NA
    ## 33  1.0277778         37
    ## 34  1.0125000         82
    ## 35  1.0000000         58
    ## 36         NA         NA
    ## 37  1.0000000          9
    ## 38  1.0000000         37
    ## 39  1.3595506        227
    ## 40  1.4867257        335
    ## 41  1.9052632        330
    ## 42  1.6179402        455
    ## 43  1.5840000        197
    ## 44  2.1450382        274
    ## 45  0.1857143        214
    ## 46  1.0800000        362
    ## 47  1.6109325        503
    ## 48  2.2485380        717
    ## 49  2.2506739        792
    ## 50  2.9027778        590
    ## 51  1.0000000        122
    ## 52  1.0400000         78
    ## 53  2.0000000         96
    ## 54  0.2727273         93
    ## 55  0.2000000          5
    ## 56  0.5000000          2
    ## 57         NA         NA
    ## 58         NA         NA
    ## 59         NA         NA
    ## 60  1.4324324        103
    ## 61  1.4273504        157
    ## 62  1.0863309        148
    ## 63  1.0974026        162
    ## 64  1.4133333        197
    ## 65  1.3052632        236
    ## 66  1.0833333         26
    ## 67  1.0000000         32
    ## 68  1.0000000         40
    ## 69  1.2032520        143
    ## 70  1.0000000         34
    ## 71  1.1680000        143
    ## 72  3.4000000        483
    ## 73         NA         NA
    ## 74  3.9196891       1293
    ## 75  3.5238095        714
    ## 76  3.2919897       1154
    ## 77  3.3643411       1249
    ## 78  1.0086207        117
    ## 79  1.0114286        177
    ## 80  0.6842105         39
    ## 81  0.8947368         39
    ## 82  0.2553191        179
    ## 83  1.2883436        204
    ## 84  0.9000000         21
    ## 85  1.0265487        116
    ## 86  1.0066667        151
    ## 87  1.1250000         79
    ## 88  0.4130435        193
    ## 89  1.0843373        195
    ## 90  1.5000000          3
    ## 91  1.0000000          7
    ## 92  1.0000000          1
    ## 93  1.0000000         14
    ## 94  1.0000000         33
    ## 95  2.6315789        100
    ## 96  1.2500000         35
    ## 97  1.0000000         10
    ## 98  1.0833333         26
    ## 99  1.2100840        142
    ## 100 0.6756757         42
    ## 101 0.7536232         79
    ## 102 1.3766234        180
    ## 103 1.9481132        368
    ## 104 1.8056872        365
    ## 105 2.1962617        438
    ## 106 2.5758929        537
    ## 107 2.2745098        605
    ## 108 0.4805825        337
    ## 109 1.3432836        433
    ## 110 1.8812665        694
    ## 111 2.5159817        510
    ## 112 2.7468750        815
    ## 113 3.0774194        897
    ## 114 1.2043011        109
    ## 115 1.2571429        126
    ## 116 1.8880000        232
    ## 117 1.7000000        135
    ## 118 1.6511628        340
    ## 119 1.6447368        110
    ## 120 1.0175439         58
    ## 121 1.2195122        148
    ## 122 2.0704225        404
    ## 123 2.9900000        493
    ## 124        NA         NA
    ## 125 2.1586207        571
    ## 126 1.9144981        450
    ## 127 1.9832215        536
    ## 128 2.3053892       1023
    ## 129 2.6837838        822
    ## 130 2.8665049       1054
    ## 131 2.6794872        921
    ## 132 1.1666667         63
    ## 133 1.2258065         38
    ## 134 1.0000000          3
    ## 135 1.1200000         55
    ## 136 1.0000000         32
    ## 137 1.1610169        137
    ## 138 1.2500000         40
    ## 139 1.0000000          4
    ## 140 1.0000000         45
    ## 141 1.4102564        255
    ## 142 1.4734513        306
    ## 143 1.1000000         43
    ## 144 1.3628319        147
    ## 145 1.1666667         77
    ## 146 1.0000000         70
    ## 147 0.0000000          0
    ## 148 1.3333333          8
    ## 149 1.0444444         47
    ## 150 1.0000000         19
    ## 151 1.7500000         14
    ## 152 1.0588235         36
    ## 153 0.0000000          0
    ## 154 1.0000000         44
    ## 155 0.1621622         41
    ## 156 0.0000000          1
    ## 157 0.2000000         30
    ## 158 0.0156250         64
    ## 159 1.1809045        233
    ## 160 1.4432990        260
    ## 161 1.8000000        393
    ## 162 2.0127796        574
    ## 163 1.7591463        531
    ## 164 2.1553398        618
    ## 165 2.8763441        469
    ## 166 2.7840000        670
    ## 167 4.1318681        698
    ## 168 3.3598326        693
    ## 169 2.9655172        357
    ## 170 3.4415584        713
    ## 171 1.0375000        166
    ## 172 1.0990991        356
    ## 173 1.5828571        254
    ## 174 1.3204225        334
    ## 175 1.6688963        511
    ## 176 1.4012346        210

``` r
# Task 2 - Nothing happens, if you include the same variable multiple times. It will only be there once.
df3 %>% 
  select(CHI_MLU, TOKENS_CHI, CHI_MLU, CHI_MLU)
```

    ##       CHI_MLU TOKENS_CHI
    ## 1          NA         NA
    ## 2   1.2522523        139
    ## 3   1.0136054        148
    ## 4   1.5568862        255
    ## 5   2.2515723        321
    ## 6   3.2380952        472
    ## 7   2.8651685        686
    ## 8   2.2768595        461
    ## 9   3.4530387        562
    ## 10  3.1193182        983
    ## 11  4.3023256        674
    ## 12         NA         NA
    ## 13  3.4135021        698
    ## 14  1.8776978        260
    ## 15  2.6418605        530
    ## 16  2.6916300        542
    ## 17  3.9292035        754
    ## 18  3.2985782        588
    ## 19  3.7103448        460
    ## 20  2.0972222        269
    ## 21  2.5630252        555
    ## 22  3.5180723        490
    ## 23  3.2571429        479
    ## 24  4.0434783        539
    ## 25  3.2781955        738
    ## 26  1.3947368        197
    ## 27  2.5743590        487
    ## 28  2.5324074        468
    ## 29  2.9072581        604
    ## 30  2.4232804        404
    ## 31  2.7665198        538
    ## 32  1.0000000         29
    ## 33  0.7606838        149
    ## 34  1.1698113         62
    ## 35  1.7142857        205
    ## 36  1.6363636        349
    ## 37  1.4967320        300
    ## 38  1.2641509        130
    ## 39  1.3211679        169
    ## 40  1.3974359        210
    ## 41  1.9115646        494
    ## 42  1.5299145        194
    ## 43         NA         NA
    ## 44  1.5600000        143
    ## 45  1.6019108        554
    ## 46         NA         NA
    ## 47  3.0265957        493
    ## 48  1.9408602        323
    ## 49  2.2258065        197
    ## 50  1.0378788        137
    ## 51  1.6162791        132
    ## 52  1.8320312        453
    ## 53  2.7756410        390
    ## 54  2.8358209        346
    ## 55         NA         NA
    ## 56  1.0375000         83
    ## 57  1.1200000         55
    ## 58  1.5520000        188
    ## 59  1.6479401        414
    ## 60  2.2835821        431
    ## 61  2.7051282        189
    ## 62         NA         NA
    ## 63         NA         NA
    ## 64  1.2168675        101
    ## 65  1.0630631        117
    ## 66  1.5234375        193
    ## 67  2.1348315        354
    ## 68  2.5482456        513
    ## 69  2.7571429        666
    ## 70  1.0877193         62
    ## 71  1.3647799        203
    ## 72  3.1857708        733
    ## 73  3.0000000        243
    ## 74  4.3647541        916
    ## 75  3.5049505        640
    ## 76  1.5740741        249
    ## 77  1.4761905        393
    ## 78  2.0798817        630
    ## 79  2.7982833        538
    ## 80  3.2301136        932
    ## 81  3.7011952        864
    ## 82  1.0396040        105
    ## 83  1.0744681        297
    ## 84  1.1855072        368
    ## 85  2.3178295        557
    ## 86  3.3750000        755
    ## 87  3.8114035        793
    ## 88  1.1647059         99
    ## 89  1.0460526        159
    ## 90  1.4408602        252
    ## 91  2.5613208        495
    ## 92  2.4691358        160
    ## 93  2.8690476        410
    ## 94  1.5988372        254
    ## 95  2.7441077        733
    ## 96  2.8076923        825
    ## 97  2.3225806        793
    ## 98  3.1095238        583
    ## 99  2.9482072        710
    ## 100 1.0277778         37
    ## 101 1.0125000         82
    ## 102 1.0000000         58
    ## 103        NA         NA
    ## 104 1.0000000          9
    ## 105 1.0000000         37
    ## 106 1.7948718        406
    ## 107 2.7220339        738
    ## 108 3.3400000        940
    ## 109 3.2128205       1092
    ## 110 3.0902778        769
    ## 111 2.9095355       1079
    ## 112 1.3595506        227
    ## 113 1.4867257        335
    ## 114 1.9052632        330
    ## 115 1.6179402        455
    ## 116 1.5840000        197
    ## 117 2.1450382        274
    ## 118 0.1857143        214
    ## 119 1.0800000        362
    ## 120 1.6109325        503
    ## 121 2.2485380        717
    ## 122 2.2506739        792
    ## 123 2.9027778        590
    ## 124 1.0000000        122
    ## 125 1.0400000         78
    ## 126 2.0000000         96
    ## 127 0.2727273         93
    ## 128 0.2000000          5
    ## 129 0.5000000          2
    ## 130        NA         NA
    ## 131        NA         NA
    ## 132        NA         NA
    ## 133 1.2371134        118
    ## 134 1.7804878        414
    ## 135 1.4549550        305
    ## 136 2.3740741        592
    ## 137 3.7000000        686
    ## 138 3.0911950        847
    ## 139 1.4324324        103
    ## 140 1.4273504        157
    ## 141 1.0863309        148
    ## 142 1.0974026        162
    ## 143 1.4133333        197
    ## 144 1.3052632        236
    ## 145 1.0833333         26
    ## 146 1.0000000         32
    ## 147 1.0000000         40
    ## 148 1.2032520        143
    ## 149 1.0000000         34
    ## 150 1.1680000        143
    ## 151 1.5827338        433
    ## 152 2.4847328        623
    ## 153 2.8042169        826
    ## 154 3.7310924       1225
    ## 155 2.7413793       1145
    ## 156 3.0614525       1010
    ## 157 3.4000000        483
    ## 158        NA         NA
    ## 159 3.9196891       1293
    ## 160 3.5238095        714
    ## 161 3.2919897       1154
    ## 162 3.3643411       1249
    ## 163 1.3661972         95
    ## 164 1.4228571        236
    ## 165 1.4976077        299
    ## 166 1.7405858        384
    ## 167 2.1791908        318
    ## 168 2.4800000        530
    ## 169 1.2758621        111
    ## 170 1.6557377        258
    ## 171 1.8369099        403
    ## 172 2.3500000        502
    ## 173 3.5852535        622
    ## 174 2.9607843        521
    ## 175 1.0086207        117
    ## 176 1.0114286        177
    ## 177 0.6842105         39
    ## 178 0.8947368         39
    ## 179 0.2553191        179
    ## 180 1.2883436        204
    ## 181 0.9000000         21
    ## 182 1.0265487        116
    ## 183 1.0066667        151
    ## 184 1.1250000         79
    ## 185 0.4130435        193
    ## 186 1.0843373        195
    ## 187 1.5333333        319
    ## 188 2.2481481        542
    ## 189 2.9875260       1246
    ## 190 2.7272727        750
    ## 191 2.1420613        690
    ## 192 2.0717489        418
    ## 193 1.5000000          3
    ## 194 1.0000000          7
    ## 195 1.0000000          1
    ## 196 1.0000000         14
    ## 197 1.0000000         33
    ## 198 2.6315789        100
    ## 199 1.2500000         35
    ## 200 1.0000000         10
    ## 201 1.0833333         26
    ## 202 1.2100840        142
    ## 203 0.6756757         42
    ## 204 0.7536232         79
    ## 205 1.3766234        180
    ## 206 1.9481132        368
    ## 207 1.8056872        365
    ## 208 2.1962617        438
    ## 209 2.5758929        537
    ## 210 2.2745098        605
    ## 211 1.2641509        260
    ## 212 1.9342916        879
    ## 213 2.5710145        702
    ## 214 1.9743590         74
    ## 215 2.5541796        736
    ## 216 2.3815789        611
    ## 217 1.2307692         16
    ## 218 1.3103448        107
    ## 219 1.7500000        218
    ## 220 2.2594142        477
    ## 221 2.2787611        459
    ## 222 2.5614754        568
    ## 223 0.4805825        337
    ## 224 1.3432836        433
    ## 225 1.8812665        694
    ## 226 2.5159817        510
    ## 227 2.7468750        815
    ## 228 3.0774194        897
    ## 229 1.3322785        398
    ## 230 2.1727273        439
    ## 231 2.8690476        825
    ## 232 2.6475410        576
    ## 233 3.5185185        800
    ## 234        NA         NA
    ## 235 1.2043011        109
    ## 236 1.2571429        126
    ## 237 1.8880000        232
    ## 238 1.7000000        135
    ## 239 1.6511628        340
    ## 240 1.6447368        110
    ## 241 1.0175439         58
    ## 242 1.2195122        148
    ## 243 2.0704225        404
    ## 244 2.9900000        493
    ## 245        NA         NA
    ## 246 2.1586207        571
    ## 247 1.0262009        235
    ## 248 1.1974249        264
    ## 249 1.9086294        352
    ## 250 2.2325581        434
    ## 251 2.2892308        651
    ## 252 2.8526316        702
    ## 253 1.3034483        176
    ## 254 2.0046729        423
    ## 255 2.4911032        604
    ## 256 3.8300395        820
    ## 257 3.7749077        875
    ## 258 3.0727969        719
    ## 259 1.9144981        450
    ## 260 1.9832215        536
    ## 261 2.3053892       1023
    ## 262 2.6837838        822
    ## 263 2.8665049       1054
    ## 264 2.6794872        921
    ## 265 1.2592593         68
    ## 266 1.3043478        120
    ## 267 2.2313433        528
    ## 268 2.5370370        378
    ## 269 3.2000000        433
    ## 270        NA         NA
    ## 271 0.5584416         91
    ## 272 0.9051724        165
    ## 273 1.1722689        354
    ## 274 1.7600000        433
    ## 275 2.2558140        502
    ## 276        NA         NA
    ## 277        NA         NA
    ## 278 1.1666667         63
    ## 279 1.2258065         38
    ## 280 1.0000000          3
    ## 281 1.1200000         55
    ## 282 1.0000000         32
    ## 283 1.1610169        137
    ## 284        NA         NA
    ## 285        NA         NA
    ## 286 1.0761421        212
    ## 287 2.2481481        571
    ## 288 2.2893617        486
    ## 289 3.1622419        973
    ## 290 2.7388060        304
    ## 291 2.7600000         61
    ## 292 1.2500000         40
    ## 293 1.0000000          4
    ## 294 1.0000000         45
    ## 295 1.4102564        255
    ## 296 1.4734513        306
    ## 297 1.4258373        286
    ## 298 2.4967742        356
    ## 299 3.1124498        671
    ## 300 3.4809524        684
    ## 301 3.5371429        586
    ## 302 3.5950000        646
    ## 303 1.1000000         43
    ## 304 1.3628319        147
    ## 305 1.1666667         77
    ## 306 1.0000000         70
    ## 307 0.0000000          0
    ## 308 1.3333333          8
    ## 309 1.1818182         38
    ## 310 1.1976744         97
    ## 311 1.5529412        122
    ## 312 3.0289855        348
    ## 313 2.9215686        238
    ## 314 2.7612903        395
    ## 315 1.4086957        319
    ## 316 1.7359551        298
    ## 317 3.3037543        897
    ## 318 3.0775510        642
    ## 319 2.8321678        723
    ## 320 2.4292929        436
    ## 321 1.9397163        473
    ## 322 3.2179487        449
    ## 323 3.1313559        660
    ## 324 3.6340694        978
    ## 325 3.8225806        814
    ## 326 3.2439024        358
    ## 327 1.3139535        109
    ## 328 2.0466667        287
    ## 329 2.3309353        261
    ## 330 3.4065041        694
    ## 331 3.6072874        725
    ## 332 2.8921569        517
    ## 333 1.1846154        154
    ## 334 1.7902098        244
    ## 335 2.4932432        672
    ## 336 2.8620690       1051
    ## 337        NA         NA
    ## 338 2.9092742       1294
    ## 339 1.0444444         47
    ## 340 1.0000000         19
    ## 341 1.7500000         14
    ## 342 1.0588235         36
    ## 343 0.0000000          0
    ## 344 1.0000000         44
    ## 345 0.1621622         41
    ## 346 0.0000000          1
    ## 347 0.2000000         30
    ## 348 0.0156250         64
    ## 349 1.1809045        233
    ## 350 1.4432990        260
    ## 351 1.8000000        393
    ## 352 2.0127796        574
    ## 353 1.7591463        531
    ## 354 2.1553398        618
    ## 355 1.1830065        244
    ## 356 1.7709251        401
    ## 357 2.1157895        424
    ## 358 2.7148289        637
    ## 359 2.6038462        636
    ## 360 2.8480000        659
    ## 361 2.8763441        469
    ## 362 2.7840000        670
    ## 363 4.1318681        698
    ## 364 3.3598326        693
    ## 365 2.9655172        357
    ## 366 3.4415584        713
    ## 367 1.0375000        166
    ## 368 1.0990991        356
    ## 369 1.5828571        254
    ## 370 1.3204225        334
    ## 371 1.6688963        511
    ## 372 1.4012346        210

USING MUTATE, SUMMARISE and PIPES 1. Add a column to the data set that
represents the mean number of words spoken during all visits. 2. Use the
summarise function and pipes to add an column in the data set containing
the mean amount of words produced by each trial across all visits. HINT:
group by Child.ID 3. The solution to task above enables us to assess the
average amount of words produced by each child. Why don’t we just use
these average values to describe the language production of the
children? What is the advantage of keeping all the data?

``` r
# Task 1
df3 %>% 
  mutate(mTOKENCHI = mean(TOKENS_CHI, na.rm = T))
```

    ##     SUBJ ADOS1 SOCIALIZATION1 MULLENRAW1 EXPRESSIVELANGRAW1 VISIT
    ## 1      1    15            104         NA                 NA     1
    ## 2      2     0            108         28                 14     1
    ## 3      2     0            108         28                 14     2
    ## 4      2     0            108         28                 14     3
    ## 5      2     0            108         28                 14     4
    ## 6      2     0            108         28                 14     5
    ## 7      2     0            108         28                 14     6
    ## 8      3    13             85         34                 27     1
    ## 9      3    13             85         34                 27     2
    ## 10     3    13             85         34                 27     3
    ## 11     3    13             85         34                 27     4
    ## 12     3    13             85         34                 27     5
    ## 13     3    13             85         34                 27     6
    ## 14     4     1             88         29                 18     1
    ## 15     4     1             88         29                 18     2
    ## 16     4     1             88         29                 18     3
    ## 17     4     1             88         29                 18     4
    ## 18     4     1             88         29                 18     5
    ## 19     4     1             88         29                 18     6
    ## 20     5     8             82         31                 27     1
    ## 21     5     8             82         31                 27     2
    ## 22     5     8             82         31                 27     3
    ## 23     5     8             82         31                 27     4
    ## 24     5     8             82         31                 27     5
    ## 25     5     8             82         31                 27     6
    ## 26     6     9             82         34                 27     1
    ## 27     6     9             82         34                 27     2
    ## 28     6     9             82         34                 27     3
    ## 29     6     9             82         34                 27     4
    ## 30     6     9             82         34                 27     5
    ## 31     6     9             82         34                 27     6
    ## 32     7    17             68         20                 17     1
    ## 33     7    17             68         20                 17     2
    ## 34     7    17             68         20                 17     3
    ## 35     7    17             68         20                 17     4
    ## 36     7    17             68         20                 17     5
    ## 37     7    17             68         20                 17     6
    ## 38     8    18             65         24                 14     1
    ## 39     8    18             65         24                 14     2
    ## 40     8    18             65         24                 14     3
    ## 41     8    18             65         24                 14     4
    ## 42     8    18             65         24                 14     5
    ## 43     8    18             65         24                 14     6
    ## 44     9     5            102         32                 31     1
    ## 45     9     5            102         32                 31     2
    ## 46     9     5            102         32                 31     3
    ## 47     9     5            102         32                 31     4
    ## 48     9     5            102         32                 31     5
    ## 49     9     5            102         32                 31     6
    ## 50    10     0            100         24                 18     1
    ## 51    10     0            100         24                 18     2
    ## 52    10     0            100         24                 18     3
    ## 53    10     0            100         24                 18     4
    ## 54    10     0            100         24                 18     5
    ## 55    10     0            100         24                 18     6
    ## 56    11     3            104         27                 18     1
    ## 57    11     3            104         27                 18     2
    ## 58    11     3            104         27                 18     3
    ## 59    11     3            104         27                 18     4
    ## 60    11     3            104         27                 18     5
    ## 61    11     3            104         27                 18     6
    ## 62    12     0            105         28                 33     1
    ## 63    12     0            105         28                 33     2
    ## 64    13     0            106         21                 15     1
    ## 65    13     0            106         21                 15     2
    ## 66    13     0            106         21                 15     3
    ## 67    13     0            106         21                 15     4
    ## 68    13     0            106         21                 15     5
    ## 69    13     0            106         21                 15     6
    ## 70    14     0            104         30                 16     1
    ## 71    14     0            104         30                 16     2
    ## 72    14     0            104         30                 16     3
    ## 73    14     0            104         30                 16     4
    ## 74    14     0            104         30                 16     5
    ## 75    14     0            104         30                 16     6
    ## 76    15     0            102         25                 17     1
    ## 77    15     0            102         25                 17     2
    ## 78    15     0            102         25                 17     3
    ## 79    15     0            102         25                 17     4
    ## 80    15     0            102         25                 17     5
    ## 81    15     0            102         25                 17     6
    ## 82    16     0             92         23                 17     1
    ## 83    16     0             92         23                 17     2
    ## 84    16     0             92         23                 17     3
    ## 85    16     0             92         23                 17     4
    ## 86    16     0             92         23                 17     5
    ## 87    16     0             92         23                 17     6
    ## 88    17     0             86         24                 15     1
    ## 89    17     0             86         24                 15     2
    ## 90    17     0             86         24                 15     3
    ## 91    17     0             86         24                 15     4
    ## 92    17     0             86         24                 15     5
    ## 93    17     0             86         24                 15     6
    ## 94    18     0            102         29                 26     1
    ## 95    18     0            102         29                 26     2
    ## 96    18     0            102         29                 26     3
    ## 97    18     0            102         29                 26     4
    ## 98    18     0            102         29                 26     5
    ## 99    18     0            102         29                 26     6
    ## 100   19    14             74         25                 11     1
    ## 101   19    14             74         25                 11     2
    ## 102   19    14             74         25                 11     3
    ## 103   19    14             74         25                 11     4
    ## 104   19    14             74         25                 11     5
    ## 105   19    14             74         25                 11     6
    ## 106   20     0            106         29                 33     1
    ## 107   20     0            106         29                 33     2
    ## 108   20     0            106         29                 33     3
    ## 109   20     0            106         29                 33     4
    ## 110   20     0            106         29                 33     5
    ## 111   20     0            106         29                 33     6
    ## 112   21    11             88         28                 20     1
    ## 113   21    11             88         28                 20     2
    ## 114   21    11             88         28                 20     3
    ## 115   21    11             88         28                 20     4
    ## 116   21    11             88         28                 20     5
    ## 117   21    11             88         28                 20     6
    ## 118   22     9             86         26                 14     1
    ## 119   22     9             86         26                 14     2
    ## 120   22     9             86         26                 14     3
    ## 121   22     9             86         26                 14     4
    ## 122   22     9             86         26                 14     5
    ## 123   22     9             86         26                 14     6
    ## 124   23    21             72         22                  8     1
    ## 125   23    21             72         22                  8     2
    ## 126   23    21             72         22                  8     3
    ## 127   23    21             72         22                  8     4
    ## 128   23    21             72         22                  8     5
    ## 129   23    21             72         22                  8     6
    ## 130   24    19             78         26                 16     1
    ## 131   24    19             78         26                 16     2
    ## 132   24    19             78         26                 16     3
    ## 133   25     0            106         21                 19     1
    ## 134   25     0            106         21                 19     2
    ## 135   25     0            106         21                 19     3
    ## 136   25     0            106         21                 19     4
    ## 137   25     0            106         21                 19     5
    ## 138   25     0            106         21                 19     6
    ## 139   26    14             65         25                 19     1
    ## 140   26    14             65         25                 19     2
    ## 141   26    14             65         25                 19     3
    ## 142   26    14             65         25                 19     4
    ## 143   26    14             65         25                 19     5
    ## 144   26    14             65         25                 19     6
    ## 145   27    20             67         13                 11     1
    ## 146   27    20             67         13                 11     2
    ## 147   27    20             67         13                 11     3
    ## 148   27    20             67         13                 11     4
    ## 149   27    20             67         13                 11     5
    ## 150   27    20             67         13                 11     6
    ## 151   28     0             90         29                 22     1
    ## 152   28     0             90         29                 22     2
    ## 153   28     0             90         29                 22     3
    ## 154   28     0             90         29                 22     4
    ## 155   28     0             90         29                 22     5
    ## 156   28     0             90         29                 22     6
    ## 157   29    11            100         32                 33     1
    ## 158   29    11            100         32                 33     2
    ## 159   29    11            100         32                 33     3
    ## 160   29    11            100         32                 33     4
    ## 161   29    11            100         32                 33     5
    ## 162   29    11            100         32                 33     6
    ## 163   30     0             96         26                 18     1
    ## 164   30     0             96         26                 18     2
    ## 165   30     0             96         26                 18     3
    ## 166   30     0             96         26                 18     4
    ## 167   30     0             96         26                 18     5
    ## 168   30     0             96         26                 18     6
    ## 169   31     0            102         20                 16     1
    ## 170   31     0            102         20                 16     2
    ## 171   31     0            102         20                 16     3
    ## 172   31     0            102         20                 16     4
    ## 173   31     0            102         20                 16     5
    ## 174   31     0            102         20                 16     6
    ## 175   32    17             72         26                 14     1
    ## 176   32    17             72         26                 14     2
    ## 177   32    17             72         26                 14     3
    ## 178   32    17             72         26                 14     4
    ## 179   32    17             72         26                 14     5
    ## 180   32    17             72         26                 14     6
    ## 181   33    12             70         31                 13     1
    ## 182   33    12             70         31                 13     2
    ## 183   33    12             70         31                 13     3
    ## 184   33    12             70         31                 13     4
    ## 185   33    12             70         31                 13     5
    ## 186   33    12             70         31                 13     6
    ## 187   34     0            100         27                 22     1
    ## 188   34     0            100         27                 22     2
    ## 189   34     0            100         27                 22     3
    ## 190   34     0            100         27                 22     4
    ## 191   34     0            100         27                 22     5
    ## 192   34     0            100         27                 22     6
    ## 193   35    21             69         21                  9     1
    ## 194   35    21             69         21                  9     2
    ## 195   35    21             69         21                  9     3
    ## 196   35    21             69         21                  9     4
    ## 197   35    21             69         21                  9     5
    ## 198   35    21             69         21                  9     6
    ## 199   36    14             76         25                 11     1
    ## 200   36    14             76         25                 11     2
    ## 201   36    14             76         25                 11     3
    ## 202   36    14             76         25                 11     4
    ## 203   36    14             76         25                 11     5
    ## 204   36    14             76         25                 11     6
    ## 205   37    10             82         27                 22     1
    ## 206   37    10             82         27                 22     2
    ## 207   37    10             82         27                 22     3
    ## 208   37    10             82         27                 22     4
    ## 209   37    10             82         27                 22     5
    ## 210   37    10             82         27                 22     6
    ## 211   38     1            102         23                 21     1
    ## 212   38     1            102         23                 21     2
    ## 213   38     1            102         23                 21     3
    ## 214   38     1            102         23                 21     4
    ## 215   38     1            102         23                 21     5
    ## 216   38     1            102         23                 21     6
    ## 217   39     3             98         19                 13     1
    ## 218   39     3             98         19                 13     2
    ## 219   39     3             98         19                 13     3
    ## 220   39     3             98         19                 13     4
    ## 221   39     3             98         19                 13     5
    ## 222   39     3             98         19                 13     6
    ## 223   40     7             70         33                 26     1
    ## 224   40     7             70         33                 26     2
    ## 225   40     7             70         33                 26     3
    ## 226   40     7             70         33                 26     4
    ## 227   40     7             70         33                 26     5
    ## 228   40     7             70         33                 26     6
    ## 229   41     1            104         29                 28     1
    ## 230   41     1            104         29                 28     2
    ## 231   41     1            104         29                 28     3
    ## 232   41     1            104         29                 28     4
    ## 233   41     1            104         29                 28     5
    ## 234   41     1            104         29                 28     6
    ## 235   42    11             88         26                 19     1
    ## 236   42    11             88         26                 19     2
    ## 237   42    11             88         26                 19     3
    ## 238   42    11             88         26                 19     4
    ## 239   42    11             88         26                 19     5
    ## 240   42    11             88         26                 19     6
    ## 241   43    13             86         27                 13     1
    ## 242   43    13             86         27                 13     2
    ## 243   43    13             86         27                 13     3
    ## 244   43    13             86         27                 13     4
    ## 245   43    13             86         27                 13     5
    ## 246   43    13             86         27                 13     6
    ## 247   44     0            102         25                 17     1
    ## 248   44     0            102         25                 17     2
    ## 249   44     0            102         25                 17     3
    ## 250   44     0            102         25                 17     4
    ## 251   44     0            102         25                 17     5
    ## 252   44     0            102         25                 17     6
    ## 253   45     0             96         24                 19     1
    ## 254   45     0             96         24                 19     2
    ## 255   45     0             96         24                 19     3
    ## 256   45     0             96         24                 19     4
    ## 257   45     0             96         24                 19     5
    ## 258   45     0             96         24                 19     6
    ## 259   46    14             65         42                 27     1
    ## 260   46    14             65         42                 27     2
    ## 261   46    14             65         42                 27     3
    ## 262   46    14             65         42                 27     4
    ## 263   46    14             65         42                 27     5
    ## 264   46    14             65         42                 27     6
    ## 265   47     3             94         30                 20     1
    ## 266   47     3             94         30                 20     2
    ## 267   47     3             94         30                 20     3
    ## 268   47     3             94         30                 20     4
    ## 269   47     3             94         30                 20     5
    ## 270   47     3             94         30                 20     6
    ## 271   48     5            113         24                 20     1
    ## 272   48     5            113         24                 20     2
    ## 273   48     5            113         24                 20     3
    ## 274   48     5            113         24                 20     4
    ## 275   48     5            113         24                 20     5
    ## 276   49     0             92         27                 17     1
    ## 277   49     0             92         27                 17     2
    ## 278   50    20             75         21                  9     1
    ## 279   50    20             75         21                  9     2
    ## 280   50    20             75         21                  9     3
    ## 281   50    20             75         21                  9     4
    ## 282   50    20             75         21                  9     5
    ## 283   50    20             75         21                  9     6
    ## 284   51     4             84         17                 16     1
    ## 285   51     4             84         17                 16     2
    ## 286   52     0            102         27                 20     1
    ## 287   52     0            102         27                 20     2
    ## 288   52     0            102         27                 20     3
    ## 289   52     0            102         27                 20     4
    ## 290   52     0            102         27                 20     5
    ## 291   52     0            102         27                 20     6
    ## 292   53    17             82         28                 10     1
    ## 293   53    17             82         28                 10     2
    ## 294   53    17             82         28                 10     4
    ## 295   53    17             82         28                 10     5
    ## 296   53    17             82         28                 10     6
    ## 297   54     0            108         27                 27     1
    ## 298   54     0            108         27                 27     2
    ## 299   54     0            108         27                 27     3
    ## 300   54     0            108         27                 27     4
    ## 301   54     0            108         27                 27     5
    ## 302   54     0            108         27                 27     6
    ## 303   55    19             64         17                 10     1
    ## 304   55    19             64         17                 10     2
    ## 305   55    19             64         17                 10     3
    ## 306   55    19             64         17                 10     4
    ## 307   55    19             64         17                 10     5
    ## 308   55    19             64         17                 10     6
    ## 309   56     1            102         22                 14     1
    ## 310   56     1            102         22                 14     2
    ## 311   56     1            102         22                 14     3
    ## 312   56     1            102         22                 14     4
    ## 313   56     1            102         22                 14     5
    ## 314   56     1            102         22                 14     6
    ## 315   57     0             94         29                 22     1
    ## 316   57     0             94         29                 22     2
    ## 317   57     0             94         29                 22     3
    ## 318   57     0             94         29                 22     4
    ## 319   57     0             94         29                 22     5
    ## 320   57     0             94         29                 22     6
    ## 321   58     0             98         30                 30     1
    ## 322   58     0             98         30                 30     2
    ## 323   58     0             98         30                 30     3
    ## 324   58     0             98         30                 30     4
    ## 325   58     0             98         30                 30     5
    ## 326   58     0             98         30                 30     6
    ## 327   59     1            115         24                 22     1
    ## 328   59     1            115         24                 22     2
    ## 329   59     1            115         24                 22     3
    ## 330   59     1            115         24                 22     4
    ## 331   59     1            115         24                 22     5
    ## 332   59     1            115         24                 22     6
    ## 333   60     0             96         26                 17     1
    ## 334   60     0             96         26                 17     2
    ## 335   60     0             96         26                 17     3
    ## 336   60     0             96         26                 17     4
    ## 337   60     0             96         26                 17     5
    ## 338   60     0             96         26                 17     6
    ## 339   61    14             77         27                 11     1
    ## 340   61    14             77         27                 11     2
    ## 341   61    14             77         27                 11     3
    ## 342   61    14             77         27                 11     6
    ## 343   62    15             66         28                 10     1
    ## 344   62    15             66         28                 10     2
    ## 345   62    15             66         28                 10     3
    ## 346   62    15             66         28                 10     4
    ## 347   62    15             66         28                 10     5
    ## 348   62    15             66         28                 10     6
    ## 349   63    15             88         30                 24     1
    ## 350   63    15             88         30                 24     2
    ## 351   63    15             88         30                 24     3
    ## 352   63    15             88         30                 24     4
    ## 353   63    15             88         30                 24     5
    ## 354   63    15             88         30                 24     6
    ## 355   64     4            108         29                 22     1
    ## 356   64     4            108         29                 22     2
    ## 357   64     4            108         29                 22     3
    ## 358   64     4            108         29                 22     4
    ## 359   64     4            108         29                 22     5
    ## 360   64     4            108         29                 22     6
    ## 361   65    13             87         30                 30     1
    ## 362   65    13             87         30                 30     2
    ## 363   65    13             87         30                 30     3
    ## 364   65    13             87         30                 30     4
    ## 365   65    13             87         30                 30     5
    ## 366   65    13             87         30                 30     6
    ## 367   66    15             79         27                 16     1
    ## 368   66    15             79         27                 16     2
    ## 369   66    15             79         27                 16     3
    ## 370   66    15             79         27                 16     4
    ## 371   66    15             79         27                 16     5
    ## 372   66    15             79         27                 16     6
    ##            ETHNICITY DIAGNOSIS GENDER   AGE ADOS SOCIALIZATION MULLENRAW
    ## 1              White        TD      M 18.07   15           104        NA
    ## 2              White        TD      M 19.80    0           108        28
    ## 3              White        TD      M 23.93   NA           110        NA
    ## 4              White        TD      M 27.70   NA           109        NA
    ## 5              White        TD      M 32.90   NA           102        33
    ## 6              White        TD      M 35.90    0           107        NA
    ## 7              White        TD      M 40.13   NA           100        42
    ## 8              White       ASD      M 28.80   13            85        34
    ## 9              White       ASD      M 33.17   NA           105        NA
    ## 10             White       ASD      M 37.07   NA            77        NA
    ## 11             White       ASD      M 41.07   NA            75        49
    ## 12             White       ASD      M 45.23   10            79        NA
    ## 13             White       ASD      M 49.70   NA            81        48
    ## 14             White        TD      F 23.50    1            88        29
    ## 15             White        TD      F 27.83   NA            89        NA
    ## 16             White        TD      F 31.83   NA            91        NA
    ## 17             White        TD      F 35.53   NA            93        39
    ## 18             White        TD      F 39.47    0            86        NA
    ## 19             White        TD      F 45.07   NA            86        45
    ## 20      White/Latino       ASD      M 31.03    8            82        31
    ## 21      White/Latino       ASD      M 35.30   NA           115        NA
    ## 22      White/Latino       ASD      M 38.90   NA            81        NA
    ## 23      White/Latino       ASD      M 43.13   NA            77        41
    ## 24      White/Latino       ASD      M 47.40    9            79        NA
    ## 25      White/Latino       ASD      M 51.37   NA            74        44
    ## 26             White       ASD      M 34.03    9            82        34
    ## 27             White       ASD      M 37.57   NA            94        NA
    ## 28             White       ASD      M 41.57   NA            77        NA
    ## 29             White       ASD      M 45.53   NA            77        38
    ## 30             White       ASD      M 49.30    9            75        NA
    ## 31             White       ASD      M 54.13   NA            75        40
    ## 32       Bangladeshi       ASD      F 26.17   17            68        20
    ## 33       Bangladeshi       ASD      F 30.27   NA            74        NA
    ## 34       Bangledeshi       ASD      F 34.57   NA            70        NA
    ## 35       Bangladeshi       ASD      F 38.47   NA            66        33
    ## 36       Bangladeshi       ASD      F 42.43    8            68        NA
    ## 37       Bangladeshi       ASD      F 46.53   NA            74        39
    ## 38             White       ASD      F 41.00   18            65        24
    ## 39             White       ASD      F 49.20   NA            63        NA
    ## 40             White       ASD      F 49.20   NA            63        NA
    ## 41             White       ASD      F 52.27   NA            61        29
    ## 42             White       ASD      F 57.73   17            63        NA
    ## 43             White       ASD      F 60.33   NA            59        32
    ## 44             White        TD      M 20.10    5           102        32
    ## 45             White        TD      M 24.07   NA           102        NA
    ## 46             White        TD      M 28.17   NA           109        NA
    ## 47             White        TD      M 32.07   NA           109        42
    ## 48             White        TD      M 36.07    5           100        NA
    ## 49             White        TD      M 40.17   NA           108        43
    ## 50             White        TD      F 18.30    0           100        24
    ## 51             White        TD      F 23.73   NA           106        NA
    ## 52             White        TD      F 26.63   NA           105        NA
    ## 53             White        TD      F 31.07   NA           103        45
    ## 54             White        TD      F 35.00    1           107        NA
    ## 55             White        TD      F    NA   NA            NA        NA
    ## 56             White        TD      M 19.27    3           104        27
    ## 57             White        TD      M 23.83   NA           106        NA
    ## 58             White        TD      M 28.53   NA           111        NA
    ## 59             White        TD      M 32.43   NA           103        31
    ## 60             White        TD      M 36.53    5           103        NA
    ## 61             White        TD      M 40.43   NA           103        33
    ## 62             White       ASD      F 31.77    0           105        28
    ## 63    White American       ASD      F 37.13   NA           110        NA
    ## 64             White        TD      M 19.23    0           106        21
    ## 65             White        TD      M    NA   NA           104        NA
    ## 66             White        TD      M 27.27   NA           102        NA
    ## 67             White        TD      M 31.20   NA           100        35
    ## 68             White        TD      M 35.63    0           100        NA
    ## 69             White        TD      M 40.27   NA           101        42
    ## 70             White        TD      M 20.07    0           104        30
    ## 71             White        TD      M 23.83   NA           115        NA
    ## 72             White        TD      M 28.27   NA           105        NA
    ## 73             White        TD      M 32.07   NA           103        42
    ## 74             White        TD      M 35.87    0           101        NA
    ## 75             White        TD      M 41.50   NA            97        44
    ## 76             White        TD      M 19.00    0           102        25
    ## 77             White        TD      M 22.97   NA           100        NA
    ## 78             White        TD      M 27.07   NA           109        NA
    ## 79             White        TD      M 31.03   NA           107        40
    ## 80             White        TD      M 35.37    0           100        NA
    ## 81             White        TD      M 39.40   NA            92        47
    ## 82             White        TD      M 18.97    0            92        23
    ## 83             White        TD      M 22.87   NA           102        NA
    ## 84             White        TD      M 26.80   NA            91        NA
    ## 85             White        TD      M 31.47   NA            96        29
    ## 86             White        TD      M 35.10    0            87        NA
    ## 87             White        TD      M 39.43   NA            83        44
    ## 88             White        TD      M 19.27    0            86        24
    ## 89             White        TD      M 24.80   NA            86        NA
    ## 90             White        TD      M 28.10   NA            86        NA
    ## 91             White        TD      M 31.27   NA            87        33
    ## 92             White        TD      M 35.90    3            86        NA
    ## 93             White        TD      M 40.30   NA            94        40
    ## 94             White        TD      M 21.67    0           102        29
    ## 95             White        TD      M 26.27   NA           102        NA
    ## 96             White        TD      M 30.63   NA            95        NA
    ## 97             White        TD      M 34.27   NA           100        45
    ## 98             White        TD      M 38.17    0           103        NA
    ## 99             White        TD      M 42.93   NA            97        49
    ## 100            White       ASD      M 34.80   14            74        25
    ## 101            White       ASD      M 38.73   NA           104        NA
    ## 102            White       ASD      M    NA   NA            68        NA
    ## 103            White       ASD      M 46.67   NA            68        27
    ## 104            White       ASD      M 50.93   15            66        NA
    ## 105            White       ASD      M 54.43   NA            65        27
    ## 106            White        TD      M 20.77    0           106        29
    ## 107            White        TD      M 26.13   NA           107        NA
    ## 108            White        TD      M 30.03   NA           112        NA
    ## 109            White        TD      M 34.43   NA           105        50
    ## 110            White        TD      M 38.70    0           118        NA
    ## 111            White        TD      M 44.07   NA           116        50
    ## 112            White       ASD      M 35.80   11            88        28
    ## 113            White       ASD      M 39.93   NA           102        NA
    ## 114            White       ASD      M 43.90   NA            79        NA
    ## 115            White       ASD      M 47.83   NA            74        35
    ## 116            White       ASD      M 51.97    9            72        NA
    ## 117            White       ASD      M 56.73   NA            72        30
    ## 118            White       ASD      M 18.77    9            86        26
    ## 119            White       ASD      M 22.50   NA            84        NA
    ## 120            White       ASD      M 26.77   NA            95        NA
    ## 121            White       ASD      M 30.80   NA            98        43
    ## 122            White       ASD      M 34.13    8           107        NA
    ## 123            White       ASD      M 37.30   NA           103        43
    ## 124 African American       ASD      M 27.53   21            72        22
    ## 125 African American       ASD      M 31.80   NA            74        NA
    ## 126 African American       ASD      M 36.17   NA            77        NA
    ## 127 African American       ASD      M 40.27   NA            68        27
    ## 128 African American       ASD      M 44.70   19            72        NA
    ## 129 African American       ASD      M 48.97   NA            65        26
    ## 130           Latino       ASD      M 35.47   19            78        26
    ## 131         Hispanic       ASD      M 39.87   NA            70        NA
    ## 132  Latino/Hispanic       ASD      M 43.57   NA            81        NA
    ## 133            White        TD      F 18.93    0           106        21
    ## 134            White        TD      F 22.90   NA           102        NA
    ## 135            White        TD      F 26.67   NA           114        NA
    ## 136            White        TD      F 30.93   NA           109        40
    ## 137            White        TD      F 35.13    0           114        NA
    ## 138            White        TD      F 39.23   NA           110        43
    ## 139     White/Latino       ASD      M 27.37   14            65        25
    ## 140     White/Latino       ASD      M 32.10   NA            65        NA
    ## 141     White/Latino       ASD      M 35.93   NA            70        NA
    ## 142     White/Latino       ASD      M 42.23   NA            63        29
    ## 143     White/Latino       ASD      M 44.93   14            65        NA
    ## 144     White/Latino       ASD      M 47.50   NA            65        30
    ## 145            White       ASD      M 37.47   20            67        13
    ## 146            White       ASD      M 42.20   NA            78        NA
    ## 147            White       ASD      M 46.37   NA            74        NA
    ## 148            White       ASD      M 52.13   NA            72        29
    ## 149            White       ASD      M 57.20   17            72        NA
    ## 150            White       ASD      M 62.40   NA            68        30
    ## 151            White        TD      M 21.77    0            90        29
    ## 152            White        TD      M 25.47   NA           108        NA
    ## 153            White        TD      M 30.13   NA           100        NA
    ## 154            White        TD      M 34.00   NA           102        47
    ## 155            White        TD      M 37.93    0           101        NA
    ## 156            White        TD      M 42.47   NA           103        48
    ## 157            White       ASD      M 30.40   11           100        32
    ## 158            White       ASD      M 34.43   NA            70        NA
    ## 159            White       ASD      M 38.60   NA            83        NA
    ## 160            White       ASD      M 42.63   NA            86        41
    ## 161            White       ASD      M 46.93    4            97        NA
    ## 162            White       ASD      M 51.00   NA            90        46
    ## 163            White        TD      M 21.03    0            96        26
    ## 164            White        TD      M 25.23   NA            98        NA
    ## 165            White        TD      M 29.07   NA            98        NA
    ## 166            White        TD      M 33.30   NA           100        42
    ## 167            White        TD      M 37.73    0           112        NA
    ## 168            White        TD      M 41.17   NA           106        43
    ## 169            White        TD      M 19.93    0           102        20
    ## 170            White        TD      M 23.57   NA            76        NA
    ## 171            White        TD      M 27.03   NA           100        NA
    ## 172            White        TD      M 31.00   NA            96        28
    ## 173            White        TD      M 36.43    0           100        NA
    ## 174            White        TD      M 39.43   NA            95        39
    ## 175            White       ASD      M 34.87   17            72        26
    ## 176            White       ASD      M 38.13   NA            68        NA
    ## 177            White       ASD      M 43.10   NA            72        NA
    ## 178            White       ASD      M 46.63   NA            72        27
    ## 179            White       ASD      M 50.97   20            68        NA
    ## 180            White       ASD      M 55.17   NA            72        39
    ## 181            White       ASD      M 36.53   12            70        31
    ## 182            White       ASD      M 40.50   NA            79        NA
    ## 183            White       ASD      M 44.67   NA            79        NA
    ## 184            White       ASD      M 48.10   NA            74        47
    ## 185            White       ASD      M 52.90   15            77        NA
    ## 186            White       ASD      M 56.43   NA            85        49
    ## 187            White        TD      M 23.13    0           100        27
    ## 188            White        TD      M 27.70   NA           103        NA
    ## 189            White        TD      M 32.07   NA           107        NA
    ## 190            White        TD      M 35.03   NA           107        43
    ## 191            White        TD      M 39.53    0           106        NA
    ## 192            White        TD      M 43.80   NA           108        45
    ## 193            White       ASD      M 34.27   21            69        21
    ## 194            White       ASD      M 38.33   NA            80        NA
    ## 195            White       ASD      M 42.13   NA            68        NA
    ## 196            White       ASD      M 47.10   NA            72        27
    ## 197            White       ASD      M 50.73   20            66        NA
    ## 198            White       ASD      M 54.63   NA            66        28
    ## 199 African American       ASD      F 25.33   14            76        25
    ## 200 African American       ASD      F 29.70   NA            74        NA
    ## 201 African American       ASD      F 34.10   NA            74        NA
    ## 202 African American       ASD      F 36.93   NA            86        36
    ## 203 African American       ASD      F 42.97   25            85        NA
    ## 204 African American       ASD      F 46.17   NA            83        33
    ## 205            White       ASD      M 33.77   10            82        27
    ## 206            White       ASD      M 38.13   NA            74        NA
    ## 207            White       ASD      M 43.00   NA            92        NA
    ## 208            White       ASD      M 46.13   NA            92        31
    ## 209            White       ASD      M 50.30   16            90        NA
    ## 210            White       ASD      M 53.77   NA            92        44
    ## 211            White        TD      M 19.30    1           102        23
    ## 212            White        TD      M 23.43   NA           105        NA
    ## 213            White        TD      M 27.13   NA           112        NA
    ## 214            White        TD      M 30.80   NA           105        36
    ## 215            White        TD      M 34.93    2           103        NA
    ## 216            White        TD      M 39.07   NA            97        45
    ## 217            White        TD      M 19.20    3            98        19
    ## 218            White        TD      M 22.87   NA           102        NA
    ## 219            White        TD      M 26.87   NA           103        NA
    ## 220            White        TD      M 30.40   NA           100        35
    ## 221            White        TD      M 34.40    2            91        NA
    ## 222            White        TD      M 38.53   NA            97        46
    ## 223            White       ASD      M 39.50    7            70        33
    ## 224            White       ASD      M 43.68   NA            83        NA
    ## 225            White       ASD      M 47.73   NA           100        NA
    ## 226            White       ASD      M 51.67   NA           101        49
    ## 227            White       ASD      M 55.17    4           105        NA
    ## 228            White       ASD      M 58.77   NA           116        50
    ## 229            White        TD      F 19.87    1           104        29
    ## 230            White        TD      F 24.17   NA           116        NA
    ## 231            White        TD      F 28.07   NA           125        NA
    ## 232            White        TD      F 32.17   NA           125        44
    ## 233            White        TD      F 36.13    0           120        NA
    ## 234            White        TD      F 40.23   NA           114        49
    ## 235      White/Asian       ASD      M 33.20   11            88        26
    ## 236      White/Asian       ASD      M 37.37   NA            72        NA
    ## 237      White/Asian       ASD      M 41.73   NA            63        NA
    ## 238      White/Asian       ASD      M 45.33   NA            66        45
    ## 239      White/Asian       ASD      M 48.63   11            63        NA
    ## 240      White/Asian       ASD      M 53.63   NA            63        30
    ## 241         Lebanese       ASD      M 24.90   13            86        27
    ## 242         Lebanese       ASD      M 32.20   NA            38        NA
    ## 243         Lebanese       ASD      M 34.00   NA            87        NA
    ## 244         Lebanese       ASD      M    NA   NA            86        NA
    ## 245         Lebanese       ASD      M 42.27   12            83        NA
    ## 246         Lebanese       ASD      M 46.40   NA            94        41
    ## 247            White        TD      M 19.23    0           102        25
    ## 248            White        TD      M 23.07   NA            92        NA
    ## 249            White        TD      M 28.47   NA           103        NA
    ## 250            White        TD      M 32.07   NA           100        41
    ## 251            White        TD      M 35.67    0           105        NA
    ## 252            White        TD      M 39.93   NA           101        45
    ## 253            White        TD      M 19.37    0            96        24
    ## 254            White        TD      M    NA   NA           105        NA
    ## 255            White        TD      M 28.90   NA           109        NA
    ## 256            White        TD      M 32.07   NA           109        39
    ## 257            White        TD      M 36.40    0           100        NA
    ## 258            White        TD      M 40.13   NA           101        46
    ## 259            White       ASD      M 37.03   14            65        42
    ## 260            White       ASD      M 42.77   NA            72        NA
    ## 261            White       ASD      M 46.23   NA            70        NA
    ## 262            White       ASD      M 49.30   NA            75        50
    ## 263            White       ASD      M 53.40   11            77        NA
    ## 264            White       ASD      M 57.37   NA            77        49
    ## 265            White        TD      M 19.77    3            94        30
    ## 266            White        TD      M 23.63   NA           106        NA
    ## 267            White        TD      M 27.67   NA           100        NA
    ## 268            White        TD      M 32.07   NA            95        40
    ## 269            White        TD      M 35.83    1           107        NA
    ## 270            White        TD      M 39.93   NA           118        40
    ## 271            White        TD      M 20.03    5           113        24
    ## 272            White        TD      M 24.68   NA           118        NA
    ## 273            White        TD      M 28.63   NA           112        NA
    ## 274            White        TD      M 32.77   NA           114        26
    ## 275            White        TD      M 36.70   10           122        NA
    ## 276            White        TD      M 23.13    0            92        27
    ## 277            White        TD      M    NA   NA            93        NA
    ## 278            White       ASD      M 36.73   20            75        21
    ## 279            White       ASD      M 41.50   NA            74        NA
    ## 280            White       ASD      M 45.57   NA            61        NA
    ## 281            White       ASD      M 50.23   NA            55        29
    ## 282            White       ASD      M 54.33   17            63        NA
    ## 283            White       ASD      M 57.43   NA            59        32
    ## 284            White        TD      M    NA    4            84        17
    ## 285            White        TD      M    NA   NA            91        NA
    ## 286            White        TD      F 20.03    0           102        27
    ## 287            White        TD      F 24.40   NA           100        NA
    ## 288            White        TD      F 29.53   NA            98        NA
    ## 289            White        TD      F 32.13   NA            93        42
    ## 290            White        TD      F 39.10   NA            95        NA
    ## 291            White        TD      F 40.37   NA           101        43
    ## 292            White       ASD      M 31.63   17            82        28
    ## 293            White       ASD      M 37.20   NA            66        NA
    ## 294            White       ASD      M 43.63   NA            68        33
    ## 295            White       ASD      M 52.77   17            68        NA
    ## 296            White       ASD      M    NA   NA            63        32
    ## 297            White        TD      M 23.07    0           108        27
    ## 298            White        TD      M 27.47   NA            70        NA
    ## 299            White        TD      M 31.63   NA           105        NA
    ## 300            White        TD      M 35.63   NA           105        45
    ## 301            White        TD      M 39.47    0           101        NA
    ## 302            White        TD      M 43.40   NA           101        45
    ## 303            White       ASD      M 37.47   19            64        17
    ## 304            White       ASD      M 42.20   NA            77        NA
    ## 305            White       ASD      M 46.37   NA            72        NA
    ## 306            White       ASD      M 52.13   NA            68        22
    ## 307            White       ASD      M 57.20   17            68        NA
    ## 308            White       ASD      M 62.40   NA            66        24
    ## 309            Asian        TD      F 20.87    1           102        22
    ## 310            Asian        TD      F 25.40   NA           109        NA
    ## 311            Asian        TD      F 29.67   NA           105        NA
    ## 312            Asian        TD      F 34.43   NA           120        30
    ## 313            Asian        TD      F 37.67    3           112        NA
    ## 314            Asian        TD      F 42.10   NA           108        46
    ## 315            White        TD      M 22.57    0            94        29
    ## 316            White        TD      M 26.63   NA            81        NA
    ## 317            White        TD      M 30.77   NA           103        NA
    ## 318            White        TD      M 35.03   NA           100        45
    ## 319            White        TD      M 38.60    0            94        NA
    ## 320            White        TD      M 43.03   NA            97        47
    ## 321            White        TD      M 23.90    0            98        30
    ## 322            White        TD      M 28.60   NA            86        NA
    ## 323            White        TD      M 32.50   NA           107        NA
    ## 324            White        TD      M 36.40   NA           108        45
    ## 325            White        TD      M 40.07    0           103        NA
    ## 326            White        TD      M 44.43   NA           101        45
    ## 327            White        TD      M 19.10    1           115        24
    ## 328            White        TD      M 23.17   NA            59        NA
    ## 329            White        TD      M 27.07   NA           107        NA
    ## 330            White        TD      M 30.83   NA           105        39
    ## 331            White        TD      M 35.17    0           105        NA
    ## 332            White        TD      M 39.30   NA           101        41
    ## 333            White        TD      M 19.97    0            96        26
    ## 334            White        TD      M 23.93   NA            96        NA
    ## 335            White        TD      M 28.03   NA            91        NA
    ## 336            White        TD      M 32.03   NA            93        40
    ## 337            White        TD      M 36.83    0           100        NA
    ## 338            White        TD      M 41.93   NA            95        45
    ## 339            White       ASD      M 35.50   14            77        27
    ## 340            White       ASD      M 39.40   NA            65        NA
    ## 341            White       ASD      M 44.47   NA            70        NA
    ## 342            White       ASD      M    NA   NA            NA        34
    ## 343            White       ASD      F 41.07   15            66        28
    ## 344            White       ASD      F 41.07   NA            66        NA
    ## 345            White       ASD      F 49.97   NA            65        NA
    ## 346            White       ASD      F 53.90   NA            68        30
    ## 347            White       ASD      F 58.27   13            66        NA
    ## 348            White       ASD      F 61.70   NA            68        32
    ## 349            White       ASD      M 26.00   15            88        30
    ## 350            White       ASD      M 30.10   NA            72        NA
    ## 351            White       ASD      M 34.27   NA            86        NA
    ## 352            White       ASD      M 38.20   NA            83        40
    ## 353            White       ASD      M 42.30   16            83        NA
    ## 354            White       ASD      M 46.07   NA            79        45
    ## 355            White        TD      M 20.80    4           108        29
    ## 356            White        TD      M 25.87   NA           118        NA
    ## 357            White        TD      M 29.67   NA           116        NA
    ## 358            White        TD      M 33.60   NA           116        34
    ## 359            White        TD      M 37.34    5           116        NA
    ## 360            White        TD      M 41.00   NA            NA        NA
    ## 361            White       ASD      M 34.00   13            87        30
    ## 362            White       ASD      M 38.63   NA            46        NA
    ## 363            White       ASD      M 42.47   NA           100        NA
    ## 364            White       ASD      M 47.00   NA           100        47
    ## 365            White       ASD      M 51.13   15            92        NA
    ## 366            White       ASD      M 54.73   NA            97        50
    ## 367            White       ASD      M 42.00   15            79        27
    ## 368            White       ASD      M 46.77   NA            79        NA
    ## 369            White       ASD      M 50.63   NA            94        NA
    ## 370            White       ASD      M 54.13   NA            92        28
    ## 371            White       ASD      M 58.57   15            95        NA
    ## 372            White       ASD      M 62.33   NA           103        41
    ##     EXPRESSIVELANGRAW  MOT_MLU   CHI_MLU TYPES_MOT TYPES_CHI TOKENS_MOT
    ## 1                  NA       NA        NA        NA        NA         NA
    ## 2                  14 3.621993 1.2522523       378        14       1835
    ## 3                  NA 3.857367 1.0136054       403        18       2160
    ## 4                  NA 4.321881 1.5568862       455        97       2149
    ## 5                  NA 4.415330 2.2515723       533       133       2260
    ## 6                  NA 5.209615 3.2380952       601       182       2553
    ## 7                  44 4.664013 2.8651685       595       210       2586
    ## 8                  27 4.098446 2.2768595       317       146       1428
    ## 9                  NA 4.964664 3.4530387       307       171       1270
    ## 10                 NA 4.147059 3.1193182       351       262       1445
    ## 11                 NA 5.309804 4.3023256       335       200       1286
    ## 12                 NA       NA        NA        NA        NA         NA
    ## 13                 48 4.588477 3.4135021       304       245        999
    ## 14                 18 3.757269 1.8776978       334        51       2674
    ## 15                 NA 4.572086 2.6418605       464       149       2694
    ## 16                 NA 5.134892 2.6916300       482       164       2630
    ## 17                 NA 5.301053 3.9292035       449       206       2397
    ## 18                 NA 4.566038 3.2985782       534       207       2672
    ## 19                 45 5.229885 3.7103448       486       173       2564
    ## 20                 27 3.459370 2.0972222       379       102       2009
    ## 21                 NA 4.130337 2.5630252       343       170       1657
    ## 22                 NA 3.818681 3.5180723       388       165       1788
    ## 23                 NA 4.301624 3.2571429       356       163       1711
    ## 24                 NA 4.602851 4.0434783       397       146       2082
    ## 25                 44 3.532374 3.2781955       410       166       2171
    ## 26                 27 3.986357 1.3947368       324        57       2859
    ## 27                 NA 4.886646 2.5743590       441       152       2955
    ## 28                 NA 4.198973 2.5324074       412       159       2939
    ## 29                 NA 4.744000 2.9072581       384       187       2685
    ## 30                 NA 4.292135 2.4232804       418       144       2749
    ## 31                 37 4.587179 2.7665198       462       179       3182
    ## 32                 17 2.618729 1.0000000       212         4        761
    ## 33                 NA 1.995885 0.7606838       140        29        464
    ## 34                 NA 1.856115 1.1698113        74        12        250
    ## 35                 NA 2.035714 1.7142857       125        50        360
    ## 36                 NA 1.948454 1.6363636        92        80        209
    ## 37                 26 2.483146 1.4967320       158        66        536
    ## 38                 14 2.244755 1.2641509       152        29        578
    ## 39                 NA 2.417344 1.3211679       186        42        755
    ## 40                 NA 2.633929 1.3974359       196        50        768
    ## 41                 NA 2.736842 1.9115646       198       126        814
    ## 42                 NA 3.992095 1.5299145       234        73        891
    ## 43                 27       NA        NA        NA        NA         NA
    ## 44                 31 3.265082 1.5600000       288        59       1564
    ## 45                 NA 3.967213 1.6019108       305       122       2099
    ## 46                 NA       NA        NA        NA        NA         NA
    ## 47                 NA 4.658333 3.0265957       375       134       2069
    ## 48                 NA 4.564103 1.9408602       387        40       2345
    ## 49                 39 4.366972 2.2258065       322        64       1352
    ## 50                 18 3.544419 1.0378788       363        36       1408
    ## 51                 NA 4.079060 1.6162791       369        45       1702
    ## 52                 NA 4.592593 1.8320312       369       129       1991
    ## 53                 NA 4.262195 2.7756410       400       121       1934
    ## 54                 NA 4.384946 2.8358209       428       121       1879
    ## 55                 NA       NA        NA        NA        NA         NA
    ## 56                 18 4.204846 1.0375000       289        15       1808
    ## 57                 NA 4.378840 1.1200000       358        18       2263
    ## 58                 NA 4.298942 1.5520000       409        55       2936
    ## 59                 NA 3.790896 1.6479401       411        96       2345
    ## 60                 NA 3.867601 2.2835821       359        97       2208
    ## 61                 33 4.235585 2.7051282       400        73       2271
    ## 62                 33       NA        NA        NA        NA         NA
    ## 63                 NA       NA        NA        NA        NA         NA
    ## 64                 15 3.380463 1.2168675       215        24       1136
    ## 65                 NA 2.821918 1.0630631       185        50        703
    ## 66                 NA 3.940711 1.5234375       227        56        878
    ## 67                 NA 4.732733 2.1348315       272        88       1387
    ## 68                 NA 4.543210 2.5482456       276       148       1300
    ## 69                 27 4.287582 2.7571429       260       168       1138
    ## 70                 16 4.195335 1.0877193       235        17       1262
    ## 71                 NA 3.444664 1.3647799       297        72       1551
    ## 72                 NA 4.974684 3.1857708       318       169       1463
    ## 73                 NA 3.988304 3.0000000       197       122        632
    ## 74                 NA 4.910494 4.3647541       290       222       1467
    ## 75                 47 4.468493 3.5049505       339       201       1498
    ## 76                 17 3.960254 1.5740741       329        69       2139
    ## 77                 NA 3.817109 1.4761905       411       119       2260
    ## 78                 NA 3.871968 2.0798817       435       139       2571
    ## 79                 NA 4.083700 2.7982833       463       203       2361
    ## 80                 NA 4.487842 3.2301136       511       247       2668
    ## 81                 41 4.847418 3.7011952       388       210       1959
    ## 82                 17 3.420315 1.0396040       287         7       1625
    ## 83                 NA 3.239832 1.0744681       346        65       1982
    ## 84                 NA 3.848175 1.1855072       378        92       2328
    ## 85                 NA 3.801292 2.3178295       369       158       1984
    ## 86                 NA 4.446281 3.3750000       390       238       1864
    ## 87                 37 4.664286 3.8114035       359       178       1802
    ## 88                 15 3.967078 1.1647059       277        27       1643
    ## 89                 NA 4.224839 1.0460526       363        25       1819
    ## 90                 NA 4.182979 1.4408602       409        92       1743
    ## 91                 NA 4.584071 2.5613208       357       161       1517
    ## 92                 NA 4.165414 2.4691358       356        69       1426
    ## 93                 40 4.347709 2.8690476       327       156       1395
    ## 94                 26 4.910190 1.5988372       375        90       2585
    ## 95                 NA 4.750455 2.7441077       391       196       2303
    ## 96                 NA 4.164789 2.8076923       408       200       2675
    ## 97                 NA 4.251605 2.3225806       508       231       3077
    ## 98                 NA 5.433579 3.1095238       517       219       2762
    ## 99                 46 4.445872 2.9482072       491       250       2264
    ## 100                11 3.182390 1.0277778       281         9       1418
    ## 101                NA 2.947230 1.0125000       243         7        999
    ## 102                NA 2.919169 1.0000000       252         2       1145
    ## 103                NA       NA        NA        NA        NA         NA
    ## 104                NA 3.996071 1.0000000       475         6       2088
    ## 105                11 3.650235 1.0000000       281         3       1454
    ## 106                33 3.587855 1.7948718       467       108       2555
    ## 107                NA 4.357911 2.7220339       461       160       2687
    ## 108                NA 4.116057 3.3400000       487       201       2479
    ## 109                NA 4.131579 3.2128205       565       207       2965
    ## 110                NA 3.877102 3.0902778       575       219       2881
    ## 111                50 4.013353 2.9095355       516       235       2576
    ## 112                20 2.539823 1.3595506       283        89       1019
    ## 113                NA 3.170000 1.4867257       315        86       1316
    ## 114                NA 3.615176 1.9052632       298        88       1159
    ## 115                NA 2.762463 1.6179402       248        87        855
    ## 116                NA 4.157191 1.5840000       328        45       1142
    ## 117                32 3.156695 2.1450382       256        55        995
    ## 118                14 2.524740 0.1857143       321        16       1787
    ## 119                NA 2.841004 1.0800000       288        73       1822
    ## 120                NA 4.063518 1.6109325       361       132       2092
    ## 121                NA 4.560201 2.2485380       422       153       2485
    ## 122                NA 4.837884 2.2506739       506       219       2504
    ## 123                48 5.379798 2.9027778       433       155       2389
    ## 124                 8 4.390879 1.0000000       485         8       2826
    ## 125                NA 3.381510 1.0400000       426         9       2524
    ## 126                NA 3.646778 2.0000000       434        13       2764
    ## 127                NA 3.600624 0.2727273       427        10       2283
    ## 128                NA 4.019231 0.2000000       373         2       1963
    ## 129                16 3.943636 0.5000000       388         2       2077
    ## 130                16       NA        NA        NA        NA         NA
    ## 131                NA       NA        NA        NA        NA         NA
    ## 132                NA       NA        NA        NA        NA         NA
    ## 133                19 3.630476 1.2371134       343        36       1698
    ## 134                NA 3.978599 1.7804878       392       121       1843
    ## 135                NA 3.094880 1.4549550       432       112       1811
    ## 136                NA 3.991667 2.3740741       480       169       2256
    ## 137                NA 4.746606 3.7000000       436       178       1965
    ## 138                40 4.211321 3.0911950       478       221       2044
    ## 139                19 3.024548 1.4324324       328        41       2138
    ## 140                NA 2.903723 1.4273504       301        61       1990
    ## 141                NA 3.215859 1.0863309       259        29       1332
    ## 142                NA 3.012594 1.0974026       270        21       1054
    ## 143                NA 2.822259 1.4133333       306        50       1518
    ## 144                18 3.534173 1.3052632       372        47       1748
    ## 145                11 2.917355 1.0833333       193         6        654
    ## 146                NA 3.815141 1.0000000       434         6       1995
    ## 147                NA 3.910448 1.0000000       344         1       1393
    ## 148                NA 3.814815 1.2032520       353        29       1482
    ## 149                NA 4.421222 1.0000000       303         4       1278
    ## 150                12 3.860795 1.1680000       309        62       1349
    ## 151                22 4.643200 1.5827338       373        71       2740
    ## 152                NA 4.600000 2.4847328       419       145       2562
    ## 153                NA 4.127941 2.8042169       455       217       2589
    ## 154                NA 5.362500 3.7310924       553       291       2978
    ## 155                NA 4.267409 2.7413793       578       298       2940
    ## 156                39 4.472993 3.0614525       555       237       2895
    ## 157                33 4.690751 3.4000000       278       119       1450
    ## 158                NA       NA        NA        NA        NA         NA
    ## 159                NA 4.316279 3.9196891       333       307       1668
    ## 160                NA 4.857143 3.5238095       398       188       2518
    ## 161                NA 4.345515 3.2919897       437       261       2410
    ## 162                46 4.111413 3.3643411       452       273       3076
    ## 163                18 3.616867 1.3661972       317        37       1361
    ## 164                NA 3.869654 1.4228571       287        72       1663
    ## 165                NA 3.846626 1.4976077       318        89       1706
    ## 166                NA 3.973469 1.7405858       364       114       1754
    ## 167                NA 3.479393 2.1791908       327       114       1385
    ## 168                35 4.254505 2.4800000       385       154       1788
    ## 169                16 2.788793 1.2758621       178        34        584
    ## 170                NA 3.367292 1.6557377       255        71       1139
    ## 171                NA 3.210084 1.8369099       233       131       1006
    ## 172                NA 3.683406 2.3500000       309       139       1470
    ## 173                NA 3.708934 3.5852535       272       177       1098
    ## 174                32 4.239437 2.9607843       391       164       1889
    ## 175                14 3.304189 1.0086207       295         6       1643
    ## 176                NA 4.003072 1.0114286       331        18       2448
    ## 177                NA 4.382550 0.6842105       345        13       2417
    ## 178                NA 3.885113 0.8947368       361        13       2214
    ## 179                NA 4.872014 0.2553191       415         9       2560
    ## 180                21 4.349353 1.2883436       454        52       2391
    ## 181                13 3.607088 0.9000000       366        11       2054
    ## 182                NA 3.843411 1.0265487       403        15       2211
    ## 183                NA 4.015699 1.0066667       430        21       2278
    ## 184                NA 3.961832 1.1250000       409        26       1863
    ## 185                NA 3.859624 0.4130435       465        42       2360
    ## 186                22 4.341549 1.0843373       425       101       2219
    ## 187                22 3.494327 1.5333333       322        91       1870
    ## 188                NA 4.222034 2.2481481       405       141       2219
    ## 189                NA 3.552459 2.9875260       396       233       1872
    ## 190                NA 3.667638 2.7272727       480       186       2233
    ## 191                NA 3.906856 2.1420613       491       211       2729
    ## 192                45 3.603473 2.0717489       475       185       2363
    ## 193                 9 3.686347 1.5000000       228         3        927
    ## 194                NA 4.142562 1.0000000       454         2       1856
    ## 195                NA 3.990826 1.0000000       323         1       1590
    ## 196                NA 3.743333 1.0000000       352         6       2054
    ## 197                NA 3.414894 1.0000000       355         3       1388
    ## 198                10 4.003257 2.6315789       585        12       2202
    ## 199                11 2.287293 1.2500000       206        13        788
    ## 200                NA 2.879925 1.0000000       285         2       1393
    ## 201                NA 3.404959 1.0833333       272         3       1504
    ## 202                NA 3.110932 1.2100840       281        18       1726
    ## 203                NA 4.185417 0.6756757       383         9       1917
    ## 204                18 3.965392 0.7536232       326        14       1852
    ## 205                22 2.743455 1.3766234       214        67        893
    ## 206                NA 2.667722 1.9481132       220       100        748
    ## 207                NA 3.866834 1.8056872       318       100       1350
    ## 208                NA 3.997126 2.1962617       321       107       1224
    ## 209                NA 3.419664 2.5758929       298       120       1223
    ## 210                32 3.370937 2.2745098       342       157       1540
    ## 211                21 3.561364 1.2641509       291        24       1344
    ## 212                NA 3.636519 1.9342916       300       128       1784
    ## 213                NA 3.675134 2.5710145       366       155       2328
    ## 214                NA 4.001613 1.9743590       361        29       2145
    ## 215                NA 4.069659 2.5541796       436       161       2279
    ## 216                34 4.239198 2.3815789       411       156       2438
    ## 217                13 3.921109 1.2307692       281         8       1631
    ## 218                NA 3.942164 1.3103448       321        44       1843
    ## 219                NA 4.595568 1.7500000       323        76       1504
    ## 220                NA 3.705441 2.2594142       295       136       1715
    ## 221                NA 4.137405 2.2787611       366       145       1979
    ## 222                31 4.388235 2.5614754       324       165       1978
    ## 223                26 4.135036 0.4805825       381        39       1988
    ## 224                NA 4.054054 1.3432836       416       100       2340
    ## 225                NA 3.642412 1.8812665       330       175       1570
    ## 226                NA 5.050000 2.5159817       410       148       2252
    ## 227                NA 4.658802 2.7468750       407       228       2314
    ## 228                48 4.240798 3.0774194       429       217       2510
    ## 229                28 3.420975 1.3322785       342        96       2035
    ## 230                NA 4.073282 2.1727273       435       142       2290
    ## 231                NA 4.577491 2.8690476       420       175       2146
    ## 232                NA 4.169162 2.6475410       513       195       2482
    ## 233                NA 4.917927 3.5185185       447       210       1999
    ## 234                43       NA        NA        NA        NA         NA
    ## 235                19 3.298748 1.2043011       274        36       1537
    ## 236                NA 2.760976 1.2571429       277        55       1481
    ## 237                NA 3.607664 1.8880000       327        51       1709
    ## 238                NA 3.714556 1.7000000       302        36       1675
    ## 239                NA 2.902089 1.6511628       321        90       1951
    ## 240                29 4.100707 1.6447368       330        55       1987
    ## 241                13 2.997050 1.0175439       252         9       1827
    ## 242                NA 2.900838 1.2195122       295        52       1882
    ## 243                NA 3.723664 2.0704225       346       134       2148
    ## 244                NA 3.812930 2.9900000       430       180       2377
    ## 245                NA       NA        NA        NA        NA         NA
    ## 246                37 3.926928 2.1586207       417       170       2508
    ## 247                17 3.093146 1.0262009       333        32       1547
    ## 248                NA 3.751332 1.1974249       404        97       1847
    ## 249                NA 4.050505 1.9086294       304       127       1426
    ## 250                NA 4.118790 2.2325581       366       151       1689
    ## 251                NA 3.969147 2.2892308       415       187       1936
    ## 252                36 4.061475 2.8526316       397       213       1741
    ## 253                19 4.033333 1.3034483       373        47       2334
    ## 254                NA 4.100167 2.0046729       368       124       2201
    ## 255                NA 4.057047 2.4911032       367       155       2124
    ## 256                NA 4.611247 3.8300395       339       193       1726
    ## 257                NA 3.921444 3.7749077       358       213       1600
    ## 258                46 3.391525 3.0727969       357       219       1764
    ## 259                27 3.341418 1.9144981       271       101       1591
    ## 260                NA 3.505582 1.9832215       309       170       2031
    ## 261                NA 3.573574 2.3053892       331       221       2141
    ## 262                NA 3.905213 2.6837838       390       243       2158
    ## 263                NA 4.232258 2.8665049       396       262       2326
    ## 264                45 4.250853 2.6794872       374       219       2227
    ## 265                20 3.088757 1.2592593       275         9       1417
    ## 266                NA 3.445545 1.3043478       271        52       1515
    ## 267                NA 4.106212 2.2313433       300       102       1740
    ## 268                NA 4.256790 2.5370370       302       122       1556
    ## 269                NA 4.113846 3.2000000       307       131       1207
    ## 270                42       NA        NA        NA        NA         NA
    ## 271                20 2.776181 0.5584416       258        20       1188
    ## 272                NA 3.171717 0.9051724       288        41       1375
    ## 273                NA 3.395899 1.1722689       370        95       1912
    ## 274                NA 4.019928 1.7600000       424       108       1909
    ## 275                NA 4.329810 2.2558140       418       142       1791
    ## 276                17       NA        NA        NA        NA         NA
    ## 277                NA       NA        NA        NA        NA         NA
    ## 278                 9 4.883966 1.1666667       387        10       2144
    ## 279                NA 3.830795 1.2258065       392         6       1973
    ## 280                NA 2.821782 1.0000000       245         2       1018
    ## 281                NA 2.377171 1.1200000       203        11        861
    ## 282                NA 3.637965 1.0000000       344         1       1609
    ## 283                10 3.460548 1.1610169       351        10       1918
    ## 284                16       NA        NA        NA        NA         NA
    ## 285                NA       NA        NA        NA        NA         NA
    ## 286                20 3.943005 1.0761421       260        13       1347
    ## 287                NA 4.183445 2.2481481       295       128       1653
    ## 288                NA 3.960265 2.2893617       364       149       1568
    ## 289                NA 4.190698 3.1622419       343       211       1559
    ## 290                NA 3.673418 2.7388060       315       131       1224
    ## 291                46 4.676101 2.7600000       322        34       1371
    ## 292                10 3.765528 1.2500000       303        17       2147
    ## 293                NA 3.555804 1.0000000       272         2       1344
    ## 294                NA 3.339114 1.0000000       280         1       1482
    ## 295                NA 3.162554 1.4102564       369        79       1871
    ## 296                21 3.370093 1.4734513       319        98       1565
    ## 297                27 4.030075 1.4258373       255        73       1452
    ## 298                NA 4.745238 2.4967742       408       126       1867
    ## 299                NA 4.737127 3.1124498       323       151       1609
    ## 300                NA 4.880240 3.4809524       340       229       1452
    ## 301                NA 5.743772 3.5371429       425       209       1525
    ## 302                48 5.247093 3.5950000       383       217       1701
    ## 303                10 3.704110 1.1000000       265         8       1215
    ## 304                NA 3.563356 1.3628319       381        20       1851
    ## 305                NA 3.979950 1.1666667       368        27       1423
    ## 306                NA 4.009934 1.0000000       358         1       1145
    ## 307                NA 3.864407 0.0000000       364         0       1447
    ## 308                11 3.886842 1.3333333       370         4       1396
    ## 309                14 3.435743 1.1818182       331         7       1503
    ## 310                NA 4.073350 1.1976744       309        20       1483
    ## 311                NA 4.022624 1.5529412       333        56       1562
    ## 312                NA 4.137014 3.0289855       363       112       1829
    ## 313                NA 5.185941 2.9215686       388       108       1986
    ## 314                40 5.153639 2.7612903       383       140       1866
    ## 315                22 5.344227 1.4086957       441        92       2267
    ## 316                NA 4.567329 1.7359551       415       122       1845
    ## 317                NA 5.288991 3.3037543       474       200       2142
    ## 318                NA 5.338462 3.0775510       554       177       2585
    ## 319                NA 4.983389 2.8321678       563       219       2772
    ## 320                44 5.587332 2.4292929       548       163       2887
    ## 321                30 4.159159 1.9397163       236       120       1170
    ## 322                NA 4.557471 3.2179487       197       126        686
    ## 323                NA 4.078292 3.1313559       219       152       1020
    ## 324                NA 4.458937 3.6340694       232       217        798
    ## 325                NA 4.857143 3.8225806       278       217       1067
    ## 326                36 3.706790 3.2439024       249       102       1024
    ## 327                22 3.487871 1.3139535       214        32       1118
    ## 328                NA 3.609881 2.0466667       352        88       1905
    ## 329                NA 4.298667 2.3309353       299        85       1418
    ## 330                NA 3.819961 3.4065041       332       153       1699
    ## 331                NA 3.750000 3.6072874       394       244       1977
    ## 332                41 4.186161 2.8921569       437       183       2347
    ## 333                17 3.509138 1.1846154       178        11       1144
    ## 334                NA 3.789634 1.7902098       219        70       1058
    ## 335                NA 4.231362 2.4932432       292       168       1470
    ## 336                NA 4.177340 2.8620690       318       205       1524
    ## 337                NA       NA        NA        NA        NA         NA
    ## 338                38 3.957230 2.9092742       367       260       1731
    ## 339                11 2.548969 1.0444444       195         9        955
    ## 340                NA 3.430642 1.0000000       285         1       1417
    ## 341                NA 2.963303 1.7500000       252         9       1393
    ## 342                17 3.474725 1.0588235       284         6       1390
    ## 343                10 3.833770 0.0000000       386         0       2613
    ## 344                NA 3.688478 1.0000000       389         1       2274
    ## 345                NA 4.145588 0.1621622       542         3       2717
    ## 346                NA 3.536415 0.0000000       400         1       2196
    ## 347                NA 3.693846 0.2000000       435         3       2070
    ## 348                 9 3.241422 0.0156250       444         2       2450
    ## 349                24 2.747100 1.1809045       338        98       2084
    ## 350                NA 3.372320 1.4432990       260        78       1511
    ## 351                NA 3.774030 1.8000000       346       153       1891
    ## 352                NA 3.471510 2.0127796       359       144       2109
    ## 353                NA 3.290683 1.7591463       360       158       2276
    ## 354                30 3.320000 2.1553398       335       197       2221
    ## 355                22 3.432387 1.1830065       345        62       1805
    ## 356                NA 4.001724 1.7709251       334       120       1959
    ## 357                NA 4.210407 2.1157895       257       101       1610
    ## 358                NA 4.697624 2.7148289       330       157       1974
    ## 359                NA 4.424837 2.6038462       341       132       1916
    ## 360                NA 4.113158 2.8480000       303       158       1460
    ## 361                30 3.604140 2.8763441       400       149       2587
    ## 362                NA 4.604341 2.7840000       413       149       2534
    ## 363                NA 4.907591 4.1318681       459       196       2841
    ## 364                NA 4.085409 3.3598326       539       214       3163
    ## 365                NA 4.223572 2.9655172       521       145       3090
    ## 366                50 4.080446 3.4415584       505       226       3072
    ## 367                16 3.030405 1.0375000       303        15       1579
    ## 368                NA 3.302663 1.0990991       250        48       1205
    ## 369                NA 4.520325 1.5828571       313        70       1618
    ## 370                NA 3.312388 1.3204225       348        61       1644
    ## 371                NA 2.965928 1.6688963       320       122       1527
    ## 372                31 3.514403 1.4012346       311        58       1481
    ##     TOKENS_CHI mTOKENCHI
    ## 1           NA  389.8011
    ## 2          139  389.8011
    ## 3          148  389.8011
    ## 4          255  389.8011
    ## 5          321  389.8011
    ## 6          472  389.8011
    ## 7          686  389.8011
    ## 8          461  389.8011
    ## 9          562  389.8011
    ## 10         983  389.8011
    ## 11         674  389.8011
    ## 12          NA  389.8011
    ## 13         698  389.8011
    ## 14         260  389.8011
    ## 15         530  389.8011
    ## 16         542  389.8011
    ## 17         754  389.8011
    ## 18         588  389.8011
    ## 19         460  389.8011
    ## 20         269  389.8011
    ## 21         555  389.8011
    ## 22         490  389.8011
    ## 23         479  389.8011
    ## 24         539  389.8011
    ## 25         738  389.8011
    ## 26         197  389.8011
    ## 27         487  389.8011
    ## 28         468  389.8011
    ## 29         604  389.8011
    ## 30         404  389.8011
    ## 31         538  389.8011
    ## 32          29  389.8011
    ## 33         149  389.8011
    ## 34          62  389.8011
    ## 35         205  389.8011
    ## 36         349  389.8011
    ## 37         300  389.8011
    ## 38         130  389.8011
    ## 39         169  389.8011
    ## 40         210  389.8011
    ## 41         494  389.8011
    ## 42         194  389.8011
    ## 43          NA  389.8011
    ## 44         143  389.8011
    ## 45         554  389.8011
    ## 46          NA  389.8011
    ## 47         493  389.8011
    ## 48         323  389.8011
    ## 49         197  389.8011
    ## 50         137  389.8011
    ## 51         132  389.8011
    ## 52         453  389.8011
    ## 53         390  389.8011
    ## 54         346  389.8011
    ## 55          NA  389.8011
    ## 56          83  389.8011
    ## 57          55  389.8011
    ## 58         188  389.8011
    ## 59         414  389.8011
    ## 60         431  389.8011
    ## 61         189  389.8011
    ## 62          NA  389.8011
    ## 63          NA  389.8011
    ## 64         101  389.8011
    ## 65         117  389.8011
    ## 66         193  389.8011
    ## 67         354  389.8011
    ## 68         513  389.8011
    ## 69         666  389.8011
    ## 70          62  389.8011
    ## 71         203  389.8011
    ## 72         733  389.8011
    ## 73         243  389.8011
    ## 74         916  389.8011
    ## 75         640  389.8011
    ## 76         249  389.8011
    ## 77         393  389.8011
    ## 78         630  389.8011
    ## 79         538  389.8011
    ## 80         932  389.8011
    ## 81         864  389.8011
    ## 82         105  389.8011
    ## 83         297  389.8011
    ## 84         368  389.8011
    ## 85         557  389.8011
    ## 86         755  389.8011
    ## 87         793  389.8011
    ## 88          99  389.8011
    ## 89         159  389.8011
    ## 90         252  389.8011
    ## 91         495  389.8011
    ## 92         160  389.8011
    ## 93         410  389.8011
    ## 94         254  389.8011
    ## 95         733  389.8011
    ## 96         825  389.8011
    ## 97         793  389.8011
    ## 98         583  389.8011
    ## 99         710  389.8011
    ## 100         37  389.8011
    ## 101         82  389.8011
    ## 102         58  389.8011
    ## 103         NA  389.8011
    ## 104          9  389.8011
    ## 105         37  389.8011
    ## 106        406  389.8011
    ## 107        738  389.8011
    ## 108        940  389.8011
    ## 109       1092  389.8011
    ## 110        769  389.8011
    ## 111       1079  389.8011
    ## 112        227  389.8011
    ## 113        335  389.8011
    ## 114        330  389.8011
    ## 115        455  389.8011
    ## 116        197  389.8011
    ## 117        274  389.8011
    ## 118        214  389.8011
    ## 119        362  389.8011
    ## 120        503  389.8011
    ## 121        717  389.8011
    ## 122        792  389.8011
    ## 123        590  389.8011
    ## 124        122  389.8011
    ## 125         78  389.8011
    ## 126         96  389.8011
    ## 127         93  389.8011
    ## 128          5  389.8011
    ## 129          2  389.8011
    ## 130         NA  389.8011
    ## 131         NA  389.8011
    ## 132         NA  389.8011
    ## 133        118  389.8011
    ## 134        414  389.8011
    ## 135        305  389.8011
    ## 136        592  389.8011
    ## 137        686  389.8011
    ## 138        847  389.8011
    ## 139        103  389.8011
    ## 140        157  389.8011
    ## 141        148  389.8011
    ## 142        162  389.8011
    ## 143        197  389.8011
    ## 144        236  389.8011
    ## 145         26  389.8011
    ## 146         32  389.8011
    ## 147         40  389.8011
    ## 148        143  389.8011
    ## 149         34  389.8011
    ## 150        143  389.8011
    ## 151        433  389.8011
    ## 152        623  389.8011
    ## 153        826  389.8011
    ## 154       1225  389.8011
    ## 155       1145  389.8011
    ## 156       1010  389.8011
    ## 157        483  389.8011
    ## 158         NA  389.8011
    ## 159       1293  389.8011
    ## 160        714  389.8011
    ## 161       1154  389.8011
    ## 162       1249  389.8011
    ## 163         95  389.8011
    ## 164        236  389.8011
    ## 165        299  389.8011
    ## 166        384  389.8011
    ## 167        318  389.8011
    ## 168        530  389.8011
    ## 169        111  389.8011
    ## 170        258  389.8011
    ## 171        403  389.8011
    ## 172        502  389.8011
    ## 173        622  389.8011
    ## 174        521  389.8011
    ## 175        117  389.8011
    ## 176        177  389.8011
    ## 177         39  389.8011
    ## 178         39  389.8011
    ## 179        179  389.8011
    ## 180        204  389.8011
    ## 181         21  389.8011
    ## 182        116  389.8011
    ## 183        151  389.8011
    ## 184         79  389.8011
    ## 185        193  389.8011
    ## 186        195  389.8011
    ## 187        319  389.8011
    ## 188        542  389.8011
    ## 189       1246  389.8011
    ## 190        750  389.8011
    ## 191        690  389.8011
    ## 192        418  389.8011
    ## 193          3  389.8011
    ## 194          7  389.8011
    ## 195          1  389.8011
    ## 196         14  389.8011
    ## 197         33  389.8011
    ## 198        100  389.8011
    ## 199         35  389.8011
    ## 200         10  389.8011
    ## 201         26  389.8011
    ## 202        142  389.8011
    ## 203         42  389.8011
    ## 204         79  389.8011
    ## 205        180  389.8011
    ## 206        368  389.8011
    ## 207        365  389.8011
    ## 208        438  389.8011
    ## 209        537  389.8011
    ## 210        605  389.8011
    ## 211        260  389.8011
    ## 212        879  389.8011
    ## 213        702  389.8011
    ## 214         74  389.8011
    ## 215        736  389.8011
    ## 216        611  389.8011
    ## 217         16  389.8011
    ## 218        107  389.8011
    ## 219        218  389.8011
    ## 220        477  389.8011
    ## 221        459  389.8011
    ## 222        568  389.8011
    ## 223        337  389.8011
    ## 224        433  389.8011
    ## 225        694  389.8011
    ## 226        510  389.8011
    ## 227        815  389.8011
    ## 228        897  389.8011
    ## 229        398  389.8011
    ## 230        439  389.8011
    ## 231        825  389.8011
    ## 232        576  389.8011
    ## 233        800  389.8011
    ## 234         NA  389.8011
    ## 235        109  389.8011
    ## 236        126  389.8011
    ## 237        232  389.8011
    ## 238        135  389.8011
    ## 239        340  389.8011
    ## 240        110  389.8011
    ## 241         58  389.8011
    ## 242        148  389.8011
    ## 243        404  389.8011
    ## 244        493  389.8011
    ## 245         NA  389.8011
    ## 246        571  389.8011
    ## 247        235  389.8011
    ## 248        264  389.8011
    ## 249        352  389.8011
    ## 250        434  389.8011
    ## 251        651  389.8011
    ## 252        702  389.8011
    ## 253        176  389.8011
    ## 254        423  389.8011
    ## 255        604  389.8011
    ## 256        820  389.8011
    ## 257        875  389.8011
    ## 258        719  389.8011
    ## 259        450  389.8011
    ## 260        536  389.8011
    ## 261       1023  389.8011
    ## 262        822  389.8011
    ## 263       1054  389.8011
    ## 264        921  389.8011
    ## 265         68  389.8011
    ## 266        120  389.8011
    ## 267        528  389.8011
    ## 268        378  389.8011
    ## 269        433  389.8011
    ## 270         NA  389.8011
    ## 271         91  389.8011
    ## 272        165  389.8011
    ## 273        354  389.8011
    ## 274        433  389.8011
    ## 275        502  389.8011
    ## 276         NA  389.8011
    ## 277         NA  389.8011
    ## 278         63  389.8011
    ## 279         38  389.8011
    ## 280          3  389.8011
    ## 281         55  389.8011
    ## 282         32  389.8011
    ## 283        137  389.8011
    ## 284         NA  389.8011
    ## 285         NA  389.8011
    ## 286        212  389.8011
    ## 287        571  389.8011
    ## 288        486  389.8011
    ## 289        973  389.8011
    ## 290        304  389.8011
    ## 291         61  389.8011
    ## 292         40  389.8011
    ## 293          4  389.8011
    ## 294         45  389.8011
    ## 295        255  389.8011
    ## 296        306  389.8011
    ## 297        286  389.8011
    ## 298        356  389.8011
    ## 299        671  389.8011
    ## 300        684  389.8011
    ## 301        586  389.8011
    ## 302        646  389.8011
    ## 303         43  389.8011
    ## 304        147  389.8011
    ## 305         77  389.8011
    ## 306         70  389.8011
    ## 307          0  389.8011
    ## 308          8  389.8011
    ## 309         38  389.8011
    ## 310         97  389.8011
    ## 311        122  389.8011
    ## 312        348  389.8011
    ## 313        238  389.8011
    ## 314        395  389.8011
    ## 315        319  389.8011
    ## 316        298  389.8011
    ## 317        897  389.8011
    ## 318        642  389.8011
    ## 319        723  389.8011
    ## 320        436  389.8011
    ## 321        473  389.8011
    ## 322        449  389.8011
    ## 323        660  389.8011
    ## 324        978  389.8011
    ## 325        814  389.8011
    ## 326        358  389.8011
    ## 327        109  389.8011
    ## 328        287  389.8011
    ## 329        261  389.8011
    ## 330        694  389.8011
    ## 331        725  389.8011
    ## 332        517  389.8011
    ## 333        154  389.8011
    ## 334        244  389.8011
    ## 335        672  389.8011
    ## 336       1051  389.8011
    ## 337         NA  389.8011
    ## 338       1294  389.8011
    ## 339         47  389.8011
    ## 340         19  389.8011
    ## 341         14  389.8011
    ## 342         36  389.8011
    ## 343          0  389.8011
    ## 344         44  389.8011
    ## 345         41  389.8011
    ## 346          1  389.8011
    ## 347         30  389.8011
    ## 348         64  389.8011
    ## 349        233  389.8011
    ## 350        260  389.8011
    ## 351        393  389.8011
    ## 352        574  389.8011
    ## 353        531  389.8011
    ## 354        618  389.8011
    ## 355        244  389.8011
    ## 356        401  389.8011
    ## 357        424  389.8011
    ## 358        637  389.8011
    ## 359        636  389.8011
    ## 360        659  389.8011
    ## 361        469  389.8011
    ## 362        670  389.8011
    ## 363        698  389.8011
    ## 364        693  389.8011
    ## 365        357  389.8011
    ## 366        713  389.8011
    ## 367        166  389.8011
    ## 368        356  389.8011
    ## 369        254  389.8011
    ## 370        334  389.8011
    ## 371        511  389.8011
    ## 372        210  389.8011

``` r
# Task 2
df3 %>% 
  group_by(SUBJ) %>% 
  mutate(mTOKENCHI = mean(TOKENS_CHI, na.rm = T))
```

    ## # A tibble: 372 x 21
    ## # Groups:   SUBJ [66]
    ##     SUBJ ADOS1 SOCIALIZATION1 MULLENRAW1 EXPRESSIVELANGR… VISIT ETHNICITY
    ##    <dbl> <dbl>          <dbl>      <dbl>            <dbl> <dbl> <chr>    
    ##  1     1    15            104         NA               NA     1 White    
    ##  2     2     0            108         28               14     1 White    
    ##  3     2     0            108         28               14     2 White    
    ##  4     2     0            108         28               14     3 White    
    ##  5     2     0            108         28               14     4 White    
    ##  6     2     0            108         28               14     5 White    
    ##  7     2     0            108         28               14     6 White    
    ##  8     3    13             85         34               27     1 White    
    ##  9     3    13             85         34               27     2 White    
    ## 10     3    13             85         34               27     3 White    
    ## # … with 362 more rows, and 14 more variables: DIAGNOSIS <chr>,
    ## #   GENDER <chr>, AGE <dbl>, ADOS <dbl>, SOCIALIZATION <dbl>,
    ## #   MULLENRAW <dbl>, EXPRESSIVELANGRAW <dbl>, MOT_MLU <dbl>,
    ## #   CHI_MLU <dbl>, TYPES_MOT <dbl>, TYPES_CHI <dbl>, TOKENS_MOT <dbl>,
    ## #   TOKENS_CHI <dbl>, mTOKENCHI <dbl>

``` r
# Task 3
# We are interested in the development of the children and their language, thus the mean is not that informing. It is a generalisation of the data, which does not take development into account.
```
