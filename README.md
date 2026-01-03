Using the Vu et al. 100 state chromatin model, this R script function will find and output overlap. Calculates as follows -
# ### Choose assays_calculating wanted. This function can do 5 things:
# 1 "boolean_overlap" ## Calculates the most simply presence or absence of region to state overlap, given as 1 or 0
# 2 "percent_overlap" ## Calculates the % overlap for each region and state, 0 to 1 (100%), also defines majority state
# 2b "majority_state" Calculate winning max % feature
# 2c "bool_pct_overlap" Calculate boolean of overlap % based on given cutoff parameter. More strict than original boolean overlap
# 3 "boolean_distance" ## Calculates distance from edge of assayed region to edge of each chromatin state 
# Note, distance measures 3 and 4 are slow, takes 20 minutes for a 200k region file
# 4 "dist_to_feature" , the bp edge to edge data. Requires option 3 calc be ran

## Dependencies

library(dplyr)
library(data.table)
library(tidygenomics)  ###Throws warnings on genome_intersect, works as of 2025
library(reshape2)  ## dcast/melt throw depreciated warnings, works as of 2025


Usage

source("/location/2025.12_E2G_chromhmm_overlap_function_v0.21.R")

Sample_df <- fread("/location/your_region_file.txt",
                                  sep="\t")

## Must contain these columns:
# c("ElementChr","ElementStart","ElementEnd")


final.table <- CreateStateIntersectFile(Sample_file= Sample_df,
                                     ExpName="YS3_ATAC_testrun_modelDL",
                                     working_directory="/u/project/kp1/jlangerm/Projects/IGVF/R_Analysis/Analysis/2025-12-23_E2G_ChromHMM_Boolean_Features/",
                                     DownloadVuModel=T,
                                     ProvideModel="none", ## or Model_df, Must have columns - chr start stop state
                                     assays_calculating = c("boolean_overlap","percent_overlap","majority_state","bool_pct_overlap","boolean_distance","dist_to_feature"),
                                     pct_overlap_cutoff = 0.5, ##Fraction cutoff for "bool_pct_overlap"
                                     distance_wanted = 10000,
                                     WriteOutput=T)
