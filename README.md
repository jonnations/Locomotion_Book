# Locomotion_Book
Repo for Locomotion Book Chapter

Investigation into the morphological signature of arboreality in (small) mammals

#### Goals:  

1) Determine which postcranial measurements, or which form of the measurements, are the best predictors of climbing.
2) Explore the relationship between the method of categorizing locomotion and the preferred morphological measurements. 
3) Predict the locomotion of fossil species using the measurements with the highest predictive ability

16 October: To Do
Working through the project

[X] Run All Models in their final form
  - `Binary_Mods.Rmd` and `Ordinal_Mods.Rmd`
  
[X] Run Multiple regression prediction Models
  - `Binary_Mult_Mods.Rmd`
  - `Ordinal_Mult_Mods.Rmd`
  
[] Do all predictions on final models
  - add "combined" predictions into reg pred script
  - predict all, including "combined"


## Layout:

- The contents of this repo is split up into 3 main directories:`Code`, `Data`, and `Plots`. We use the awesome package "[here](https://here.r-lib.org/)" to deal with the repo structure. Currently anyone can download the repo ***as is*** and run the code without dealing with paths (if all packages are installed).

## Packages:
- This is the list of packages used throughout the repository. If these are all installed, then everything *should just work* for anyone that downloads this repository. 

- `tidyverse`, `here`, `glue`, `brms`, `cmdstanr`, `kableExtra`,  `ape`, `tidybayes`, `patchwork`, `ggthemes`, `furrr`.

- Note that if you want to use `cmdstanr`, which improves the compilation time and execution time of the `brms` models, then it's a little more involved than just `install.packages()` (but definitely worth it!). See [this link](https://mc-stan.org/cmdstanr/articles/cmdstanr.html) to get started.

- If you want to plot the tree (Figure 1), you need these packages: `ggtree`, `ggtreeExtra`, `tidytree`, `treeio`. If you don't want to plot it, then don't bother. 

## **`Code`** 

***Code should be run in the order listed below***

#### 


#### Organize Data  
- The first step is to take the raw data from our measurements and calculate the indices, geometric means, and log-shape ratios. The raw data are (`Data/Raw_Data_Extant.csv`, and `Data/Raw_Data_Fossil.csv`) and are presented as **Tables S1** and **S6** in the Supporting Information. We also scale the raw data to a mean of zero and a standard deviation of 1, which puts all values on the same scale and helps interpretability, comparability, prior choice, and model convergence. All of this is done in `Code/Data_load.Rmd`. The outputs are called `Data/Extant_Master.csv` and `Data/Fossil_Master.csv`.

#### Run Models

- We use the extant data to build predictive models. The models are found in `Code/Binary_Mods.Rmd` for the binary logistic regression models, and `Code/Ordinal_Mods` for the ranked locomotor varaibles. You can see the model structure, missing data estimation methods, priors, etc. by looking at these scripts. **WARNING**, These models take a long time to run! Hours! These scripts save the model outputs as `B_gm_mods_mis.rds`, `B_gm_mods_mis.rds`, `B_ratio_mods_mis.rds`, `O_gm_mods_mis.rds`, `O_gm_mods_mis.rds`, and `O_ratio_mods_mis.rds`, which are all pretty big files so they are not saved on GitHub. But they are used in all the predictions, so if you are following along you need to run these models and save the outputs before continuing on.

- Miltiple Regression models are run in the scripts `Code/Binary_Mult_Mods.Rmd` and `Code/Ordinal_Mult_Mods.Rmd`. These are models that use all of the "accurate" predictors (see next header) in the model. There are many of them, as the fossil data are missing lots of data, and a different model has to be run for most of them individually based on which predictors are avaialble. The outputs are `Data/B_lm_mods_multi.rds` and `Data/O_lm_mods_multi.rds`

- Model result outputs are presented in **Tables S2** (`Data/Binary_Table.csv`) and **S3** (`Data/Ordinal_Table.csv`). These show the median and 89% probability intervals of each of the relevant parameters. Full model outputs (all ~500 parameters) can be viewed by loading the `Data/B_*_mods_*.rds` model files. The scripts to organize all of the outputs into reasonable format are in the script `Effect_Results_Tables.Rmd`. 

#### Test Prediciton Accuracy

- The scripts `Code/Binary_Prediction_Accuracy.Rmd` and `Code/Ordinal_Prediction_Accuracy.Rmd` generate Pareto-$k$ values using leave-one-out cross validation, and generate predictions for the extant species. These predictions are summarized as percentages of accurate predictions, both with and without the phylogeny as a group level effect. Outputs are `Data/Accuracy.csv` and `Data/Ord_Accuracy.csv` which are presented as **Tables S4** and **S5** in the supporting information.

#### Predictions

- Extant "model" taxa predictions and fossil predictions are made in the same scripts: `Code/Binary_Predict.Rmd` and `Code/Ordianl_Predict.Rmd`. 







- The bulk of our results are from the multilevel models generated in [**brms**](https://github.com/paul-buerkner/brms). The scripts containing the models for each food item are in the `Code/Models/` directory and labeled `Mods_FOODITEM.Rmd`. Each script contains 15 models. To run all of these, visit the script `Code/Mod_All.Rmd`, which wrangles the data and calls each food item script individually. ***This Must Be Run Before the Plotting or Prediction Scripts*** because the brms model objects are necessary for those. The outputs of the models will be stored in the `Code/Models/mod_outputs` directory (see below). These are imported later for predictive and plotting scripts (it's better than running all the models again).

- After running the models, we estimated the LOO model weights of each model using stacking. We first calculated the weights for each model for each metric (3 per metric, each with a different prior on the response, see above). Then we took the models with the highest weight for each metric (n=5) and estimated the model weights for these. This is all in the `Mods_FOODITEM.Rmd` scripts. The model weights are collected iteratively as each food item file runs in `Code/D_All_Models.Rmd`, and the weights are stored in the file `Data/Test_weights_all.csv`. 

- The parameter estimates of all the models with a model weight >0 are found in `Data/Table_S2_Model_Results.csv`, and kept as a supporting file in the manuscript. The script for generating this table is called `Code/Table_S2_Model_Results.Rmd`. There is a lot of data wrangling involved to get the table formatted like it is. 

#### Model Posterior Validation & Accuracy Checks
