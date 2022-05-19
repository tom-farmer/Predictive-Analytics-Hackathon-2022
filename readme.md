# 2022 Predictive Analytics Hackathon
[2022 Predictive Analytics Hack-A-Thon](https://www.kaggle.com/competitions/2022-pa-hackathon-adv-competition/data)

Author: Tom Farmer

## File Structure
I wrote all my code in the `scratch_work.ipynb` notebook.  I pulled out the code to produce my final model and put it in the `final_model.ipynb` notebook.  

Data is stored in data/ and the zip file is in the root directory but both
are ignored by git.  Submission file is a .csv so also ignored by git.

## Modeling Approach

### Data Pre-processing

I took a simple approach of producing many variables from the input data without much thought put into crafting custom features.  Many of the variables with multiple comma separated values were one-hot encoded into binary features for the common values.

For many of the numeric variables with missing values I used KNN Imputation with n=10 which seemed to improve model results noticeably beyond other impute strategies like mean or median.

Given the importance of list price to the model, I replaced outlier values for that variable with 0s which considerably improved model performance on the test data.  I also added variables that were ratios of the prices to one another between list price, tax assessment, and last sold price.

After data pre-processing there were over 800 variables to choose from.  Due to time constraints I wasn't able to use some of the more complex variables like Summary.

### Model and Variable Selection
I used a Gradient Boosting model and spent quite a bit of time experimenting with the hyperparameters to determine the best model.  Some of the learnings I had were:
 - More parsimonious models with fewer trees, fewer variables, smaller max depth, etc. tended to perform better on the leaderboards.
 - I tried using Bayesian hyperparameter optimization but didn't find it improved my performance over manual hyperparameter selection.  
 - For a given max depth, learning rate, and set of variables I used a graph of error on the test dataset to select the number of trees to use.
 - Random variable selection in the trees noticeably worsened model performance.  I suspect because the list price variable was so key to the model that it wasn't able to have as big of an impact

## Key Commands:
 - `kaggle competitions download -c 2022-pa-hackathon-adv-competition`
 - `kaggle competitions submit 2022-pa-hackathon-adv-competition -f
data/sample_submission.csv -m "test submission"`

## Environment
This work uses Conda to manage package dependencies.  See environment.yml file.
