# Data Prep for A Delta Smelt Individual-Based Life Cycle Model

This repository contains the raw data and code to prep data for use in the Delta Smelt Life Cycle Model. Text is by [Will Smith.](mailto:william_e_smith@fws.gov) 

## Data Overview
Five physical and biological variables, representing observed Delta conditions during 1995-2014, drive simulated population dynamics: prey density, Old and Middle River flow, delta smelt distribution, water temperature, and Secchi depth. All variables except Old and Middle River flow had dimensions year _y_, month _m_, and spatial strata _s_. Old and Middle River flow was a _y_ x _m_ matrix, and prey densities included a fourth dimension _p_ indexing prey type.

**The Spatial Strata are ordered as follows:**

| Strata Number | Region |
| --- | --- |
| 1 | Sacramento R. |
| 2 | South Delta |
| 3 | East Delta |
| 4 | Lower Sacramento R. |
| 5 | Lower San Joaquin R. |
| 6 | Confluence |
| 7 | SE Suisun |
| 8 | NE Suisun |
| 9 | Suisun Marsh |
| 10 | SW Suisun |
| 11 | NWE Suisun |
| 12 | Yolo Bypass |

## Data Files
Filenames for datasets with raw data are listed below. Each dataset, except fish distribution and data are saved at the daily time scale; however the IBMR requires monthly-scaled data. Each dataset must, therefore, be summarized into the spatiotemporal dimensions of the IBMR, using a set of data summary functions that are included with the model code.

| Data type | Description | File name | Comments |
| --- | --- | --- | --- |
| Prey density | log (carbon density) of positive catches | &#39;Zoop.acartela\_\_.txt&#39;  &#39;Zoop.allcopnaup.txt&#39; &#39;Zoop.daphnia\_\_\_.txt&#39; &#39;Zoop.eurytem\_\_\_.txt&#39;  &#39;Zoop.limno\_\_\_\_\_.txt&#39; &#39;Zoop.othcalad\_\_.txt&#39; &#39;Zoop.othcaljuv\_.txt&#39; &#39;Zoop.othclad\_\_\_.txt&#39; &#39;Zoop.othcyc\_\_\_\_.txt&#39; &#39;Zoop.other\_\_\_\_\_.txt&#39; | One file for each prey type |
| Prey density | proportion zero catches | &#39;Zoop.P0.acartela\_\_.txt&#39; &#39;Zoop.P0.allcopnaup.txt&#39; &#39;Zoop.P0.daphnia\_\_\_.txt&#39; &#39;Zoop.P0.eurytem\_\_\_.txt&#39; &#39;Zoop.P0.limno\_\_\_\_\_.txt&#39; &#39;Zoop.P0.othcalad\_\_.txt&#39; &#39;Zoop.P0.othcaljuv\_.txt&#39; &#39;Zoop.P0.othclad\_\_\_.txt&#39; &#39;Zoop.P0.othcyc\_\_\_\_.txt&#39; &#39;Zoop.P0.other\_\_\_\_\_.txt&#39; | One file for each prey type |
| OMR | Old and Middle River flow | &#39;OMR\_daily.txt&#39; | |
| Fish distribution | | &#39;DS\_distribution\_JanDec\_1995\_2015.txt&#39; | |
| Temperature | DSM2 | &#39;temp.txt&#39; | |
| Fish survey | | &#39;delta\_water\_quality\_data\_with\_strata\_v2.csv&#39; | |
| Secchi depth | Fish survey | &#39;delta\_water\_quality\_data\_with\_strata\_v2.csv&#39; | |

### Data Sources and Description
**Prey density.** Prey density estimates _PD_<sub>ymsp</sub> for 12 zooplankton prey types _p_ and years 1989-2015 were developed by Wim Kimmerer (Appendix A) and provided by Derek Hilts (Bay-Delta FWO; 24 September 2019). Daily log prey carbon densities, estimated from positive catches, and proportion zero catch were summarized from the Interagency Ecological Program&#39;s zooplankton survey and the CA Department of Fish and Wildlife (CDFW) 20-mm survey. Monthly means of the daily values were used to simulate prey densities. For more in depth information on prey densities see the [prey data methods.](https://github.com/CSAMP/prey-data) 

