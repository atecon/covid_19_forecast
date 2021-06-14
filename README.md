# Time-series based 7-days ahead forecasts of (known) confirmed cases of the novel Corona virus COVID-19 (2019-nCoV)

**Archived since 2021-01-12**: This repo is archived but still can be forked.

**Statement**: This is just "hobby" project and forecast results *should not* be taken too seriously! Even though forecast performance may be reasonable for some country and some period in time. Don't take the numbers forecast for granted!
This project is mainly supposed to show what may be realized (without too much effort) with the open-source statistics and econometrics software gretl (URL: http://gretl.sourceforge.net/).

**Minimum gretl version**: At least gretl 2020b.

**An automated job** retrives latest data at 3am (CET), trains a new model, computes the forecasts and uploads the new forecasting plots here to my github-repo. The overall job finishes in about 15 seconds on a Raspberry Pi 4 computer --- this is just an amazing piece of hardware ;-)

## Data source
Data provided by the Johns Hopkins University Center for Systems Science and Engineering (JHU CCSE) can be found here:
https://github.com/CSSEGISandData/COVID-19

## Some words on the underlying model
I want to keep it simple. If you fancy, you can use the current state of code as a starting point for devloping and evaluating more complex approaches. However, the ARIMA type of model applied may already provide a reasonable approach for modelling and computing short-term contagion dynamics.

I recently (2020-07-26) switched from simple pre-defined ARIMA models to my official *auto_arima* package for gretl automatcally searching for the "best" model. Details on the auto_arima package can be found here:
https://github.com/atecon/auto_arima

The code executes a brute-force search for the 'best' ARIMA model specification by optimizing the corrected Akaike information criteria ("aicc"). The following parameter space -- **implying 120 different ARIMA models in total** -- is evaluated:

**Default ARIMA parameter space**:

```
string ARIMA_OPTS.INFO_CRIT = "aicc"		# information criteria to optimize
scalar ARIMA_OPTS.min_p = 0					# autoregressive (AR) order
scalar ARIMA_OPTS.max_p = 4                 # autoregressive (AR) order
scalar ARIMA_OPTS.min_d = 0                 # differencing order
scalar ARIMA_OPTS.max_d = 2                 # differencing order
scalar ARIMA_OPTS.min_q = 0                 # moving average (MA) order
scalar ARIMA_OPTS.max_q = 1                 # moving average (MA) order

scalar ARIMA_OPTS.min_P = 0                 # seasonal autoregressive (AR) order
scalar ARIMA_OPTS.max_P = 1                 # seasonal autoregressive (AR) order
scalar ARIMA_OPTS.min_D = 0                 # seasonal differencing order
scalar ARIMA_OPTS.max_D = 0                 # seasonal differencing order
scalar ARIMA_OPTS.min_Q = 0                 # seasonal MA order
scalar ARIMA_OPTS.max_Q = 1                 # seasonal MA order
```

## Forecasting method
We compute out-of-sample multi-period interval forecasts. The multi-period forecast is recursively computed. Per default the 90 % forecast (Gaussian) interval will be shown as well.

## The gretl script
The gretl script for setting up relevant things and executing the analysis is ```./script/run.inp```.

At the beginning of the script, the user can specify the following parameters:
```
string DIR_WORK = "" 		# <SET_PATH_HERE> e.g. "/home/git_project"
string DATA_URL = "https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_19-covid-Confirmed.csv"	# Data source
string INITIAL_DATE = "2020-01-22"			# 1st available observation of CSSE dataset
scalar MINIMUM_CASES = 30					# Minimal number of confirmed cases at latest observation for consideration
string SAVE_PLOT_AS = "png"					# grpahic format: "png", "pdf"", eps"

# ARIMA model settings -- see auto_arima package: http://ricardo.ecn.wfu.edu/gretl/cgi-bin/current_fnfiles/auto_arima.gfn
bundle ARIMA_OPTS = null
scalar ARIMA_OPTS.MAX_HORIZON = 7           # max. multi-step OoS forecast horizon
# optimize by information criteria, either aic, aicc, bic or hqc
string ARIMA_OPTS.INFO_CRIT = "aicc"
scalar ARIMA_OPTS.min_p = 0					# autoregressive (AR) order
scalar ARIMA_OPTS.max_p = 4                 # autoregressive (AR) order
scalar ARIMA_OPTS.min_d = 0                 # differencing order
scalar ARIMA_OPTS.max_d = 2                 # differencing order
scalar ARIMA_OPTS.min_q = 0                 # moving average (MA) order
scalar ARIMA_OPTS.max_q = 1                 # moving average (MA) order

scalar ARIMA_OPTS.min_P = 0                 # seasonal autoregressive (AR) order
scalar ARIMA_OPTS.max_P = 1                 # seasonal autoregressive (AR) order
scalar ARIMA_OPTS.min_D = 0                 # seasonal differencing order
scalar ARIMA_OPTS.max_D = 0                 # seasonal differencing order
scalar ARIMA_OPTS.min_Q = 0                 # seasonal MA order
scalar ARIMA_OPTS.max_Q = 1                 # seasonal MA order
```

This script will also load the functions doing the main stuff in the beckground which are stored in ```./src/helper.inp``` as well as the 3rd party library ```auto_arima```.

The gretl script can be executed in the following ways:
Option A:

	1) Clone the repo by means of ```git clone```
	2) open the script "./script/run.inp"
	3) Set your project path by setting the variable "DIR_WORK" accordingly.
	4) Execute and enjoy.

Option B (works for linux):

	1) Clone the repo by means of ```git clone```
	2) Execute the shell-script run.sh

## What does the script do?
The script downloads latest available CCSE-data, processes the raw data for obtaining a clean panel data set.
Next, for each country-province combination two exercises are conducted:

	1) If the parameter ```RUN_EXPOST_ANALYSIS``` is set to '1' , an **ex-post** forecasting analysis is done. For this,  the training-set is set to <CURRENT_DATE - MAX_HORIZON> observations where "CURRENT_DATE" refers to latest date for which data is available, and ```MAX_HORIZON``` is the set multi-step forecast horizon (default 7 days).
	2) Out-of-sample interval forecasts for the forthcoming ```MAX_HORIZON``` days are computed.

