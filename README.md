Using the Vu et al. 100 state chromatin model, this R script function will find and output overlap. Calculates as follows -

# Assays
```
 Choose assays_calculating wanted. This function can do 5 things:
 1 "boolean_overlap" ## Calculates the most simply presence or absence of region to state overlap, given as 1 or 0 < br / >
 2 "percent_overlap" ## Calculates the % overlap for each region and state, 0 to 1 (100%), also defines majority state < br / >
 2b "majority_state" Calculate winning max % feature < br / >
 2c "bool_pct_overlap" Calculate boolean of overlap % based on given cutoff parameter. More strict than original boolean overlap < br / >
 3 "boolean_distance" ## Calculates distance from edge of assayed region to edge of each chromatin state  < br / >
       Note, distance measures 3 and 4 are slow, takes 20 minutes for a 200k region file < br / >
 4 "dist_to_feature" , the bp edge to edge data. Requires option 3 calc be ran < br / >
```
# Dependencies
```
library(dplyr)
library(data.table)
library(tidygenomics)  ###Throws warnings on genome_intersect, works as of 2025
library(reshape2)  ## dcast/melt throw depreciated warnings, works as of 2025
```

# Usage
```
source("/location/2025.12_E2G_chromhmm_overlap_function_v0.21.R")

Sample_df <- fread("/location/your_region_file.txt",
                                  sep="\t")

## Must contain these columns:
# c("ElementChr","ElementStart","ElementEnd")


final.table <- CreateStateIntersectFile(Sample_file= Sample_df, ## Your provided region file
                                     ExpName="ExpName",  ##Name the run
                                     working_directory="/projectdir/", ## location needed for state download and output write 
                                     DownloadVuModel=T, ## T or False if you wish to provide your own data/ already have it downloaded
                                     ProvideModel="none", ## or Model_df, Must have columns - chr start stop state
                                     assays_calculating = c("boolean_overlap","percent_overlap","majority_state","bool_pct_overlap","boolean_distance","dist_to_feature"), ##Pick any combination
                                     pct_overlap_cutoff = 0.5, ##Fraction cutoff for "bool_pct_overlap"
                                     distance_wanted = 10000,  ##Basepair cutoff for "boolean_distance"
                                     WriteOutput=T)
```
Defaults for parameters are as shown. If you wish to load a different state model in, provide Model_df to the ProvideModel option with columns : start stop end state
