Welcome to the second exciting part of the Language Development in ASD exercise
-------------------------------------------------------------------------------

In this exercise we will delve more in depth with different practices of
model comparison and model selection, by first evaluating your models
from last time against some new data. Does the model generalize well?
Then we will learn to do better by cross-validating models and
systematically compare them.

The questions to be answered (in a separate document) are: 1- Discuss
the differences in performance of your model in training and testing
data 2- Which individual differences should be included in a model that
maximizes your ability to explain/predict new data? 3- Predict a new
kid’s performance (Bernie) and discuss it against expected performance
of the two groups

Learning objectives
-------------------

-   Critically appraise the predictive framework (contrasted to the
    explanatory framework)
-   Learn the basics of machine learning workflows: training/testing,
    cross-validation, feature selections

Let’s go
--------

N.B. There are several datasets for this exercise, so pay attention to
which one you are using!

1.  The (training) dataset from last time (the awesome one you produced
    :-) ).
2.  The (test) datasets on which you can test the models from last time:

-   Demographic and clinical data:
    <a href="https://www.dropbox.com/s/ra99bdvm6fzay3g/demo_test.csv?dl=1" class="uri">https://www.dropbox.com/s/ra99bdvm6fzay3g/demo_test.csv?dl=1</a>
-   Utterance Length data:
    <a href="https://www.dropbox.com/s/uxtqqzl18nwxowq/LU_test.csv?dl=1" class="uri">https://www.dropbox.com/s/uxtqqzl18nwxowq/LU_test.csv?dl=1</a>
-   Word data:
    <a href="https://www.dropbox.com/s/1ces4hv8kh0stov/token_test.csv?dl=1" class="uri">https://www.dropbox.com/s/1ces4hv8kh0stov/token_test.csv?dl=1</a>

### Exercise 1) Testing model performance

How did your models from last time perform? In this exercise you have to
compare the results on the training data () and on the test data. Report
both of them. Compare them. Discuss why they are different.

-   recreate the models you chose last time (just write the model code
    again and apply it to your training data (from the first
    assignment))
-   calculate performance of the model on the training data: root mean
    square error is a good measure. (Tip: google the function rmse())
-   create the test dataset (apply the code from assignment 1 to clean
    up the 3 test datasets)
-   test the performance of the models on the test data (Tips: google
    the functions “predict()”)
-   optional: predictions are never certain, can you identify the
    uncertainty of the predictions? (e.g. google predictinterval())

For all of us, the performance of the model was better in training data
compared to testing data. This appears through a measure of root mean
squared error, which for our model is 0.36 in the training data, but
increases to 0.66. That is, the error increases as the data changes from
training to test data. This is probably an indicator of the model being
overfitted to the training data.

### Exercise 2) Model Selection via Cross-validation (N.B: ChildMLU!)

One way to reduce bad surprises when testing a model on new data is to
train the model via cross-validation.

In this exercise you have to use cross-validation to calculate the
predictive error of your models and use this predictive error to select
the best possible model.

-   Use cross-validation to compare your model from last week with the
    basic model (Child MLU as a function of Time and Diagnosis, and
    don’t forget the random effects!)
-   (Tips): google the function “createFolds”; loop through each fold,
    train both models on the other folds and test them on the fold)

-   Now try to find the best possible predictive model of ChildMLU, that
    is, the one that produces the best cross-validated results.

-   Bonus Question 1: What is the effect of changing the number of
    folds? Can you plot RMSE as a function of number of folds?
-   Bonus Question 2: compare the cross-validated predictive error
    against the actual predictive error on the test data