# Ex-Post and up-to-date out-of-sample 7-days ahead forecasts
The left panel shows forecast made in information available 7 days ago and the realization of 'confirmed cases' during this period. This may give you an idea of how 'well' the forecasted dynamics were.

The middle panel shows forecasts made on latest data for the forthcoming 7 days. The left panel depicts both the historic and predicted day-to-day changes of confirmed cases.

<img src="./figures/forecast_maxhorizon_7_afghanistan__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_afghanistan_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_afghanistan__first_difference.png" width="260"/>
<em>afghanistan - </em>

<img src="./figures/forecast_maxhorizon_7_albania__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_albania_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_albania__first_difference.png" width="260"/>
<em>albania - </em>

<img src="./figures/forecast_maxhorizon_7_algeria__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_algeria_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_algeria__first_difference.png" width="260"/>
<em>algeria - </em>

<img src="./figures/forecast_maxhorizon_7_andorra__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_andorra_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_andorra__first_difference.png" width="260"/>
<em>andorra - </em>

<img src="./figures/forecast_maxhorizon_7_angola__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_angola_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_angola__first_difference.png" width="260"/>
<em>angola - </em>

<img src="./figures/forecast_maxhorizon_7_antigua_and_barbuda__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_antigua_and_barbuda_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_antigua_and_barbuda__first_difference.png" width="260"/>
<em>antigua_and_barbuda - </em>

<img src="./figures/forecast_maxhorizon_7_argentina__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_argentina_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_argentina__first_difference.png" width="260"/>
<em>argentina - </em>

<img src="./figures/forecast_maxhorizon_7_armenia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_armenia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_armenia__first_difference.png" width="260"/>
<em>armenia - </em>

<img src="./figures/forecast_maxhorizon_7_australia_australian_capital_territory_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_australia_australian_capital_territory.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_australia_australian_capital_territory_first_difference.png" width="260"/>
<em>australia - australian_capital_territory</em>

<img src="./figures/forecast_maxhorizon_7_australia_new_south_wales_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_australia_new_south_wales.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_australia_new_south_wales_first_difference.png" width="260"/>
<em>australia - new_south_wales</em>

<img src="./figures/forecast_maxhorizon_7_australia_northern_territory_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_australia_northern_territory.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_australia_northern_territory_first_difference.png" width="260"/>
<em>australia - northern_territory</em>

<img src="./figures/forecast_maxhorizon_7_australia_queensland_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_australia_queensland.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_australia_queensland_first_difference.png" width="260"/>
<em>australia - queensland</em>

<img src="./figures/forecast_maxhorizon_7_australia_south_australia_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_australia_south_australia.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_australia_south_australia_first_difference.png" width="260"/>
<em>australia - south_australia</em>

<img src="./figures/forecast_maxhorizon_7_australia_tasmania_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_australia_tasmania.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_australia_tasmania_first_difference.png" width="260"/>
<em>australia - tasmania</em>

<img src="./figures/forecast_maxhorizon_7_australia_victoria_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_australia_victoria.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_australia_victoria_first_difference.png" width="260"/>
<em>australia - victoria</em>

<img src="./figures/forecast_maxhorizon_7_australia_western_australia_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_australia_western_australia.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_australia_western_australia_first_difference.png" width="260"/>
<em>australia - western_australia</em>

<img src="./figures/forecast_maxhorizon_7_austria__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_austria_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_austria__first_difference.png" width="260"/>
<em>austria - </em>

<img src="./figures/forecast_maxhorizon_7_azerbaijan__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_azerbaijan_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_azerbaijan__first_difference.png" width="260"/>
<em>azerbaijan - </em>

<img src="./figures/forecast_maxhorizon_7_bahamas__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_bahamas_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_bahamas__first_difference.png" width="260"/>
<em>bahamas - </em>

<img src="./figures/forecast_maxhorizon_7_bahrain__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_bahrain_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_bahrain__first_difference.png" width="260"/>
<em>bahrain - </em>

<img src="./figures/forecast_maxhorizon_7_bangladesh__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_bangladesh_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_bangladesh__first_difference.png" width="260"/>
<em>bangladesh - </em>

<img src="./figures/forecast_maxhorizon_7_barbados__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_barbados_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_barbados__first_difference.png" width="260"/>
<em>barbados - </em>

<img src="./figures/forecast_maxhorizon_7_belarus__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_belarus_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_belarus__first_difference.png" width="260"/>
<em>belarus - </em>

<img src="./figures/forecast_maxhorizon_7_belgium__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_belgium_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_belgium__first_difference.png" width="260"/>
<em>belgium - </em>

<img src="./figures/forecast_maxhorizon_7_belize__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_belize_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_belize__first_difference.png" width="260"/>
<em>belize - </em>

<img src="./figures/forecast_maxhorizon_7_benin__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_benin_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_benin__first_difference.png" width="260"/>
<em>benin - </em>

<img src="./figures/forecast_maxhorizon_7_bhutan__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_bhutan_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_bhutan__first_difference.png" width="260"/>
<em>bhutan - </em>

<img src="./figures/forecast_maxhorizon_7_bolivia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_bolivia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_bolivia__first_difference.png" width="260"/>
<em>bolivia - </em>

<img src="./figures/forecast_maxhorizon_7_bosnia_and_herzegovina__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_bosnia_and_herzegovina_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_bosnia_and_herzegovina__first_difference.png" width="260"/>
<em>bosnia_and_herzegovina - </em>

<img src="./figures/forecast_maxhorizon_7_botswana__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_botswana_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_botswana__first_difference.png" width="260"/>
<em>botswana - </em>

<img src="./figures/forecast_maxhorizon_7_brazil__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_brazil_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_brazil__first_difference.png" width="260"/>
<em>brazil - </em>

<img src="./figures/forecast_maxhorizon_7_brunei__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_brunei_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_brunei__first_difference.png" width="260"/>
<em>brunei - </em>

<img src="./figures/forecast_maxhorizon_7_bulgaria__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_bulgaria_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_bulgaria__first_difference.png" width="260"/>
<em>bulgaria - </em>

<img src="./figures/forecast_maxhorizon_7_burkina_faso__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_burkina_faso_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_burkina_faso__first_difference.png" width="260"/>
<em>burkina_faso - </em>

