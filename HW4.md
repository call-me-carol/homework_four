HW 4
================

### Q1. (10 points)

Using the derived dataset `Q1_data` extracted from the Kaggle March
Madness competition datasets, fit a linear model to predict the score
differential for a given game based on the teams seeds (and seed
difference). Construct some metric for evaluating this prediction. If
you’d like to consider a distributional prediction:
`scoringRules::crps_sample()` contains a function to do so.

Then implement at least one of the predictive models we have seen in
class: CART, randomForest, Gaussian Processes, regularized regression …
and compare the predictive ability of this model. *Add a short paragraph
summarizing the differences in the models from both a predictive and, if
you can, explanatory perspective*.

``` r
seeds <- read_csv('MNCAATourneySeeds.csv')
results <- read_csv('MNCAATourneyDetailedResults.csv')

Q1_data <- results %>% mutate(score_diff = WScore - LScore) %>%
  select(score_diff, Season, WTeamID, LTeamID) %>%
left_join(seeds, by = c('Season', 'WTeamID' = 'TeamID')) %>% 
  rename(Seed1 = Seed) %>%
left_join(seeds, by = c('Season', 'LTeamID' = 'TeamID')) %>%
  rename(Seed2 = Seed) %>%
  mutate(Seed1 = as.numeric(substr(Seed1, 2,3)), Seed2 = as.numeric(substr(Seed2, 2,3))) %>% rowwise() %>%
  mutate(min_seed = min(Seed1, Seed2), max_seed = max(Seed1, Seed2)) %>% 
  ungroup() %>% mutate(seed_diff = max_seed - min_seed) %>%
  select(score_diff, seed_diff, max_seed, min_seed)
```

Using both `glm` and `stan_glm` with a probit link to fit the data.
Identify the differences in the model output and discuss why they might
differ.

### Q2. (2 points)

Write a short summary of your plan for project 2. Project 2 will focus
on predictive modeling and the only requirement is to compare a linear
model with a more advanced technique. You can work in a group, either
for a NCAA basketball predictive framework or using an alternative
dataset. If you are working in a team, let me know who you will be
working with.