``` r
#- Create the basic model of ChildMLU as a function of Time and Diagnosis (don't forget the random effects!).
m0 <- lmer(CHI_MLU ~ Visit + Diagnosis + (1 + Visit|Child.ID), df_train, REML = F)

#- Make a cross-validated version of the model. (Tips: google the function "createFolds";  loop through each fold, train a model on the other folds and test it on the fold)
set.seed(1)
folds <- createFolds(unique(df_train$Child.ID), 5) # Creating the folds

rmse0 <- vector() # Creating an empty vector

# Making a loop
for (f in folds){
  train <- subset(df_train, !(Child.ID %in% f)) # Train data - removing the current fold (the children with IDs, which are in the fold)
  model <- lmer(CHI_MLU ~ Visit + Diagnosis + (1 + Visit|Child.ID), data=train, REML = F, lmerControl(optCtrl=list(xtol_abs=1e-8, ftol_abs=1e-8))) # Defining the model
  actual <- na.omit(subset(df_train, (Child.ID %in% f))) # Test data - only consisting of the current fold (the children with IDs, which are in the fold)
  predictions <- predict(model, actual, allow.new.levels = T) # Predictions for the test-data's child MLU based on the model
  print(ModelMetrics::rmse(actual$CHI_MLU, predictions)) # The root mean squared error - comparing the actual data to the predictions
  rmse0 <- c(rmse0, na.omit(ModelMetrics::rmse(actual$CHI_MLU, predictions))) # Putting the resulting root mean squared errors within a variable
}
```

    ## [1] 0.4586471
    ## [1] 0.7178315
    ## [1] 0.5447356
    ## [1] 0.5786218
    ## [1] 0.3358065

``` r
# Calculating the mean
mean(rmse0)
```

    ## [1] 0.5271285

``` r
#- Report the results and comment on them.
# The root mean squared errors are respectively: 0.46, 0.72, 0.54, 0.58 and 0.34. Resulting in an average rmse of 0.53 morphemes per utterance.

# Checking the standard deviation of the child MLUs
sd(na.omit(df_train$CHI_MLU))
```

    ## [1] 0.932019

``` r
# Standard deviation = 0.93.
# Meaning that the model is a better prediction than the mean of the data, since 0.53 is a smaller error than 0.93.

#- Now try to find the best possible predictive model of ChildMLU, that is, the one that produces the best cross-validated results.
# A two-way interaction effect
rmse1 <- vector()

for (f in folds){
  train <- subset(df_train, !(Child.ID %in% f)) # Train data
  model <- lmer(CHI_MLU ~ Visit * Diagnosis + (1 + Visit|Child.ID), data=train, REML = F, lmerControl(optCtrl=list(xtol_abs=1e-8, ftol_abs=1e-8)))
  actual <- na.omit(subset(df_train, (Child.ID %in% f))) # Test data
  predictions <- predict(model, actual, allow.new.levels=T)
  print(ModelMetrics::rmse(actual$CHI_MLU, predictions))
  rmse1 <- c(rmse1, na.omit(ModelMetrics::rmse(actual$CHI_MLU, predictions)))
}
```

    ## [1] 0.3693076
    ## [1] 0.7162796
    ## [1] 0.5352468
    ## [1] 0.5709479
    ## [1] 0.3068175

``` r
# Calculating the mean
mean(rmse1)
```

    ## [1] 0.4997199

``` r
# Resulting in root mean squared errors of respectively 0.37, 0.82, 0.54, 0.57 and 0.31. The mean of these errors is 0.5. That is, a bit smaller error than for the baseline model, but not much. And it is, therefore still a better model than the mean.

# A three-way interaction effect
rmse2 <- vector()

for (f in folds){
  train <- subset(df_train, !(Child.ID %in% f)) # Train data
  model <- lmer(CHI_MLU ~ Visit * Diagnosis * verbalIQ1+ (1 + Visit|Child.ID), data=train, REML = F, lmerControl(optCtrl=list(xtol_abs=1e-8, ftol_abs=1e-8)))
  actual <- na.omit(subset(df_train, (Child.ID %in% f))) # Test data
  predictions <- predict(model, actual, allow.new.levels=T)
  print(ModelMetrics::rmse(actual$CHI_MLU, predictions))
  rmse2 <- c(rmse2, na.omit(ModelMetrics::rmse(actual$CHI_MLU, predictions)))
}
```

    ## [1] 0.3761739
    ## [1] 0.5960439
    ## [1] 0.3475808
    ## [1] 0.538392
    ## [1] 0.3763854