<img src="./figures/forecast_maxhorizon_7_burma__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_burma_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_burma__first_difference.png" width="260"/>
<em>burma - </em>

<img src="./figures/forecast_maxhorizon_7_burundi__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_burundi_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_burundi__first_difference.png" width="260"/>
<em>burundi - </em>

<img src="./figures/forecast_maxhorizon_7_cabo_verde__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_cabo_verde_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_cabo_verde__first_difference.png" width="260"/>
<em>cabo_verde - </em>

<img src="./figures/forecast_maxhorizon_7_cambodia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_cambodia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_cambodia__first_difference.png" width="260"/>
<em>cambodia - </em>

<img src="./figures/forecast_maxhorizon_7_cameroon__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_cameroon_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_cameroon__first_difference.png" width="260"/>
<em>cameroon - </em>

<img src="./figures/forecast_maxhorizon_7_canada_alberta_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_canada_alberta.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_canada_alberta_first_difference.png" width="260"/>
<em>canada - alberta</em>

<img src="./figures/forecast_maxhorizon_7_canada_british_columbia_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_canada_british_columbia.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_canada_british_columbia_first_difference.png" width="260"/>
<em>canada - british_columbia</em>

<img src="./figures/forecast_maxhorizon_7_canada_manitoba_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_canada_manitoba.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_canada_manitoba_first_difference.png" width="260"/>
<em>canada - manitoba</em>

<img src="./figures/forecast_maxhorizon_7_canada_new_brunswick_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_canada_new_brunswick.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_canada_new_brunswick_first_difference.png" width="260"/>
<em>canada - new_brunswick</em>

<img src="./figures/forecast_maxhorizon_7_canada_newfoundland_and_labrador_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_canada_newfoundland_and_labrador.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_canada_newfoundland_and_labrador_first_difference.png" width="260"/>
<em>canada - newfoundland_and_labrador</em>

<img src="./figures/forecast_maxhorizon_7_canada_nova_scotia_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_canada_nova_scotia.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_canada_nova_scotia_first_difference.png" width="260"/>
<em>canada - nova_scotia</em>

<img src="./figures/forecast_maxhorizon_7_canada_ontario_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_canada_ontario.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_canada_ontario_first_difference.png" width="260"/>
<em>canada - ontario</em>

<img src="./figures/forecast_maxhorizon_7_canada_prince_edward_island_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_canada_prince_edward_island.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_canada_prince_edward_island_first_difference.png" width="260"/>
<em>canada - prince_edward_island</em>

<img src="./figures/forecast_maxhorizon_7_canada_quebec_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_canada_quebec.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_canada_quebec_first_difference.png" width="260"/>
<em>canada - quebec</em>

<img src="./figures/forecast_maxhorizon_7_canada_saskatchewan_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_canada_saskatchewan.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_canada_saskatchewan_first_difference.png" width="260"/>
<em>canada - saskatchewan</em>

<img src="./figures/forecast_maxhorizon_7_central_african_republic__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_central_african_republic_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_central_african_republic__first_difference.png" width="260"/>
<em>central_african_republic - </em>

<img src="./figures/forecast_maxhorizon_7_chad__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_chad_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_chad__first_difference.png" width="260"/>
<em>chad - </em>

<img src="./figures/forecast_maxhorizon_7_chile__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_chile_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_chile__first_difference.png" width="260"/>
<em>chile - </em>

<img src="./figures/forecast_maxhorizon_7_china_anhui_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_anhui.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_anhui_first_difference.png" width="260"/>
<em>china - anhui</em>

<img src="./figures/forecast_maxhorizon_7_china_beijing_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_beijing.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_beijing_first_difference.png" width="260"/>
<em>china - beijing</em>

<img src="./figures/forecast_maxhorizon_7_china_chongqing_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_chongqing.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_chongqing_first_difference.png" width="260"/>
<em>china - chongqing</em>

<img src="./figures/forecast_maxhorizon_7_china_fujian_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_fujian.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_fujian_first_difference.png" width="260"/>
<em>china - fujian</em>

<img src="./figures/forecast_maxhorizon_7_china_gansu_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_gansu.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_gansu_first_difference.png" width="260"/>
<em>china - gansu</em>

<img src="./figures/forecast_maxhorizon_7_china_guangdong_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_guangdong.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_guangdong_first_difference.png" width="260"/>
<em>china - guangdong</em>

<img src="./figures/forecast_maxhorizon_7_china_guangxi_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_guangxi.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_guangxi_first_difference.png" width="260"/>
<em>china - guangxi</em>

<img src="./figures/forecast_maxhorizon_7_china_guizhou_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_guizhou.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_guizhou_first_difference.png" width="260"/>
<em>china - guizhou</em>

<img src="./figures/forecast_maxhorizon_7_china_hainan_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_hainan.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_hainan_first_difference.png" width="260"/>
<em>china - hainan</em>

<img src="./figures/forecast_maxhorizon_7_china_hebei_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_hebei.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_hebei_first_difference.png" width="260"/>
<em>china - hebei</em>

<img src="./figures/forecast_maxhorizon_7_china_heilongjiang_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_heilongjiang.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_heilongjiang_first_difference.png" width="260"/>
<em>china - heilongjiang</em>

<img src="./figures/forecast_maxhorizon_7_china_henan_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_henan.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_henan_first_difference.png" width="260"/>
<em>china - henan</em>

<img src="./figures/forecast_maxhorizon_7_china_hong_kong_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_hong_kong.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_hong_kong_first_difference.png" width="260"/>
<em>china - hong_kong</em>

<img src="./figures/forecast_maxhorizon_7_china_hubei_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_hubei.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_hubei_first_difference.png" width="260"/>
<em>china - hubei</em>

<img src="./figures/forecast_maxhorizon_7_china_hunan_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_hunan.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_hunan_first_difference.png" width="260"/>
<em>china - hunan</em>

<img src="./figures/forecast_maxhorizon_7_china_inner_mongolia_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_inner_mongolia.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_inner_mongolia_first_difference.png" width="260"/>
<em>china - inner_mongolia</em>

<img src="./figures/forecast_maxhorizon_7_china_jiangsu_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_jiangsu.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_jiangsu_first_difference.png" width="260"/>
<em>china - jiangsu</em>

