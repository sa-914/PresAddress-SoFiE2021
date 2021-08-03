Code for the paper "**Realized semi(co)variation: Signs that good and bad volatility are not created equal**" by Tim Bollerslev - (Forthcoming) Presidential address presented at the Fourteenth Annual Society for Financial Econometrics Conference, University of California, San Diego, June 2021. 

# Code Description

**Data collection** is done using the scripts [taq_scrape.sas](code/data_collection/taq_scrape.sas) and [taqmsec_scrape.sas](code/data_collection/taqmsec_scrape.sas). These scripts can be modified to collect clean five-minute prices for any set of stocks (by permno, cusip, or symbol) for any particular day. For data from 2014 or earlier, use [taq_scrape.sas](code/data_collection/taq_scrape.sas). These scripts come with some placeholder arguments, but can be modified programmatically as in [get_taq_data_simple.ipynb](code/data_collection/get_taq_data_simple.ipynb); this notebook references metadata for CRSP that is too large to be included in this repository. However, its fairly easy to input your own list of tickers/permnos/cusips. The notebook generates SAS scripts that collect data for the given stocks for each day in the given dates. These scripts can be run on WRDS using autogenerated shell scripts; the command to run all the scripts in parallel is also generated by the notebook. The scripts output csv files which go through some minor cleaning/reformatting before being saved as parquet files. These processed files should be saved locally in a new folder called data/taq/prices. Additionally, CRSP data is also collected for adjusted overnight returns. This is obtained using the [get_crsp_daily.ipynb](code/data_collection/get_crsp_daily.ipynb) notebook; this notebook also preprocesses the raw SAS output. 

**Data cleaning** is done by merging the TAQ data with CRSP data. The majority of the cleaning is done by [clean_prices.ipynb](code/data_cleaning/clean_prices.ipynb). The output is saved to data/proc/clean_prices. 

**Data Analysis** is done in [three notebooks](code/analysis). The [main notebook](code/analysis/main.ipynb) loads all the data, does some additional data processing and computes volatility measures. Most of the processed dataframes are saved to the [data](data) folder; the dataframe containing high-freq prices for all tickers is not included in the repo due to its size. This notebook concludes with the SHAR regressions. The return regressions are done in [spy_return_regs.ipynb](code/analysis/spy_return_regs.ipynb) while the semicovariance/semibeta results are computed in [semitables.ipynb](code/analysis/semitables.ipynb). All figures/tables generated by the notebooks are saved to the exhibits folder.