``` r
# Calculating the mean
mean(rmse2)
```

    ## [1] 0.4469152

``` r
# Resulting in root mean squared errors of respectively 0.38, 0.60, 0.35, 0.54 and 0.38. The mean of these errors is 0.45. That is again, a smaller error than for the baseline model, and also smaller than the model including only a two-way interaction. Therefore, the best model seems to be the model which includes a triple interaction effect between Visit, Diagnosis and verbalIQ1. 
```

### Exercise 3) Assessing the single child

Let’s get to business. This new kiddo - Bernie - has entered your
clinic. This child has to be assessed according to his group’s average
and his expected development.

Bernie is one of the six kids in the test dataset, so make sure to
extract that child alone for the following analysis.

You want to evaluate:

-   how does the child fare in ChildMLU compared to the average TD child
    at each visit? Define the distance in terms of absolute difference
    between this Child and the average TD.

-   how does the child fare compared to the model predictions at Visit
    6? Is the child below or above expectations? (tip: use the predict()
    function on Bernie’s data only and compare the prediction with the
    actual performance of the child)

``` r
set.seed(1)
# Bernie = Child.ID: 2

# Extracting Bernie from the rest of the children
Bernie <-
  df_test %>% 
  filter(Child.ID == 2)

# Checking the mean of the TD's in each visit and the difference between the average and Bernie
TD_Bernie <-
  df_train %>% 
  filter(Diagnosis == "TD") %>% 
  group_by(Visit) %>% 
  summarise(TDmean = mean(CHI_MLU, na.rm = T)) %>% 
  mutate(Bernie$CHI_MLU) %>% 
  mutate(difference = (Bernie$CHI_MLU - TDmean))

mean(TD_Bernie$difference)
```

    ## [1] 0.6253991

``` r
# This shows that Bernie has longer MLU than the average TD child througout all the visits - on average a MLU, which is 0.63 morphemes longer than the average typically developing child.


# Bernie compared to predictions for visit 6 - is he below or above expectations?
# Specifying our best model
m_best <- lmer(CHI_MLU ~ Visit * Diagnosis * verbalIQ1+ (1 + Visit|Child.ID), data = df_train, REML = F, lmerControl(optCtrl=list(xtol_abs=1e-8, ftol_abs=1e-8)))

prediction <- predict(m_best, Bernie) # Predicting Bernie's values based on the best model
prediction
```

    ##        1        2        3        4        5        6 
    ## 2.655982 2.906677 3.157372 3.408066 3.658761 3.909456

``` r
# The prediction is 3.91 in Bernie's MLU at visit 6 

# Bernies actual performance at visit 6
Bernie %>% 
  filter(Visit == 6) %>% 
  select(CHI_MLU)
```

    ##    CHI_MLU
    ## 1 3.448413

``` r
# 3.45

# That is, following the prediction, Bernie's MLU is worse than expected/predicted.
```

### OPTIONAL: Exercise 4) Model Selection via Information Criteria

Another way to reduce the bad surprises when testing a model on new data
is to pay close attention to the relative information criteria between
the models you are comparing. Let’s learn how to do that!

Re-create a selection of possible models explaining ChildMLU (the ones
you tested for exercise 2, but now trained on the full dataset and not
cross-validated).

Then try to find the best possible predictive model of ChildMLU, that
is, the one that produces the lowest information criterion.

-   Bonus question for the optional exercise: are information criteria
    correlated with cross-validated RMSE? That is, if you take AIC for
    Model 1, Model 2 and Model 3, do they co-vary with their
    cross-validated RMSE?

### OPTIONAL: Exercise 5): Using Lasso for model selection

Welcome to the last secret exercise. If you have already solved the
previous exercises, and still there’s not enough for you, you can expand
your expertise by learning about penalizations. Check out this tutorial:
<a href="http://machinelearningmastery.com/penalized-regression-in-r/" class="uri">http://machinelearningmastery.com/penalized-regression-in-r/</a>
and make sure to google what penalization is, with a focus on L1 and
L2-norms. Then try them on your data!