<img src="./figures/forecast_maxhorizon_7_china_jiangxi_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_jiangxi.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_jiangxi_first_difference.png" width="260"/>
<em>china - jiangxi</em>

<img src="./figures/forecast_maxhorizon_7_china_jilin_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_jilin.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_jilin_first_difference.png" width="260"/>
<em>china - jilin</em>

<img src="./figures/forecast_maxhorizon_7_china_liaoning_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_liaoning.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_liaoning_first_difference.png" width="260"/>
<em>china - liaoning</em>

<img src="./figures/forecast_maxhorizon_7_china_macau_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_macau.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_macau_first_difference.png" width="260"/>
<em>china - macau</em>

<img src="./figures/forecast_maxhorizon_7_china_ningxia_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_ningxia.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_ningxia_first_difference.png" width="260"/>
<em>china - ningxia</em>

<img src="./figures/forecast_maxhorizon_7_china_shaanxi_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_shaanxi.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_shaanxi_first_difference.png" width="260"/>
<em>china - shaanxi</em>

<img src="./figures/forecast_maxhorizon_7_china_shandong_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_shandong.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_shandong_first_difference.png" width="260"/>
<em>china - shandong</em>

<img src="./figures/forecast_maxhorizon_7_china_shanghai_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_shanghai.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_shanghai_first_difference.png" width="260"/>
<em>china - shanghai</em>

<img src="./figures/forecast_maxhorizon_7_china_shanxi_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_shanxi.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_shanxi_first_difference.png" width="260"/>
<em>china - shanxi</em>

<img src="./figures/forecast_maxhorizon_7_china_sichuan_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_sichuan.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_sichuan_first_difference.png" width="260"/>
<em>china - sichuan</em>

<img src="./figures/forecast_maxhorizon_7_china_tianjin_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_tianjin.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_tianjin_first_difference.png" width="260"/>
<em>china - tianjin</em>

<img src="./figures/forecast_maxhorizon_7_china_xinjiang_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_xinjiang.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_xinjiang_first_difference.png" width="260"/>
<em>china - xinjiang</em>

<img src="./figures/forecast_maxhorizon_7_china_yunnan_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_yunnan.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_yunnan_first_difference.png" width="260"/>
<em>china - yunnan</em>

<img src="./figures/forecast_maxhorizon_7_china_zhejiang_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_china_zhejiang.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_china_zhejiang_first_difference.png" width="260"/>
<em>china - zhejiang</em>

<img src="./figures/forecast_maxhorizon_7_colombia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_colombia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_colombia__first_difference.png" width="260"/>
<em>colombia - </em>

<img src="./figures/forecast_maxhorizon_7_comoros__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_comoros_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_comoros__first_difference.png" width="260"/>
<em>comoros - </em>

<img src="./figures/forecast_maxhorizon_7_congo_(brazzaville)__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_congo_(brazzaville)_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_congo_(brazzaville)__first_difference.png" width="260"/>
<em>congo_(brazzaville) - </em>

<img src="./figures/forecast_maxhorizon_7_congo_(kinshasa)__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_congo_(kinshasa)_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_congo_(kinshasa)__first_difference.png" width="260"/>
<em>congo_(kinshasa) - </em>

<img src="./figures/forecast_maxhorizon_7_costa_rica__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_costa_rica_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_costa_rica__first_difference.png" width="260"/>
<em>costa_rica - </em>

<img src="./figures/forecast_maxhorizon_7_cote_d'ivoire__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_cote_d'ivoire_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_cote_d'ivoire__first_difference.png" width="260"/>
<em>cote_d'ivoire - </em>

<img src="./figures/forecast_maxhorizon_7_croatia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_croatia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_croatia__first_difference.png" width="260"/>
<em>croatia - </em>

<img src="./figures/forecast_maxhorizon_7_cuba__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_cuba_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_cuba__first_difference.png" width="260"/>
<em>cuba - </em>

<img src="./figures/forecast_maxhorizon_7_cyprus__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_cyprus_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_cyprus__first_difference.png" width="260"/>
<em>cyprus - </em>

<img src="./figures/forecast_maxhorizon_7_czechia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_czechia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_czechia__first_difference.png" width="260"/>
<em>czechia - </em>

<img src="./figures/forecast_maxhorizon_7_denmark_faroe_islands_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_denmark_faroe_islands.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_denmark_faroe_islands_first_difference.png" width="260"/>
<em>denmark - faroe_islands</em>

<img src="./figures/forecast_maxhorizon_7_denmark__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_denmark_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_denmark__first_difference.png" width="260"/>
<em>denmark - </em>

<img src="./figures/forecast_maxhorizon_7_diamond_princess__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_diamond_princess_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_diamond_princess__first_difference.png" width="260"/>
<em>diamond_princess - </em>

<img src="./figures/forecast_maxhorizon_7_djibouti__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_djibouti_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_djibouti__first_difference.png" width="260"/>
<em>djibouti - </em>

<img src="./figures/forecast_maxhorizon_7_dominica__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_dominica_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_dominica__first_difference.png" width="260"/>
<em>dominica - </em>

<img src="./figures/forecast_maxhorizon_7_dominican_republic__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_dominican_republic_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_dominican_republic__first_difference.png" width="260"/>
<em>dominican_republic - </em>

<img src="./figures/forecast_maxhorizon_7_ecuador__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_ecuador_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_ecuador__first_difference.png" width="260"/>
<em>ecuador - </em>

<img src="./figures/forecast_maxhorizon_7_egypt__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_egypt_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_egypt__first_difference.png" width="260"/>
<em>egypt - </em>

<img src="./figures/forecast_maxhorizon_7_el_salvador__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_el_salvador_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_el_salvador__first_difference.png" width="260"/>
<em>el_salvador - </em>

<img src="./figures/forecast_maxhorizon_7_equatorial_guinea__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_equatorial_guinea_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_equatorial_guinea__first_difference.png" width="260"/>
<em>equatorial_guinea - </em>

<img src="./figures/forecast_maxhorizon_7_eritrea__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_eritrea_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_eritrea__first_difference.png" width="260"/>
<em>eritrea - </em>

