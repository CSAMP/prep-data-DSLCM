# A Delta Smelt Individual-Based Life Cycle Model in the R Statistical Environment

# Data files

Filenames for datasets with raw data are listed below. Each dataset, except fish distribution and data are saved at the daily time scale; however the IBMR requires monthly-scaled data. Each dataset must, therefore, be summarized into the spatiotemporal dimensions of the IBMR, using a set of data summary functions that are included with the model code.

| Data type | Description | File name | Comments |
| --- | --- | --- | --- |
| Prey density | log(carbon density) of positive catches | &#39;Zoop.acartela\_\_.txt&#39; &#39;Zoop.allcopnaup.txt&#39;  &#39;Zoop.daphnia\_\_\_.txt&#39; &#39;Zoop.eurytem\_\_\_.txt&#39; &#39;Zoop.limno\_\_\_\_\_.txt&#39; &#39;Zoop.othcalad\_\_.txt&#39;  &#39;Zoop.othcaljuv\_.txt&#39; &#39;Zoop.othclad\_\_\_.txt&#39; &#39;Zoop.othcyc\_\_\_\_.txt&#39; &#39;Zoop.other\_\_\_\_\_.txt&#39;  | One file for each prey type |
| Prey density | proportion zero catches | &#39;Zoop.P0.acartela\_\_.txt&#39; &#39;Zoop.P0.allcopnaup.txt&#39; &#39;Zoop.P0.daphnia\_\_\_.txt&#39; &#39;Zoop.P0.eurytem\_\_\_.txt&#39; &#39;Zoop.P0.limno\_\_\_\_\_.txt&#39; &#39;Zoop.P0.othcalad\_\_.txt&#39; &#39;Zoop.P0.othcaljuv\_.txt&#39; &#39;Zoop.P0.othclad\_\_\_.txt&#39; &#39;Zoop.P0.othcyc\_\_\_\_.txt&#39; &#39;Zoop.P0.other\_\_\_\_\_.txt&#39; | One file for each prey type |
| OMR | Old and Middle River flow | &#39;OMR\_daily.txt&#39; | |
| Fish distribution | | &#39;DS\_distribution\_JanDec\_1995\_2015.txt&#39; | |
| Temperature | DSM2 | &#39;temp.txt&#39; | |
| Fish survey | | &#39;delta\_water\_quality\_data\_with\_strata\_v2.csv&#39; | |
| Secchi depth | Fish survey | &#39;delta\_water\_quality\_data\_with\_strata\_v2.csv&#39; | |

## Data description 

Five physical and biological variables, representing observed Delta conditions during 1995-2014, drive simulated population dynamics: prey density, Old and Middle River flow, delta smelt distribution, water temperature, and Secchi depth. All variables except Old and Middle River flow had dimensions year _y_, month _m_, and spatial strata _s_. Old and Middle River flow was a _y_ x _m_ matrix, and prey densities included a fourth dimension _p_ indexing prey type.

**Prey density.** Prey density estimates for 12 zooplankton prey types _p_ and years 1989-2015 were developed by Wim Kimmerer (Appendix A) and provided by Derek Hilts (Bay-Delta FWO; 24 September 2019). Daily log prey carbon densities, estimated from positive catches, and proportion zero catch were summarized from the Interagency Ecological Program&#39;s zooplankton survey and the CA Department of Fish and Wildlife (CDFW) 20-mm survey. Monthly means of the daily values were used to simulate prey densities. For more in depth information on prey densities see the [prey data methods.](https://github.com/CSAMP/prey-data) 

