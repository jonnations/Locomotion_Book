# Locomotion_Book
Repo for Locomotion Book Chapter

Investigation into the morphological signature of arboreality in (small) mammals

#### Goals:  

1) Determine which postcranial measurements, or which form of the measurements, are the best predictors of climbing.
2) Explore the relationship between the method of categorizing locomotion and the preferred morphological measurements. 
3) Predict the locomotion of fossil species using the measurements with the highest predictive ability


Questions to figure out

1- what do i do about mass? I think I will skip the interactions! It may make more sense to 

## Layout:

- The contents of this repo is split up into 3 main directories:`Code`, `Data`, and `Plots`. We use the awesome package "[here](https://here.r-lib.org/)" to deal with the repo structure. Currently anyone can download the repo ***as is*** and run the code without dealing with paths (if all packages are installed).


## **`Code`** 

***Code should be run in the order listed below***

#### 


#### Polychoric PCA and Cluster Analyses  
- We are interested if the multivariate diet matches traditional diet categories from several commonly used classification schemes. To do this, we want to project the importance rankings of the 13 food items into a multivariate diet space, then run a cluster analysis to identify natural groupings in dietspace. This is outlined in the `Code/Ord_Clust_Plot_FIgure_2.Rmd/` script. As the dietary importance rankings are ordinal rankings, and not continuous, we use a method called polychoric PCA, which is designed for ordinal variables. We estimate a polychoric correlation matrix, the project the species into diet space. The process is pretty well annotated in the script. Then we use the package [`mclust`](https://cran.r-project.org/web/packages/mclust/vignettes/mclust.html), which performs cluster analyses using finite normal mixture modeling, to determine the natural clusters. Since there is no really strong preference for a number of clusters, we calculate $k$ = 3, 4, 5, and 6. Then we use the adjusted Rand index to compare these to 4 classification schemes. More details in the text. This generates the Figure 2 plots, which are stored as `Plots/Figure_2_Diet_Clusters.pdf`.

#### Prior Predictive Checks and Model Estimation
- All of the models, prior and posterior checking, predictions, data wrangling, etc. are found in this directory.

- To determine the parameters for each prior distribution on the response variables, we ran prior predictive checks. Similar to the model scripts, these are in the `Code/Prior_Pred_Checks/` directory and labeled `FOODITEM_Prior_Check.Rmd`. The script `Prior_Pred_Checks_All.Rmd` runs all 13 of these scripts, and stores the outputs in the `Code/Prior_Pred_Checks/prior_pred_outputs` directory. `Code/Prior_Preds_Figure_S1.Rmd` generates **Figure_S1_Prior_Pred_Checks.pdf**, stored in the `Plots` dir, shows the prior distributions that we used for each tooth metric for each food item. Note that we used a $N$(0,1) prior on all of our predictor variables, as they are all scaled to a mean of 0 and an sd of 1, and this loosely regularizing prior keeps the models in check but still allows for large effect sizes. 

- All of our ordinal **brms** models use one of three priors on the response variable (the ranks): A Normal distribution, a Student- $T$ distribution, and a  Dirichlet Prior on the threshold (aka cutpoint) values. The Dirichet prior script is found in `Dirichlet_Prior.R`. We used the Stan code described in [this Stan Discourse post](https://discourse.mc-stan.org/t/dirichlet-prior-on-ordinal-regression-cutpoints-in-brms/20640/3), written by [Staffan Betnèr](https://github.com/StaffanBetner), to create this prior. This code was generated from and informed by the case study by [Michael Betancourt](https://betanalpha.github.io/) found [here](https://betanalpha.github.io/assets/case_studies/ordinal_regression.html). We set the mean intercept	$\phi$ to 0 rather than have the model estimate it, which performed better in our simulations. See comment #9 in the discourse post.

- The bulk of our results are from the multilevel models generated in [**brms**](https://github.com/paul-buerkner/brms). The scripts containing the models for each food item are in the `Code/Models/` directory and labeled `Mods_FOODITEM.Rmd`. Each script contains 15 models. To run all of these, visit the script `Code/Mod_All.Rmd`, which wrangles the data and calls each food item script individually. ***This Must Be Run Before the Plotting or Prediction Scripts*** because the brms model objects are necessary for those. The outputs of the models will be stored in the `Code/Models/mod_outputs` directory (see below). These are imported later for predictive and plotting scripts (it's better than running all the models again).

- After running the models, we estimated the LOO model weights of each model using stacking. We first calculated the weights for each model for each metric (3 per metric, each with a different prior on the response, see above). Then we took the models with the highest weight for each metric (n=5) and estimated the model weights for these. This is all in the `Mods_FOODITEM.Rmd` scripts. The model weights are collected iteratively as each food item file runs in `Code/D_All_Models.Rmd`, and the weights are stored in the file `Data/Test_weights_all.csv`. 

- The parameter estimates of all the models with a model weight >0 are found in `Data/Table_S2_Model_Results.csv`, and kept as a supporting file in the manuscript. The script for generating this table is called `Code/Table_S2_Model_Results.Rmd`. There is a lot of data wrangling involved to get the table formatted like it is. 

#### Model Posterior Validation & Accuracy Checks