<img src="./figures/forecast_maxhorizon_7_estonia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_estonia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_estonia__first_difference.png" width="260"/>
<em>estonia - </em>

<img src="./figures/forecast_maxhorizon_7_eswatini__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_eswatini_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_eswatini__first_difference.png" width="260"/>
<em>eswatini - </em>

<img src="./figures/forecast_maxhorizon_7_ethiopia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_ethiopia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_ethiopia__first_difference.png" width="260"/>
<em>ethiopia - </em>

<img src="./figures/forecast_maxhorizon_7_fiji__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_fiji_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_fiji__first_difference.png" width="260"/>
<em>fiji - </em>

<img src="./figures/forecast_maxhorizon_7_finland__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_finland_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_finland__first_difference.png" width="260"/>
<em>finland - </em>

<img src="./figures/forecast_maxhorizon_7_france_french_guiana_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_france_french_guiana.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_france_french_guiana_first_difference.png" width="260"/>
<em>france - french_guiana</em>

<img src="./figures/forecast_maxhorizon_7_france_french_polynesia_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_france_french_polynesia.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_france_french_polynesia_first_difference.png" width="260"/>
<em>france - french_polynesia</em>

<img src="./figures/forecast_maxhorizon_7_france_guadeloupe_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_france_guadeloupe.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_france_guadeloupe_first_difference.png" width="260"/>
<em>france - guadeloupe</em>

<img src="./figures/forecast_maxhorizon_7_france_martinique_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_france_martinique.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_france_martinique_first_difference.png" width="260"/>
<em>france - martinique</em>

<img src="./figures/forecast_maxhorizon_7_france_mayotte_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_france_mayotte.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_france_mayotte_first_difference.png" width="260"/>
<em>france - mayotte</em>

<img src="./figures/forecast_maxhorizon_7_france_reunion_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_france_reunion.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_france_reunion_first_difference.png" width="260"/>
<em>france - reunion</em>

<img src="./figures/forecast_maxhorizon_7_france_saint_barthelemy_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_france_saint_barthelemy.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_france_saint_barthelemy_first_difference.png" width="260"/>
<em>france - saint_barthelemy</em>

<img src="./figures/forecast_maxhorizon_7_france_st_martin_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_france_st_martin.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_france_st_martin_first_difference.png" width="260"/>
<em>france - st_martin</em>

<img src="./figures/forecast_maxhorizon_7_france__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_france_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_france__first_difference.png" width="260"/>
<em>france - </em>

<img src="./figures/forecast_maxhorizon_7_gabon__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_gabon_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_gabon__first_difference.png" width="260"/>
<em>gabon - </em>

<img src="./figures/forecast_maxhorizon_7_gambia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_gambia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_gambia__first_difference.png" width="260"/>
<em>gambia - </em>

<img src="./figures/forecast_maxhorizon_7_georgia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_georgia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_georgia__first_difference.png" width="260"/>
<em>georgia - </em>

<img src="./figures/forecast_maxhorizon_7_germany__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_germany_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_germany__first_difference.png" width="260"/>
<em>germany - </em>

<img src="./figures/forecast_maxhorizon_7_ghana__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_ghana_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_ghana__first_difference.png" width="260"/>
<em>ghana - </em>

<img src="./figures/forecast_maxhorizon_7_greece__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_greece_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_greece__first_difference.png" width="260"/>
<em>greece - </em>

<img src="./figures/forecast_maxhorizon_7_grenada__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_grenada_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_grenada__first_difference.png" width="260"/>
<em>grenada - </em>

<img src="./figures/forecast_maxhorizon_7_guatemala__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_guatemala_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_guatemala__first_difference.png" width="260"/>
<em>guatemala - </em>

<img src="./figures/forecast_maxhorizon_7_guinea__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_guinea_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_guinea__first_difference.png" width="260"/>
<em>guinea - </em>

<img src="./figures/forecast_maxhorizon_7_guinea-bissau__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_guinea-bissau_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_guinea-bissau__first_difference.png" width="260"/>
<em>guinea-bissau - </em>

<img src="./figures/forecast_maxhorizon_7_guyana__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_guyana_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_guyana__first_difference.png" width="260"/>
<em>guyana - </em>

<img src="./figures/forecast_maxhorizon_7_haiti__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_haiti_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_haiti__first_difference.png" width="260"/>
<em>haiti - </em>

<img src="./figures/forecast_maxhorizon_7_honduras__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_honduras_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_honduras__first_difference.png" width="260"/>
<em>honduras - </em>

<img src="./figures/forecast_maxhorizon_7_hungary__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_hungary_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_hungary__first_difference.png" width="260"/>
<em>hungary - </em>

<img src="./figures/forecast_maxhorizon_7_iceland__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_iceland_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_iceland__first_difference.png" width="260"/>
<em>iceland - </em>

<img src="./figures/forecast_maxhorizon_7_india__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_india_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_india__first_difference.png" width="260"/>
<em>india - </em>

<img src="./figures/forecast_maxhorizon_7_indonesia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_indonesia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_indonesia__first_difference.png" width="260"/>
<em>indonesia - </em>

<img src="./figures/forecast_maxhorizon_7_iran__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_iran_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_iran__first_difference.png" width="260"/>
<em>iran - </em>

<img src="./figures/forecast_maxhorizon_7_iraq__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_iraq_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_iraq__first_difference.png" width="260"/>
<em>iraq - </em>

<img src="./figures/forecast_maxhorizon_7_ireland__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_ireland_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_ireland__first_difference.png" width="260"/>
<em>ireland - </em>

<img src="./figures/forecast_maxhorizon_7_israel__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_israel_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_israel__first_difference.png" width="260"/>
<em>israel - </em>

<img src="./figures/forecast_maxhorizon_7_italy__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_italy_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_italy__first_difference.png" width="260"/>
<em>italy - </em>

<img src="./figures/forecast_maxhorizon_7_jamaica__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_jamaica_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_jamaica__first_difference.png" width="260"/>
<em>jamaica - </em>

<img src="./figures/forecast_maxhorizon_7_japan__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_japan_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_japan__first_difference.png" width="260"/>
<em>japan - </em>

<img src="./figures/forecast_maxhorizon_7_jordan__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_jordan_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_jordan__first_difference.png" width="260"/>
<em>jordan - </em>

