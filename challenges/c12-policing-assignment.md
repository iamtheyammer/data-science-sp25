Massachusetts Highway Stops
================
Sam Mendelson
2025-04-27

- [Grading Rubric](#grading-rubric)
  - [Individual](#individual)
  - [Submission](#submission)
- [Setup](#setup)
  - [**q1** Go to the Stanford Open Policing Project page and download
    the Massachusetts State Police records in `Rds` format. Move the
    data to your `data` folder and match the `filename` to load the
    data.](#q1-go-to-the-stanford-open-policing-project-page-and-download-the-massachusetts-state-police-records-in-rds-format-move-the-data-to-your-data-folder-and-match-the-filename-to-load-the-data)
- [EDA](#eda)
  - [**q2** Do your “first checks” on the dataset. What are the basic
    facts about this
    dataset?](#q2-do-your-first-checks-on-the-dataset-what-are-the-basic-facts-about-this-dataset)
  - [**q3** Check the set of factor levels for `subject_race` and
    `raw_Race`. What do you note about overlap / difference between the
    two
    sets?](#q3-check-the-set-of-factor-levels-for-subject_race-and-raw_race-what-do-you-note-about-overlap--difference-between-the-two-sets)
  - [**q4** Check whether `subject_race` and `raw_Race` match for a
    large fraction of cases. Which of the two hypotheses above is most
    likely, based on your
    results?](#q4-check-whether-subject_race-and-raw_race-match-for-a-large-fraction-of-cases-which-of-the-two-hypotheses-above-is-most-likely-based-on-your-results)
  - [Vis](#vis)
    - [**q5** Compare the *arrest rate*—the fraction of total cases in
      which the subject was arrested—across different factors. Create as
      many visuals (or tables) as you need, but make sure to check the
      trends across all of the `subject` variables. Answer the questions
      under *observations*
      below.](#q5-compare-the-arrest-ratethe-fraction-of-total-cases-in-which-the-subject-was-arrestedacross-different-factors-create-as-many-visuals-or-tables-as-you-need-but-make-sure-to-check-the-trends-across-all-of-the-subject-variables-answer-the-questions-under-observations-below)
- [Modeling](#modeling)
  - [**q6** Run the following code and interpret the regression
    coefficients. Answer the the questions under *observations*
    below.](#q6-run-the-following-code-and-interpret-the-regression-coefficients-answer-the-the-questions-under-observations-below)
  - [**q7** Re-fit the logistic regression from q6 setting `"white"` as
    the reference level for `subject_race`. Interpret the the model
    terms and answer the questions
    below.](#q7-re-fit-the-logistic-regression-from-q6-setting-white-as-the-reference-level-for-subject_race-interpret-the-the-model-terms-and-answer-the-questions-below)
  - [**q8** Re-fit the model using a factor indicating the presence of
    contraband in the subject’s vehicle. Answer the questions under
    *observations*
    below.](#q8-re-fit-the-model-using-a-factor-indicating-the-presence-of-contraband-in-the-subjects-vehicle-answer-the-questions-under-observations-below)
  - [**q9** Go deeper: Pose at least one more question about the data
    and fit at least one more model in support of answering that
    question.](#q9-go-deeper-pose-at-least-one-more-question-about-the-data-and-fit-at-least-one-more-model-in-support-of-answering-that-question)
  - [Further Reading](#further-reading)

*Purpose*: In this last challenge we’ll focus on using logistic
regression to study a large, complicated dataset. Interpreting the
results of a model can be challenging—both in terms of the statistics
and the real-world reasoning—so we’ll get some practice in this
challenge.

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category | Needs Improvement | Satisfactory |
|----|----|----|
| Effort | Some task **q**’s left unattempted | All task **q**’s attempted |
| Observed | Did not document observations, or observations incorrect | Documented correct observations based on analysis |
| Supported | Some observations not clearly supported by analysis | All observations clearly supported by analysis (table, graph, etc.) |
| Assessed | Observations include claims not supported by the data, or reflect a level of certainty not warranted by the data | Observations are appropriately qualified by the quality & relevance of the data and (in)conclusiveness of the support |
| Specified | Uses the phrase “more data are necessary” without clarification | Any statement that “more data are necessary” specifies which *specific* data are needed to answer what *specific* question |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability | Code sufficiently close to the [style guide](https://style.tidyverse.org/) |

## Submission

<!-- ------------------------- -->

Make sure to commit both the challenge report (`report.md` file) and
supporting files (`report_files/` folder) when you are done! Then submit
a link to Canvas. **Your Challenge submission is not complete without
all files uploaded to GitHub.**

*Background*: We’ll study data from the [Stanford Open Policing
Project](https://openpolicing.stanford.edu/data/), specifically their
dataset on Massachusetts State Patrol police stops.

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(broom)
```

# Setup

<!-- -------------------------------------------------- -->

### **q1** Go to the [Stanford Open Policing Project](https://openpolicing.stanford.edu/data/) page and download the Massachusetts State Police records in `Rds` format. Move the data to your `data` folder and match the `filename` to load the data.

*Note*: An `Rds` file is an R-specific file format. The function
`readRDS` will read these files.

``` r
## TODO: Download the data, move to your data folder, and load it
filename <- "data/yg821jf8611_ma_statewide_2020_04_01.rds"
df_data <- readRDS(filename)
```

# EDA

<!-- -------------------------------------------------- -->

### **q2** Do your “first checks” on the dataset. What are the basic facts about this dataset?

``` r
summary(df_data)
```

    ##  raw_row_number          date              location         county_name       
    ##  Length:3416238     Min.   :2007-01-01   Length:3416238     Length:3416238    
    ##  Class :character   1st Qu.:2009-04-22   Class :character   Class :character  
    ##  Mode  :character   Median :2011-07-08   Mode  :character   Mode  :character  
    ##                     Mean   :2011-07-16                                        
    ##                     3rd Qu.:2013-08-27                                        
    ##                     Max.   :2015-12-31                                        
    ##                                                                               
    ##   subject_age                     subject_race     subject_sex     
    ##  Min.   :10.00    asian/pacific islander: 166842   male  :2362238  
    ##  1st Qu.:25.00    black                 : 351610   female:1038377  
    ##  Median :34.00    hispanic              : 338317   NA's  :  15623  
    ##  Mean   :36.47    white                 :2529780                   
    ##  3rd Qu.:46.00    other                 :  11008                   
    ##  Max.   :94.00    unknown               :  17017                   
    ##  NA's   :158006   NA's                  :   1664                   
    ##          type         arrest_made     citation_issued warning_issued 
    ##  pedestrian:      0   Mode :logical   Mode :logical   Mode :logical  
    ##  vehicular :3416238   FALSE:3323303   FALSE:1244039   FALSE:2269244  
    ##                       TRUE :92019     TRUE :2171283   TRUE :1146078  
    ##                       NA's :916       NA's :916       NA's :916      
    ##                                                                      
    ##                                                                      
    ##                                                                      
    ##      outcome        contraband_found contraband_drugs contraband_weapons
    ##  warning :1146078   Mode :logical    Mode :logical    Mode :logical     
    ##  citation:2171283   FALSE:28256      FALSE:36296      FALSE:53237       
    ##  summons :      0   TRUE :27474      TRUE :19434      TRUE :2493        
    ##  arrest  :  92019   NA's :3360508    NA's :3360508    NA's :3360508     
    ##  NA's    :   6858                                                       
    ##                                                                         
    ##                                                                         
    ##  contraband_alcohol contraband_other frisk_performed search_conducted
    ##  Mode :logical      Mode :logical    Mode :logical   Mode :logical   
    ##  FALSE:3400070      FALSE:51708      FALSE:51029     FALSE:3360508   
    ##  TRUE :16168        TRUE :4022       TRUE :3602      TRUE :55730     
    ##                     NA's :3360508    NA's :3361607                   
    ##                                                                      
    ##                                                                      
    ##                                                                      
    ##          search_basis     reason_for_stop    vehicle_type      
    ##  k9            :      0   Length:3416238     Length:3416238    
    ##  plain view    :      0   Class :character   Class :character  
    ##  consent       :   6903   Mode  :character   Mode  :character  
    ##  probable cause:  25898                                        
    ##  other         :  18228                                        
    ##  NA's          :3365209                                        
    ##                                                                
    ##  vehicle_registration_state   raw_Race        
    ##  MA     :3053713            Length:3416238    
    ##  CT     :  82906            Class :character  
    ##  NY     :  69059            Mode  :character  
    ##  NH     :  51514                              
    ##  RI     :  39375                              
    ##  (Other): 109857                              
    ##  NA's   :   9814

**Observations**:

- What are the basic facts about this dataset?
  - There are over 3.4 million observations - very cool
  - The date range is between 1/1/07 and 12/31/15
  - Subjects’ ages range from 10-74 with an average of 36.47
  - Most subjects are white, and there are over 2 men per woman
  - There are then a number of boolean variables about the stop

Note that we have both a `subject_race` and `race_Raw` column. There are
a few possibilities as to what `race_Raw` represents:

- `race_Raw` could be the race of the police officer in the stop
- `race_Raw` could be an unprocessed version of `subject_race`

Let’s try to distinguish between these two possibilities.

### **q3** Check the set of factor levels for `subject_race` and `raw_Race`. What do you note about overlap / difference between the two sets?

``` r
## TODO: Determine the factor levels for subject_race and raw_Race
df_races <- df_data %>%
  group_by(raw_Race, subject_race) %>%
  summarize(subject_race_count = n())
```

    ## `summarise()` has grouped output by 'raw_Race'. You can override using the
    ## `.groups` argument.

**Observations**:

- What are the unique values for `subject_race`?
  - asian/pacific islander
  - black
  - hispanic
  - white
  - other
  - unknown
  - NA
- What are the unique values for `raw_Race`?
  - A
  - American Indian or Alaskan Native
  - Asian or Pacific Islander
  - Black
  - Hispanic
  - Middle Eastern or East Indian (South Asian)
  - None - for no operator present citations
  - White
- What is the overlap between the two sets?
  - Races that are present in `subject_race` are directly copied,
    however “other” and “unknown” comprise different groups.
  - NAs are present in both.
- What is the difference between the two sets?
  - “A” and “American Indian or Alaskan Native” are marked as “other”,
    and `None - for no operator present citations` are marked as
    “unknown” in subject_race.

### **q4** Check whether `subject_race` and `raw_Race` match for a large fraction of cases. Which of the two hypotheses above is most likely, based on your results?

*Note*: Just to be clear, I’m *not* asking you to do a *statistical*
hypothesis test.

``` r
## TODO: Devise your own way to test the hypothesis posed above.
df_data %>% filter(subject_race == "white" & raw_Race != "White")
```

    ## # A tibble: 0 × 24
    ## # ℹ 24 variables: raw_row_number <chr>, date <date>, location <chr>,
    ## #   county_name <chr>, subject_age <int>, subject_race <fct>,
    ## #   subject_sex <fct>, type <fct>, arrest_made <lgl>, citation_issued <lgl>,
    ## #   warning_issued <lgl>, outcome <fct>, contraband_found <lgl>,
    ## #   contraband_drugs <lgl>, contraband_weapons <lgl>, contraband_alcohol <lgl>,
    ## #   contraband_other <lgl>, frisk_performed <lgl>, search_conducted <lgl>,
    ## #   search_basis <fct>, reason_for_stop <chr>, vehicle_type <chr>, …

**Observations**

Between the two hypotheses:

- `race_Raw` could be the race of the police officer in the stop
- `race_Raw` could be an unprocessed version of `subject_race`

which is most plausible, based on your results?

- `race_Raw` is most plausible as an unprocessed version of
  `subject_race`, as there are zero rows where `subject_race` == “white”
  and `raw_Race` != “White”.
- While this is not completely guaranteed, it would mean that across all
  3.4+ million stops, ONLY white officers could have stopped white
  subjects, which is highly unlikely.

## Vis

<!-- ------------------------- -->

### **q5** Compare the *arrest rate*—the fraction of total cases in which the subject was arrested—across different factors. Create as many visuals (or tables) as you need, but make sure to check the trends across all of the `subject` variables. Answer the questions under *observations* below.

(Note: Create as many chunks and visuals as you need)

**Observations**:

- How does `arrest_rate` tend to vary with `subject_age`?

In general, arrest rates are highest for minors, however there are far
fewer stops involving minors.

``` r
df_data %>%
  group_by(subject_age) %>%
  summarize(arrest_rate = mean(arrest_made, na.rm = TRUE), num_obs = n()) %>%
  ggplot(aes(subject_age, arrest_rate, color = num_obs)) +
  geom_point() +
  ggtitle("Subject Age vs. Arrest Rate, MA State Police 2007-2015") +
  xlab("Subject Age") +
  ylab("Arrest Rate") +
  labs(color = "# Observations")
```

    ## Warning: Removed 1 row containing missing values or values outside the scale range
    ## (`geom_point()`).

![](c12-policing-assignment_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

- How does `arrest_rate` tend to vary with `subject_sex`?

It appears that arrest rates are highest for men.

``` r
df_data %>%
  group_by(subject_age, subject_sex) %>%
  summarize(arrest_rate = mean(arrest_made, na.rm = TRUE), num_obs = n()) %>%
  ggplot(aes(subject_age, arrest_rate, color = num_obs)) +
  geom_point() +
  facet_wrap(~subject_sex) +
  ggtitle("Subject Age vs. Arrest Rate, MA State Police 2007-2015") +
  xlab("Subject Age") +
  ylab("Arrest Rate") +
  labs(color = "# Observations")
```

    ## `summarise()` has grouped output by 'subject_age'. You can override using the
    ## `.groups` argument.

    ## Warning: Removed 3 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](c12-policing-assignment_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

- How does `arrest_rate` tend to vary with `subject_race`?

Note that for clarity, some outliers have been removed.

Overall, arrest rates are highest for the hispanic race, then black,
then white.

``` r
df_data %>%
  group_by(subject_age, subject_race) %>%
  summarize(arrest_rate = mean(arrest_made, na.rm = TRUE), num_obs = n()) %>%
  ggplot(aes(subject_age, arrest_rate, color = subject_race)) +
  geom_point() +
  ylim(0, 0.23) +
  # facet_wrap(~subject_race, scales = "free") +
  ggtitle("Subject Age vs. Arrest Rate, MA State Police 2007-2015") +
  xlab("Subject Age") +
  ylab("Arrest Rate") +
  labs(color = "# Observations")
```

    ## `summarise()` has grouped output by 'subject_age'. You can override using the
    ## `.groups` argument.

    ## Warning: Removed 9 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](c12-policing-assignment_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

# Modeling

<!-- -------------------------------------------------- -->

We’re going to use a model to study the relationship between `subject`
factors and arrest rate, but first we need to understand a bit more
about *dummy variables*

### **q6** Run the following code and interpret the regression coefficients. Answer the the questions under *observations* below.

``` r
## NOTE: No need to edit; inspect the estimated model terms.
fit_q6 <-
  glm(
    formula = arrest_made ~ subject_age + subject_race + subject_sex,
    data = df_data %>%
      filter(
        !is.na(arrest_made),
        subject_race %in% c("white", "black", "hispanic")
      ),
    family = "binomial"
  )

fit_q6 %>% tidy()
```

    ## # A tibble: 5 × 5
    ##   term                 estimate std.error statistic   p.value
    ##   <chr>                   <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)           -2.67    0.0132      -202.  0        
    ## 2 subject_age           -0.0142  0.000280     -50.5 0        
    ## 3 subject_racehispanic   0.513   0.0119        43.3 0        
    ## 4 subject_racewhite     -0.380   0.0103       -37.0 3.12e-299
    ## 5 subject_sexfemale     -0.755   0.00910      -83.0 0

**Observations**:

- Which `subject_race` levels are included in fitting the model?
  - Only white, black, and hispanic.
- Which `subject_race` levels have terms in the model?
  - Only hispanic.

You should find that each factor in the model has a level *missing* in
its set of terms. This is because R represents factors against a
*reference level*: The model treats one factor level as “default”, and
each factor model term represents a change from that “default” behavior.
For instance, the model above treats `subject_sex==male` as the
reference level, so the `subject_sexfemale` term represents the *change
in probability* of arrest due to a person being female (rather than
male).

The this reference level approach to coding factors is necessary for
[technical
reasons](https://www.andrew.cmu.edu/user/achoulde/94842/lectures/lecture10/lecture10-94842.html#why-is-one-of-the-levels-missing-in-the-regression),
but it complicates interpreting the model results. For instance; if we
want to compare two levels, neither of which are the reference level, we
have to consider the difference in their model coefficients. But if we
want to compare all levels against one “baseline” level, then we can
relevel the data to facilitate this comparison.

By default `glm` uses the first factor level present as the reference
level. Therefore we can use
`mutate(factor = fct_relevel(factor, "desired_level"))` to set our
`"desired_level"` as the reference factor.

### **q7** Re-fit the logistic regression from q6 setting `"white"` as the reference level for `subject_race`. Interpret the the model terms and answer the questions below.

``` r
## TODO: Re-fit the logistic regression, but set "white" as the reference
## level for subject_race
fit_q7 <-
  glm(
    formula = arrest_made ~ subject_age + subject_race + subject_sex,
    data = df_data %>%
      mutate(factor = fct_relevel(subject_race, "white")) %>%
      filter(
        !is.na(arrest_made),
        subject_race %in% c("white", "black", "hispanic")
      ),
    family = "binomial"
  )

fit_q7 %>% tidy()
```

    ## # A tibble: 5 × 5
    ##   term                 estimate std.error statistic   p.value
    ##   <chr>                   <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)           -2.67    0.0132      -202.  0        
    ## 2 subject_age           -0.0142  0.000280     -50.5 0        
    ## 3 subject_racehispanic   0.513   0.0119        43.3 0        
    ## 4 subject_racewhite     -0.380   0.0103       -37.0 3.12e-299
    ## 5 subject_sexfemale     -0.755   0.00910      -83.0 0

**Observations**:

- Which `subject_race` level has the highest probability of being
  arrested, according to this model? Which has the lowest probability?
  - According to this model, hispanic people have the highest
    probability of being arrested, and white people have the lowest
    probability.
- What could explain this difference in probabilities of arrest across
  race? List **multiple** possibilities.
  - There could be less hispanic people in Massachusetts, therefore they
    would be less likely to have many stops that do not result in an
    arrest (like speeding/parking tickets).
  - Hispanic people could be less likely to drive, therefore also
    resulting in less stops that do NOT result in an arrest.
  - There could be bias from the arresting officers, whose biases could
    result in more hispanic people getting arrested.
- Look at the set of variables in the dataset; do any of the columns
  relate to a potential explanation you listed?
  - There are some contraband columns, relating to types of contraband
    that officers may have found. It’s possible that biases would result
    in the fact that more arrests happen after a contraband discovery
    from a hispanic subject.
  - Reason for stop could be important, but this would require much more
    work to categorize (it’s comma-separated)

One way we can explain differential arrest rates is to include some
measure indicating the presence of an arrestable offense. We’ll do this
in a particular way in the next task.

### **q8** Re-fit the model using a factor indicating the presence of contraband in the subject’s vehicle. Answer the questions under *observations* below.

``` r
## TODO: Repeat the modeling above, but control for whether contraband was found
## during the police stop
fit_q8 <-
  glm(
    formula = arrest_made ~ subject_age + subject_race + subject_sex + contraband_found,
    data = df_data %>%
      mutate(factor = fct_relevel(subject_race, "white")) %>%
      filter(
        !is.na(arrest_made),
        subject_race %in% c("white", "black", "hispanic")
      ),
    family = "binomial"
  )

fit_q8 %>% tidy()
```

    ## # A tibble: 6 × 5
    ##   term                 estimate std.error statistic   p.value
    ##   <chr>                   <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)           -1.77    0.0387      -45.9  0        
    ## 2 subject_age            0.0225  0.000866     26.0  2.19e-149
    ## 3 subject_racehispanic   0.272   0.0315        8.62 6.99e- 18
    ## 4 subject_racewhite      0.0511  0.0270        1.90 5.80e-  2
    ## 5 subject_sexfemale     -0.306   0.0257      -11.9  1.06e- 32
    ## 6 contraband_foundTRUE   0.609   0.0192       31.7  4.29e-221

**Observations**:

- How does controlling for found contraband affect the `subject_race`
  terms in the model?
  - The presence of contraband is now the largest factor, which has
    diminished the effect of the race term significantly.
- What does the *finding of contraband* tell us about the stop? What
  does it *not* tell us about the stop?
  - The finding of contraband tells us that the stop was likely to
    result in an arrest.
  - It does NOT tell us what the contraband was, how much there was, or
    any other factors that could contribute to an arrest.

### **q9** Go deeper: Pose at least one more question about the data and fit at least one more model in support of answering that question.

How do frisks influence arrests?

``` r
## TODO: Repeat the modeling above, but control for whether contraband was found
## during the police stop
fit_q9 <-
  glm(
    formula = arrest_made ~ frisk_performed + search_conducted,
    data = df_data %>%
      mutate(factor = fct_relevel(subject_race, "white")) %>%
      filter(
        !is.na(arrest_made),
        subject_race %in% c("white", "black", "hispanic")
      ),
    family = "binomial"
  )

fit_q9 %>% tidy()
```

    ## # A tibble: 3 × 5
    ##   term                 estimate std.error statistic   p.value
    ##   <chr>                   <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)            -1.10     0.155      -7.11 1.14e- 12
    ## 2 frisk_performedTRUE    -1.38     0.0595    -23.2  8.38e-119
    ## 3 search_conductedTRUE    0.464    0.154       3.01 2.65e-  3

**Observations**:

- My question is: “How do the odds of a frisk or a search influence the
  odds of an arrest?”
- It looks like the odds of a **frisk** decreases the chance of arrest,
  but the odds of a **search** increase the odds.
- I was really curious why this was, so I did some research on the legal
  difference between a search and frisk.
  - The difference between a frisk and a search is that the purpose of a
    frisk is limited to finding weapons hidden on the suspect to ensure
    the personal safety of police officers, its scope is limited to
    weapons and not for the discovery of other evidence; a search
    generally requires a search warrant , and its purpose is mainly to
    find evidence of illegal criminal activity.
    ([source](https://www.law.cornell.edu/wex/frisk#:~:text=The%20difference%20between%20a%20frisk,warrant%20%2C%20and%20its%20purpose%20is))
- Since a search requires a warrant, it makes sense that the arrest
  probability would rise: if a judge believes that there may be evidence
  for an arrest, then there is a reasonable chance that one is made
  after the search.
- And, since a frisk helps to ensure that there are no weapons on a
  subject (or that those weapons are disclosed), officers might feel
  safer and therefore less likely to arrest the subject.
- Not really an observation, but it was interesting to learn the (legal)
  difference between a search and a frisk.

## Further Reading

<!-- -------------------------------------------------- -->

- Stanford Open Policing Project
  [findings](https://openpolicing.stanford.edu/findings/).