**Old and Middle River flow.** _OMR_<sub>yms</sub> was the monthly average of the daily sum of tidally filtered flows from two adjacent channels of the San Joaquin River basin, Old and Middle Rivers. _OMR_ data were available from US Geological Survey (USGS) [streamflow databases](https://waterdata.usgs.gov). Much of the daily streamflow gauge data was missing. If data for one river was missing, daily _OMR_ was predicted from a linear model of the flow in the remaining river, and if data for both rivers was missing, the model proposed by Andrews et al. (2016) was used to estimate missing _OMR_ from San Joaquin River flows and exports from South Delta water diversions. Daily San Joaquin River flow and export data were available from the [Dayflow database](https://data.cnra.ca.gov/dataset/dayflow).

**Fish distribution.** Observed delta smelt distributions _DS_<sub>yms</sub> were developed from 20-mm, Midwater Trawl, and Spring Kodiak Surveys. Townet Survey data were not used, because tow volume data were unavailable before 2003, use of Townet data would only leverage one additional month of distribution information (August), and in most year-months with a concurrent 20-mm Survey sample, delta smelt were observed in fewer spatial strata in the Townet Survey (in 7 of 13 years). Fish survey data were made available online by [CDFW](ftp://ftp.dfg.ca.gov/). Monthly observed catch densities (catch/volume sampled) were assigned to the 12 spatial strata, and observed densities were expanded to total population abundance by multiplying by strata volumes. Strata volumes were estimated by Derek Hilts using DSM2. Estimated population abundances in each spatial stratum were then converted to proportions of the total abundance, which were interpreted as observed occupancy probabilities.

Surveys were not completed in all months, and in some months, no fish were observed (mostly February, March, and August). Distributions from the prior month were assumed when observations were missing. A high level of zero-inflation, resulting in zero observed occupancy of many spatial strata, was noted beginning in spring of 2015; therefore, the terminal cohort of observed spatial distributions was 2014 (last observed in April 2015).

Delta smelt densities in the South Delta stratum were depleted by entrainment, resulting in a negative bias and underestimation of the proportion of the population in this region of the Delta. Observed South Delta densities were therefore divided by estimates of proportional entrainment loss, `PEL_DSLCM.txt` (Smith et al. 2021) to correct for bias.

**Temperature.** Water temperature data _Temp_<sub>yms</sub> for years 1990-2010 were summarized from DSM2 hydrodynamic simulations by Derek Hilts (Fig. 2). The terminal year of the DSM2 _Temp_ dataset, 2010, limited the number of years available for the delta smelt model by at least 4 years. A second set of water temperature data was therefore summarized from all available online data collected by the [CDFW](https://nrm.dfg.ca.gov/) and [FWS](https://www.fws.gov/lodi/) during Delta fish monitoring programs. The second water temperature dataset spanned years 1959-2020, but prior to 2011, data for some year-month-strata combinations were missing or sparse, with only a single sample.

The water temperatures that would have been simulated by DSM2 for missing years, 2011-2014, were predicted using a general linear model of DSM2 monthly means _Temp_<sub>yms</sub> as a function of season, spatial strata, and monthly mean temperatures measured by fish monitoring programs _Temp_<sub>yms</sub>. The best model of _Temp_<sub>yms</sub> was identified using backwards selection, starting with a full model, having an effect for each stratum and season, and eliminating non-significant spatial effects (acceptance level \< 0.05), one coefficient at a time, before eliminating non-significant seasonal effects. The model was fit to data from years 1990-2010, and used to predict _Temp_<sub>yms</sub> from measured _Temp_<sub>yms</sub> for years 2011-2014. See Appendix B for details of the model to predict _Temp_<sub>yms</sub>. 

**Secchi depth.** Secchi depth data _Secchi_<sub>yms</sub> were used as an index of turbidity. Like _Temp_<sub>yms</sub>, Secchi depths for years 1959-2020 were summarized from all CDFW and FWS databases available online (Fig. 2). Means for each year-month-stratum combination were summarized, but data were not available for all strata in all year-months. Missing data were estimated from general linear models of the remaining Secchi data in other spatial strata. The best general linear model for each stratum was selected using backwards selection, beginning with the full model, having a separate coefficient for each spatial stratum, and eliminating non-significant coefficients, one at a time.

For more information on _Secchi_ and _Temperature_ data see the [`delta-secchi-temperature-repository.`](https://github.com/CSAMP/delta-secchi-temperature-data)  

## R Script
`Delta smelt data functions.R` contains the functions required to prep data from the raw format described above for use in the Delta Smelt Life Cycle Model. 

`Delta smelt data functions.R` is broken up into sections. Each section reads in a particular type of data, summarizes into year-month-strata means, and ends with a function to return processed data for IBMR.

The following functions are called on within the Delta Smelt Life Cycle Model: 

1. `make.OMR(first)`, `make.WQ(first)` - Create tables of physical conditions OMR, Temp, and Secchi
2. `smlt.dist(t,yr)` - Calculate DS distribution based on observed distributions
3. `make.food(t,yr)` - Get food variables to make prey density estimates
4. `make.temp(t,yr)` - Create hybrid temp dataset from DSM2 temps and fish survey data
5. `egg2larv(TempCv)` - Calculate egg to larvae survival based on Bennett 2005

`t` = month index(1:12)

`yr` = year index(1=1980; 16=1995)

`first` = first year index for simulation (usually, first=16)

`TempCv = n.strata-length` vector of monthly mean temperature

