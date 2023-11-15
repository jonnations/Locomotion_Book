# Locomotion_Book  

<br>

Repository for the draft of "*Predictors of arboreality from the mammalian appendicular skeleton illuminate locomotor trends among early mammals*" by Jonathan Nations, Lucas Weaver, and David Grossnickle. 

This is an investigation into the morphological signature of climbing in (mostly small, extant) mammals to better understand the locomotor affinities of early mammals and plesiadapiforms. 

#### Goals:  

1) Determine which postcranial measurements, or which form of the measurements, are the best predictors of climbing.
2) Explore the relationship between the method of categorizing locomotion and the preferred morphological measurements. 
3) Predict the locomotion of fossil species using the measurements with the highest predictive ability

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

- We use the extant data to build predictive models. The models are found in `Code/Binary_Mods.Rmd` for the binary logistic regression models, and `Code/Ordinal_Mods` for the ranked locomotor variables. We use multilevel models generated in [**brms**](https://github.com/paul-buerkner/brms). You can see the model structure, missing data estimation methods, priors, etc. by looking at these scripts. **WARNING**, These models take a long time to run! Hours! These scripts save the model outputs as `B_gm_mods_mis.rds`, `B_gm_mods_mis.rds`, `B_ratio_mods_mis.rds`, `O_gm_mods_mis.rds`, `O_gm_mods_mis.rds`, and `O_ratio_mods_mis.rds`, which are all pretty big files so they are not saved on GitHub. But they are used in all the predictions, so if you are following along you need to run these models and save the outputs before continuing on.

- Multiple Regression models are run in the scripts `Code/Binary_Mult_Mods.Rmd` and `Code/Ordinal_Mult_Mods.Rmd`. These are models that use all of the "accurate" predictors (see next header) in the model. There are many of them, as the fossil data are missing lots of data, and a different model has to be run for most of them individually based on which predictors are available. The outputs are `Data/B_lm_mods_multi.rds` and `Data/O_lm_mods_multi.rds`

- Model result outputs are presented in **Tables S2** (`Data/Binary_Table.csv`) and **S3** (`Data/Ordinal_Table.csv`). These show the median and 89% probability intervals of each of the relevant parameters. Full model outputs (all ~500 parameters) can be viewed by loading the `Data/B_*_mods_*.rds` model files. The scripts to organize all of the outputs into reasonable format are in the script `Effect_Results_Tables.Rmd`. 

#### Test Prediciton Accuracy

- The scripts `Code/Binary_Prediction_Accuracy.Rmd` and `Code/Ordinal_Prediction_Accuracy.Rmd` generate Pareto-$k$ values using leave-one-out cross validation, and generate predictions for the extant species. These predictions are summarized as percentages of accurate predictions, both with and without the phylogeny as a group level effect. Outputs are `Data/Accuracy.csv` and `Data/Ord_Accuracy.csv` which are presented as **Tables S4** and **S5** in the supporting information.

#### Predictions & Plots

- Most plots have a .pdf along with a .jpg version for using in google docs. Final versions will be .tiff.

- Extant "model" taxa predictions and fossil predictions are made in the same scripts: `Code/Binary_Predict.Rmd` and `Code/Ordianl_Predict.Rmd`. The scripts generate the predictions, then plot them as seen in **Figures 3**, **4**, **5**, & **6**.


## **`Data`**

- Most of the data are outputs that are called in other scripts, like models being called in prediction scripts. Most of these are discussed above. Here is a list of the relevant **Supporting Information** tables and data:
  - **Table S1**: `Raw_Data_Extant.csv`
  - **Table S2**: `Binary_Table.csv`
  - **Table S3**: `Ordinal_Table.csv`
  - **Table S4**: `Accuracy_Bin.csv`
  - **Table S5**: `Accuracy_Ord.csv` 
  - **Table S6**: `Raw_Data_Fossil.csv`

## **`Plots`**

- The tree plot, **Figure 1**, was made using the `Code/Tree_Plot.Rmd` script. The output is called `Plots/tre_test_rectangle.pdf`.

- The effect output plots are generated in the scripts `Code/Binary_Effect_Plots.Rmd`, `Code/Ordinal_Effect_Plots.Rmd`, and `Code/Combined_Effect_Plots.Rmd`. 
  - **Figures 2**: `Combined_Effects_Some_BW.pdf`
  - **Figures S1**: `B_Effects_All_BW.pdf`
  - **Figures S2**: `O_Effects_All_BW.pdf`

  