<img src="./figures/forecast_maxhorizon_7_kazakhstan__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_kazakhstan_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_kazakhstan__first_difference.png" width="260"/>
<em>kazakhstan - </em>

<img src="./figures/forecast_maxhorizon_7_kenya__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_kenya_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_kenya__first_difference.png" width="260"/>
<em>kenya - </em>

<img src="./figures/forecast_maxhorizon_7_korea_south__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_korea_south_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_korea_south__first_difference.png" width="260"/>
<em>korea_south - </em>

<img src="./figures/forecast_maxhorizon_7_kosovo__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_kosovo_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_kosovo__first_difference.png" width="260"/>
<em>kosovo - </em>

<img src="./figures/forecast_maxhorizon_7_kuwait__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_kuwait_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_kuwait__first_difference.png" width="260"/>
<em>kuwait - </em>

<img src="./figures/forecast_maxhorizon_7_kyrgyzstan__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_kyrgyzstan_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_kyrgyzstan__first_difference.png" width="260"/>
<em>kyrgyzstan - </em>

<img src="./figures/forecast_maxhorizon_7_latvia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_latvia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_latvia__first_difference.png" width="260"/>
<em>latvia - </em>

<img src="./figures/forecast_maxhorizon_7_lebanon__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_lebanon_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_lebanon__first_difference.png" width="260"/>
<em>lebanon - </em>

<img src="./figures/forecast_maxhorizon_7_lesotho__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_lesotho_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_lesotho__first_difference.png" width="260"/>
<em>lesotho - </em>

<img src="./figures/forecast_maxhorizon_7_liberia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_liberia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_liberia__first_difference.png" width="260"/>
<em>liberia - </em>

<img src="./figures/forecast_maxhorizon_7_libya__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_libya_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_libya__first_difference.png" width="260"/>
<em>libya - </em>

<img src="./figures/forecast_maxhorizon_7_liechtenstein__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_liechtenstein_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_liechtenstein__first_difference.png" width="260"/>
<em>liechtenstein - </em>

<img src="./figures/forecast_maxhorizon_7_lithuania__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_lithuania_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_lithuania__first_difference.png" width="260"/>
<em>lithuania - </em>

<img src="./figures/forecast_maxhorizon_7_luxembourg__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_luxembourg_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_luxembourg__first_difference.png" width="260"/>
<em>luxembourg - </em>

<img src="./figures/forecast_maxhorizon_7_madagascar__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_madagascar_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_madagascar__first_difference.png" width="260"/>
<em>madagascar - </em>

<img src="./figures/forecast_maxhorizon_7_malawi__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_malawi_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_malawi__first_difference.png" width="260"/>
<em>malawi - </em>

<img src="./figures/forecast_maxhorizon_7_malaysia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_malaysia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_malaysia__first_difference.png" width="260"/>
<em>malaysia - </em>

<img src="./figures/forecast_maxhorizon_7_maldives__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_maldives_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_maldives__first_difference.png" width="260"/>
<em>maldives - </em>

<img src="./figures/forecast_maxhorizon_7_mali__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_mali_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_mali__first_difference.png" width="260"/>
<em>mali - </em>

<img src="./figures/forecast_maxhorizon_7_malta__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_malta_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_malta__first_difference.png" width="260"/>
<em>malta - </em>

<img src="./figures/forecast_maxhorizon_7_mauritania__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_mauritania_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_mauritania__first_difference.png" width="260"/>
<em>mauritania - </em>

<img src="./figures/forecast_maxhorizon_7_mauritius__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_mauritius_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_mauritius__first_difference.png" width="260"/>
<em>mauritius - </em>

<img src="./figures/forecast_maxhorizon_7_mexico__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_mexico_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_mexico__first_difference.png" width="260"/>
<em>mexico - </em>

<img src="./figures/forecast_maxhorizon_7_moldova__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_moldova_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_moldova__first_difference.png" width="260"/>
<em>moldova - </em>

<img src="./figures/forecast_maxhorizon_7_monaco__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_monaco_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_monaco__first_difference.png" width="260"/>
<em>monaco - </em>

<img src="./figures/forecast_maxhorizon_7_mongolia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_mongolia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_mongolia__first_difference.png" width="260"/>
<em>mongolia - </em>

<img src="./figures/forecast_maxhorizon_7_montenegro__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_montenegro_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_montenegro__first_difference.png" width="260"/>
<em>montenegro - </em>

<img src="./figures/forecast_maxhorizon_7_morocco__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_morocco_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_morocco__first_difference.png" width="260"/>
<em>morocco - </em>

<img src="./figures/forecast_maxhorizon_7_mozambique__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_mozambique_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_mozambique__first_difference.png" width="260"/>
<em>mozambique - </em>

<img src="./figures/forecast_maxhorizon_7_namibia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_namibia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_namibia__first_difference.png" width="260"/>
<em>namibia - </em>

<img src="./figures/forecast_maxhorizon_7_nepal__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_nepal_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_nepal__first_difference.png" width="260"/>
<em>nepal - </em>

<img src="./figures/forecast_maxhorizon_7_netherlands_aruba_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_netherlands_aruba.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_netherlands_aruba_first_difference.png" width="260"/>
<em>netherlands - aruba</em>

<img src="./figures/forecast_maxhorizon_7_netherlands_bonaire_sint_eustatius_and_saba_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_netherlands_bonaire_sint_eustatius_and_saba.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_netherlands_bonaire_sint_eustatius_and_saba_first_difference.png" width="260"/>
<em>netherlands - bonaire_sint_eustatius_and_saba</em>

<img src="./figures/forecast_maxhorizon_7_netherlands_curacao_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_netherlands_curacao.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_netherlands_curacao_first_difference.png" width="260"/>
<em>netherlands - curacao</em>

<img src="./figures/forecast_maxhorizon_7_netherlands_sint_maarten_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_netherlands_sint_maarten.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_netherlands_sint_maarten_first_difference.png" width="260"/>
<em>netherlands - sint_maarten</em>

<img src="./figures/forecast_maxhorizon_7_netherlands__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_netherlands_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_netherlands__first_difference.png" width="260"/>
<em>netherlands - </em>