**Old and Middle River flow.** was the monthly average of the daily sum of tidally filtered flows from two adjacent channels of the San Joaquin River basin, Old and Middle Rivers. _OMR_ data were available from US Geological Survey (USGS) [streamflow databases](https://waterdata.usgs.gov). Much of the daily streamflow gauge data was missing. If data for one river was missing, daily _OMR_ was predicted from a linear model of the flow in the remaining river, and if data for both rivers was missing, the model proposed by Andrews et al. (2016) was used to estimate missing _OMR_ from San Joaquin River flows and exports from South Delta water diversions. Daily San Joaquin River flow and export data were available from the [Dayflow database](https://data.cnra.ca.gov/dataset/dayflow).

**Fish distribution.** Observed delta smelt distributions were developed from 20-mm, Midwater Trawl, and Spring Kodiak Surveys. Townet Survey data were not used, because tow volume data were unavailable before 2003, use of Townet data would only leverage one additional month of distribution information (August), and in most year-months with a concurrent 20-mm Survey sample, delta smelt were observed in fewer spatial strata in the Townet Survey (in 7 of 13 years). Fish survey data were made available online by [CDFW](ftp://ftp.dfg.ca.gov/). Monthly observed catch densities (catch/volume sampled) were assigned to the 12 spatial strata, and observed densities were expanded to total population abundance by multiplying by strata volumes. Strata volumes were estimated by Derek Hilts using DSM2. Estimated population abundances in each spatial stratum were then converted to proportions of the total abundance, which were interpreted as observed occupancy probabilities.

Surveys were not completed in all months, and in some months, no fish were observed (mostly February, March, and August). Distributions from the prior month were assumed when observations were missing. A high level of zero-inflation, resulting in zero observed occupancy of many spatial strata, was noted beginning in spring of 2015; therefore, the terminal cohort of observed spatial distributions was 2014 (last observed in April 2015).

Delta smelt densities in the South Delta stratum were depleted by entrainment, resulting in a negative bias and underestimation of the proportion of the population in this region of the Delta. Observed South Delta densities were therefore divided by estimates of proportional entrainment loss (Smith et al. 2021) to correct for bias.

**Temperature.** Water temperature data for years 1990-2010 were summarized from DSM2 hydrodynamic simulations by Derek Hilts (Fig. 2). The terminal year of the DSM2 _Temp_ dataset, 2010, limited the number of years available for the delta smelt model by at least 4 years. A second set of water temperature data was therefore summarized from all available online data collected by the CDFW and FWS during [Delta fish monitoring programs](ftp://ftp.dfg.ca.gov/; https://www.fws.gov/lodi/). The second water temperature dataset spanned years 1959-2020, but prior to 2011, data for some year-month-strata combinations were missing or sparse, with only a single sample.

The water temperatures that would have been simulated by DSM2 for missing years, 2011-2014, were predicted using a general linear model of DSM2 monthly means as a function of season, spatial strata, and monthly mean temperatures measured by fish monitoring programs . The best model of was identified using backwards selection, starting with a full model, having an effect for each stratum and season, and eliminating non-significant spatial effects (acceptance level \&lt; 0.05), one coefficient at a time, before eliminating non-significant seasonal effects. The model was fit to data from years 1990-2010, and used to predict from measured for years 2011-2014. See Appendix B for details of the model to predict .

**Secchi depth.** Secchi depth data were used as an index of turbidity. Like , Secchi depths for years 1959-2020 were summarized from all CDFW and FWS databases available online (Fig. 2). Means for each year-month-stratum combination were summarized, but data were not available for all strata in all year-months. Missing data were estimated from general linear models of the remaining Secchi data in other spatial strata. The best general linear model for each stratum was selected using backwards selection, beginning with the full model, having a separate coefficient for each spatial stratum, and eliminating non-significant coefficients, one at a time.

**Missing temperature and Secchi depth data**
TODO: Fix equation fomrating 

General linear models were developed to predicting missing temperature and turbidity data. The objective of these models was prediction, not inference; therefore, covariate effects were not selected based any particular mechanistic link (e.g., spatial proximity), and the relative effects within each model were not compared. Only explanatory power and model performance was considered.

_Temperature._ DSM2 monthly mean for years _y_, 2011-2014, months _m_, and spatial strata _s_ were predicted as a function of season, spatial strata, and monthly mean temperatures measured by fish monitoring programs , using a general linear model. from DSM2 represented a water column mean, while from monitoring programs represented surface measurements. The relationship between and was expected to vary seasonally, with the onset of thermal stratification as water warmed, and the effect of thermal stratification was expected to vary spatially, as water depth, tidal influence, and stratification vary spatially. Factorial seasonal and strata effects accounted for this spatiotemporal variation in the model to predict missing (1) represented coefficients of the general linear model, the quantity represented a unique intercept for each _s_, and scaled fish monitoring temperatures to DSM2 temperatures. Beginning with a full model, having four seasonal effects , backwards selection was used to combine seasons until _n.season_ groupings remained, with coefficient p-values \&lt; 0.05. After selecting seasonal effects, the same process was used to eliminate strata-specific effects. Beginning with a full model, having 12 strata-specific effects , backwards selection was used to combine strata until groupings remained, with coefficient p-values \&lt; 0.05. _Secchi depth._ Missing Secchi depth measurements were also predicted from a general linear model. Since turbidity stratification was not expected to occur and no secondary measurements of were available from independent sources, missing were predicted using the remaining measurements in other strata. (2) for _i_ in the set [Yolo, East Delta, Northeast Suisun]. Backwards model selection was used to eliminate strata-specific effects until strata effects remained, with coefficient p-values \&lt; 0.05.

Models (1) and (2) were fit using the glm() function in R (R 2018), and the best models indicated by model selection were evaluated graphically using general diagnostic plots, residual, q-q norm, standardized residual, and influence, for violations of basic linear model assumptions. Residuals were expected to be randomly distributed around zero. qq-norm plots were expected to be relatively linear, and the slope of standardized residuals was expected to be zero.

**Results**

The best model of included separate spring and summer effects, but winter and fall were grouped (Table 1). Most strata were grouped, but South Delta, Confluence, and Southwest Suisun strata each received unique coefficients, resulting in unique intercepts for these three strata. Diagnostics did not indicate severe violations of general linear model assumptions. Model residuals appeared to be normally distributed, and no extreme outliers or leverage points were detected (Fig. 1). The model explained 95.8% of the variation in the data, indicating high explanatory power.

All missing data were from years prior to 2011. Of 240 year-month combinations, 30 were missing, 7 were missing, and 1 were missing. The month of August was especially problematic for , with zero samples from 1998 to 2010.

The best model to fill in missing varied by stratum (Table 2). The best model to predict included Sacramento, South Delta, Lower Sacramento, and Lower San Joaquin effects. Some were missing in the same time periods as the missing Yolo data, so East Delta data were not used to predict Yolo Secchi depth. The best model to predict included Sacramento, South Delta, Southeast Suisun, and Southwest Suisun effects, and the best model to predict included Sacramento, Southeast Suisun, Northeast Suisun, and Northwest Suisun effects. Diagnostics revealed exactly one extreme outlier in the datasets to predict for each stratum, so these outliers were removed prior to fitting the final model. After removing outliers, most residuals appeared to be normally distributed (Figs. 2-4), and no high leverage points were detected. Residuals were somewhat higher at the highest predicted values of greater than 175 cm Secchi depth, suggesting that a log-linear model might fit the data better. Modeled effects on delta smelt did not vary at Secchi depths greater than 84 cm, so an improved model of the highest Secchi depths was not expected to change the IBMR dynamics.

The explanatory power of models to predict varied, with East Delta and Northeast Suisun models explaining 79.5% and 82.7% of variation, respectively. The best model to predict missing explained only 61.6% of variation. Many of the Yolo observations, or y values, were limited to a single replicate sample per month, versus and average of 30 Secchi depth samples within other year-month-strata combinations. This limitation may have been a source of observation error in the Yolo model.

**Discussion**

The models developed here to predict missing and leave much room for improvement.
 -The best way to improve the estimation of data used by IBMR would be to complete DSM2 simulations for the entire 1995-2014 time series.

-Backwards selection using coefficient p-values was used to identify the best and general linear models, but a comparison of all possible models, using AIC would be a superior method. The primary advantage would be increasing the number of strata groupings to consider. The set of all possible strata combinations is very large. For example, just consideration of all 1- and 2-strata groupings would result in a total of 78 candidate models to consider.

-Replacing the linear models with a more flexible functional form, such as a generalized additive model (GAM), could improve predictions by accounting for non-linear relationships.

-Some hydrodynamic models of the San Francisco Estuary are capable of simulating turbidity fields. Application of these models to estimate the IBMR dataset would be especially useful for the Yolo spatial strata, which was limited by a low number of within-month (replicate) samples and a larger number of unsampled year-month combinations, relative to other strata.





