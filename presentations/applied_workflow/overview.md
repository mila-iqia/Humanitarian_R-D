Applied machine learning workflow.

# Principles
* Iterative improvements
* Reproducibility
* Modularity

## Structure
- data
  - models
  - processed
  - raw
- src
  - eval
  - models
  - preprocess
  - train
- expers
  - experiments with one-off models
  - different exploratory scripts
- cluster
  - singularity definition
  - experiment config
  - cluster sbatch script
  - scripts to launch configurations

# Data Preparation
* This takes `data/raw/` to `data/processed/`
* Preprocessing config 
* Input is a pointer to the raw data + a config file
* Output is a version of the data in the processed file
* Output also adds a line in a metadata preprocessed datasets file, saying
  whether that type of data has been created

## Cleaning
* Coding and rare categories
* Missing values
* Tracking modalities

## Preprocessing
* Normalization
 - Centering / scaling
 - Min/ Max
 - Histogram
* Featurization
 - We might realize that certain transformations of the variables are better.
 - Might also need to put different data modalities on the same sampling
   representation
 - We typically don't have to worry about this in deep learning
 - Canonical example: Looking at the raw values for lat longs in one
   competition, became clear that they were coming from a few geographically
   homogeneous regions. We clustered the regions to give some additional
   assignment.
* Splitting into tiles
* Reading into loaders. Any transformations of raw data needed to put into a
  dataloader. Often don't have matrices -- need to convert image or geo formats,
  for example.
* Trimming extreme values, coding outliers

# Model Definitions
* Function that takes x -> y_hat
* Want to use the same interface across all the models, so that it's easy to
  interchange them
  - In python: sklearn
  - In R: caret
  - For most of our projects: Pytorch conventions

# Metric Definitions
* You will want a uniform way to evaluating all approaches
* Predictions should have the same format
* May find it useful to perform evaluation across several characteristics

# Training and Hyperparameter Search
* For larger datasets, makes sense to have fixed validation and test
* For smaller datasets, can be more useful to apply cross validation
  - Advantage: Lets you estimate the variance of your validation error

# Evaluation
* It's important to be able to compare different models and processing
  strategies with each other, with minimal effort
* The minimal effort part is what makes it more likely that you will iterate on
  strategies more quickly
* This step also facilitates error analysis -- this is how you will be able to
  improve your models down the line

## Quantitative
* Competitions usually give you metrics to target
* Real-world applications require you to think about metrics that might be relevant
  - They will typically involve a combination of metrics, which need to be
    reported to collaborators and assessed in combinations
* Rather than reporting a single summary statistic, it's helpful to look at the
  whole distribution of errors

## Qualitative
* It's often helpful to print out example good and bad predictions
* Different algorithms make different types of mistakes, and it's easier to
  notice that when you can see lots of example predictions