<img src="./figures/forecast_maxhorizon_7_new_zealand__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_new_zealand_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_new_zealand__first_difference.png" width="260"/>
<em>new_zealand - </em>

<img src="./figures/forecast_maxhorizon_7_nicaragua__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_nicaragua_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_nicaragua__first_difference.png" width="260"/>
<em>nicaragua - </em>

<img src="./figures/forecast_maxhorizon_7_niger__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_niger_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_niger__first_difference.png" width="260"/>
<em>niger - </em>

<img src="./figures/forecast_maxhorizon_7_nigeria__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_nigeria_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_nigeria__first_difference.png" width="260"/>
<em>nigeria - </em>

<img src="./figures/forecast_maxhorizon_7_north_macedonia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_north_macedonia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_north_macedonia__first_difference.png" width="260"/>
<em>north_macedonia - </em>

<img src="./figures/forecast_maxhorizon_7_norway__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_norway_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_norway__first_difference.png" width="260"/>
<em>norway - </em>

<img src="./figures/forecast_maxhorizon_7_oman__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_oman_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_oman__first_difference.png" width="260"/>
<em>oman - </em>

<img src="./figures/forecast_maxhorizon_7_pakistan__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_pakistan_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_pakistan__first_difference.png" width="260"/>
<em>pakistan - </em>

<img src="./figures/forecast_maxhorizon_7_panama__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_panama_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_panama__first_difference.png" width="260"/>
<em>panama - </em>

<img src="./figures/forecast_maxhorizon_7_papua_new_guinea__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_papua_new_guinea_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_papua_new_guinea__first_difference.png" width="260"/>
<em>papua_new_guinea - </em>

<img src="./figures/forecast_maxhorizon_7_paraguay__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_paraguay_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_paraguay__first_difference.png" width="260"/>
<em>paraguay - </em>

<img src="./figures/forecast_maxhorizon_7_peru__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_peru_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_peru__first_difference.png" width="260"/>
<em>peru - </em>

<img src="./figures/forecast_maxhorizon_7_philippines__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_philippines_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_philippines__first_difference.png" width="260"/>
<em>philippines - </em>

<img src="./figures/forecast_maxhorizon_7_poland__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_poland_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_poland__first_difference.png" width="260"/>
<em>poland - </em>

<img src="./figures/forecast_maxhorizon_7_portugal__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_portugal_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_portugal__first_difference.png" width="260"/>
<em>portugal - </em>

<img src="./figures/forecast_maxhorizon_7_qatar__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_qatar_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_qatar__first_difference.png" width="260"/>
<em>qatar - </em>

<img src="./figures/forecast_maxhorizon_7_romania__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_romania_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_romania__first_difference.png" width="260"/>
<em>romania - </em>

<img src="./figures/forecast_maxhorizon_7_russia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_russia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_russia__first_difference.png" width="260"/>
<em>russia - </em>

<img src="./figures/forecast_maxhorizon_7_rwanda__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_rwanda_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_rwanda__first_difference.png" width="260"/>
<em>rwanda - </em>

<img src="./figures/forecast_maxhorizon_7_saint_lucia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_saint_lucia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_saint_lucia__first_difference.png" width="260"/>
<em>saint_lucia - </em>

<img src="./figures/forecast_maxhorizon_7_saint_vincent_and_the_grenadines__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_saint_vincent_and_the_grenadines_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_saint_vincent_and_the_grenadines__first_difference.png" width="260"/>
<em>saint_vincent_and_the_grenadines - </em>

<img src="./figures/forecast_maxhorizon_7_san_marino__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_san_marino_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_san_marino__first_difference.png" width="260"/>
<em>san_marino - </em>

<img src="./figures/forecast_maxhorizon_7_sao_tome_and_principe__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_sao_tome_and_principe_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_sao_tome_and_principe__first_difference.png" width="260"/>
<em>sao_tome_and_principe - </em>

<img src="./figures/forecast_maxhorizon_7_saudi_arabia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_saudi_arabia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_saudi_arabia__first_difference.png" width="260"/>
<em>saudi_arabia - </em>

<img src="./figures/forecast_maxhorizon_7_senegal__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_senegal_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_senegal__first_difference.png" width="260"/>
<em>senegal - </em>

<img src="./figures/forecast_maxhorizon_7_serbia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_serbia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_serbia__first_difference.png" width="260"/>
<em>serbia - </em>

<img src="./figures/forecast_maxhorizon_7_seychelles__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_seychelles_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_seychelles__first_difference.png" width="260"/>
<em>seychelles - </em>

<img src="./figures/forecast_maxhorizon_7_sierra_leone__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_sierra_leone_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_sierra_leone__first_difference.png" width="260"/>
<em>sierra_leone - </em>

<img src="./figures/forecast_maxhorizon_7_singapore__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_singapore_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_singapore__first_difference.png" width="260"/>
<em>singapore - </em>

<img src="./figures/forecast_maxhorizon_7_slovakia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_slovakia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_slovakia__first_difference.png" width="260"/>
<em>slovakia - </em>

<img src="./figures/forecast_maxhorizon_7_slovenia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_slovenia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_slovenia__first_difference.png" width="260"/>
<em>slovenia - </em>

<img src="./figures/forecast_maxhorizon_7_somalia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_somalia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_somalia__first_difference.png" width="260"/>
<em>somalia - </em>

<img src="./figures/forecast_maxhorizon_7_south_africa__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_south_africa_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_south_africa__first_difference.png" width="260"/>
<em>south_africa - </em>

<img src="./figures/forecast_maxhorizon_7_south_sudan__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_south_sudan_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_south_sudan__first_difference.png" width="260"/>
<em>south_sudan - </em>

<img src="./figures/forecast_maxhorizon_7_spain__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_spain_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_spain__first_difference.png" width="260"/>
<em>spain - </em>

<img src="./figures/forecast_maxhorizon_7_sri_lanka__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_sri_lanka_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_sri_lanka__first_difference.png" width="260"/>
<em>sri_lanka - </em>

<img src="./figures/forecast_maxhorizon_7_sudan__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_sudan_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_sudan__first_difference.png" width="260"/>
<em>sudan - </em>

<img src="./figures/forecast_maxhorizon_7_suriname__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_suriname_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_suriname__first_difference.png" width="260"/>
<em>suriname - </em>

<img src="./figures/forecast_maxhorizon_7_sweden__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_sweden_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_sweden__first_difference.png" width="260"/>
<em>sweden - </em>

<img src="./figures/forecast_maxhorizon_7_switzerland__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_switzerland_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_switzerland__first_difference.png" width="260"/>
<em>switzerland - </em>

<img src="./figures/forecast_maxhorizon_7_syria__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_syria_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_syria__first_difference.png" width="260"/>
<em>syria - </em>

<img src="./figures/forecast_maxhorizon_7_taiwan*__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_taiwan*_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_taiwan*__first_difference.png" width="260"/>
<em>taiwan* - </em>

<img src="./figures/forecast_maxhorizon_7_tajikistan__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_tajikistan_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_tajikistan__first_difference.png" width="260"/>
<em>tajikistan - </em>

<img src="./figures/forecast_maxhorizon_7_tanzania__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_tanzania_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_tanzania__first_difference.png" width="260"/>
<em>tanzania - </em>

<img src="./figures/forecast_maxhorizon_7_thailand__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_thailand_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_thailand__first_difference.png" width="260"/>
<em>thailand - </em>

<img src="./figures/forecast_maxhorizon_7_timor-leste__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_timor-leste_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_timor-leste__first_difference.png" width="260"/>
<em>timor-leste - </em>

<img src="./figures/forecast_maxhorizon_7_togo__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_togo_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_togo__first_difference.png" width="260"/>
<em>togo - </em>

<img src="./figures/forecast_maxhorizon_7_trinidad_and_tobago__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_trinidad_and_tobago_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_trinidad_and_tobago__first_difference.png" width="260"/>
<em>trinidad_and_tobago - </em>

<img src="./figures/forecast_maxhorizon_7_tunisia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_tunisia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_tunisia__first_difference.png" width="260"/>
<em>tunisia - </em>

<img src="./figures/forecast_maxhorizon_7_turkey__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_turkey_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_turkey__first_difference.png" width="260"/>
<em>turkey - </em>

<img src="./figures/forecast_maxhorizon_7_us__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_us_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_us__first_difference.png" width="260"/>
<em>us - </em>

<img src="./figures/forecast_maxhorizon_7_uganda__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_uganda_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_uganda__first_difference.png" width="260"/>
<em>uganda - </em>

<img src="./figures/forecast_maxhorizon_7_ukraine__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_ukraine_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_ukraine__first_difference.png" width="260"/>
<em>ukraine - </em>

<img src="./figures/forecast_maxhorizon_7_united_arab_emirates__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_united_arab_emirates_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_united_arab_emirates__first_difference.png" width="260"/>
<em>united_arab_emirates - </em>

<img src="./figures/forecast_maxhorizon_7_united_kingdom_bermuda_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_united_kingdom_bermuda.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_united_kingdom_bermuda_first_difference.png" width="260"/>
<em>united_kingdom - bermuda</em>

<img src="./figures/forecast_maxhorizon_7_united_kingdom_british_virgin_islands_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_united_kingdom_british_virgin_islands.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_united_kingdom_british_virgin_islands_first_difference.png" width="260"/>
<em>united_kingdom - british_virgin_islands</em>

<img src="./figures/forecast_maxhorizon_7_united_kingdom_cayman_islands_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_united_kingdom_cayman_islands.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_united_kingdom_cayman_islands_first_difference.png" width="260"/>
<em>united_kingdom - cayman_islands</em>

<img src="./figures/forecast_maxhorizon_7_united_kingdom_channel_islands_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_united_kingdom_channel_islands.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_united_kingdom_channel_islands_first_difference.png" width="260"/>
<em>united_kingdom - channel_islands</em>

<img src="./figures/forecast_maxhorizon_7_united_kingdom_gibraltar_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_united_kingdom_gibraltar.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_united_kingdom_gibraltar_first_difference.png" width="260"/>
<em>united_kingdom - gibraltar</em>

<img src="./figures/forecast_maxhorizon_7_united_kingdom_isle_of_man_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_united_kingdom_isle_of_man.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_united_kingdom_isle_of_man_first_difference.png" width="260"/>
<em>united_kingdom - isle_of_man</em>

<img src="./figures/forecast_maxhorizon_7_united_kingdom_turks_and_caicos_islands_expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_united_kingdom_turks_and_caicos_islands.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_united_kingdom_turks_and_caicos_islands_first_difference.png" width="260"/>
<em>united_kingdom - turks_and_caicos_islands</em>

<img src="./figures/forecast_maxhorizon_7_united_kingdom__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_united_kingdom_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_united_kingdom__first_difference.png" width="260"/>
<em>united_kingdom - </em>

<img src="./figures/forecast_maxhorizon_7_uruguay__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_uruguay_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_uruguay__first_difference.png" width="260"/>
<em>uruguay - </em>

<img src="./figures/forecast_maxhorizon_7_uzbekistan__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_uzbekistan_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_uzbekistan__first_difference.png" width="260"/>
<em>uzbekistan - </em>

<img src="./figures/forecast_maxhorizon_7_venezuela__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_venezuela_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_venezuela__first_difference.png" width="260"/>
<em>venezuela - </em>

<img src="./figures/forecast_maxhorizon_7_vietnam__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_vietnam_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_vietnam__first_difference.png" width="260"/>
<em>vietnam - </em>

<img src="./figures/forecast_maxhorizon_7_west_bank_and_gaza__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_west_bank_and_gaza_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_west_bank_and_gaza__first_difference.png" width="260"/>
<em>west_bank_and_gaza - </em>

<img src="./figures/forecast_maxhorizon_7_yemen__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_yemen_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_yemen__first_difference.png" width="260"/>
<em>yemen - </em>

<img src="./figures/forecast_maxhorizon_7_zambia__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_zambia_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_zambia__first_difference.png" width="260"/>
<em>zambia - </em>

<img src="./figures/forecast_maxhorizon_7_zimbabwe__expost.png" width="260"/>            <img src="./figures/forecast_maxhorizon_7_zimbabwe_.png" width="260"/> <img src="./figures/forecast_maxhorizon_7_zimbabwe__first_difference.png" width="260"/>
<em>zimbabwe - </em>
