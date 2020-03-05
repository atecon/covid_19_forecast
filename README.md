# Time-series based 7-days ahead forecasts of (known) confirmed cases of the novel Coronavirus COVID-19 (2019-nCoV)

**Statement**: This is just "hobby" project and forecast results *should not* be taken too seriously! Even though forecast performance may be reasonable for some country and some period in time. Don't take the numbers forecast as granted!
This project is mainly supposed to show what may be realized (without too much effort) with the open-source statistics and econometrics software gretl (URL: http://gretl.sourceforge.net/).

**An automated job** retrives latest data at 3am (CET), trains a new model, computes the forecasts and uploads the new forecasting plots here to my github-repo. The overall job finishes in about 15 seconds on a Raspberry Pi 4 computer --- this is just an amazing piece of hardware ;-)

## Data source
Data provided by the Johns Hopkins University Center for Systems Science and Engineering (JHU CCSE) can be found here:
https://github.com/CSSEGISandData/COVID-19

## Some words on the underlying model
I want to keep it simple. If you fancy, you can use the current state of code as a starting point for devloping and evaluating more complex approaches. However, the ARIMA type of model applied may already provide a reasonable approach for modelling and computing short-term contagion dynamics. *Gretl's* built-in ```arima``` command is used for this.

The default model is an ARIMA(2,1,1) specification (every textbook covering time-seres has a chapter on this traditional approach).
In case the maximum-likelihood estimator does not converge, an ARIMA(1,1,0) will be estimated. If this specification also fails to converge a simple ARIMA(0,1,0) will be tried.

## Forecasting method
We compute out-of-sample multi-period interval forecasts. The multi-period forecast is recursively computed. Per default the 90 % forecast (Gaussian) interval will be shown as well.

## The gretl script
The gretl script for setting up relevant things and executing the analysis is ```./script/run.inp```.

At the beginning of the script, the user can specify the following parameters:
```
string DIR_WORK = "" 		# <SET_PATH_HERE> e.g. "/home/git_project"
string DATA_URL = "https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_19-covid-Confirmed.csv"	# Data source
string INITIAL_DATE = "2020-01-22"			# 1st available observation of CSSE dataset
scalar MAX_HORIZON = 7					# maximum multi-step OoS forecast horizon
scalar RUN_EXPOST_ANALYSIS = 0				# run expost forecasting exercise, by going 'MAX_HORIZON' back in time
scalar COMPILE_MARKDOWN_CMD = 1				# compile markdown command for inserting forecasting plots to README.md

# ARIMA model settings
scalar ARIMA_P = 2					# autoregressive (AR) order
scalar ARIMA_D = 1					# differencing order
scalar ARIMA_Q = 1					# moving average (MA) order
```

This script will also load the functions doing the main stuff in the beckground which are stored in ```./src/helper.inp```.

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

# Up-to-date out-of-sample 7-days ahead forecasts

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_anhui.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_beijing.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_chongqing.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_fujian.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_gansu.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_guangdong.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_guangxi.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_guizhou.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_hainan.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_hebei.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_heilongjiang.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_henan.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_hubei.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_hunan.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_inner_mongolia.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_jiangsu.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_jiangxi.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_jilin.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_liaoning.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_ningxia.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_qinghai.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_shaanxi.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_shandong.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_shanghai.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_shanxi.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_sichuan.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_tianjin.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_tibet.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_xinjiang.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_yunnan.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_zhejiang.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_thailand_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_japan_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_south_korea_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_taiwan_taiwan.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_king_county_wa.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_cook_county_il.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_tempe_az.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_macau_macau.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_hong_kong_hong_kong.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_singapore_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_vietnam_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_france_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_nepal_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_malaysia_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_canada_toronto_on.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_canada_british_columbia.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_los_angeles_ca.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_australia_new_south_wales.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_australia_victoria.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_australia_queensland.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_cambodia_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_sri_lanka_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_germany_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_finland_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_united_arab_emirates_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_philippines_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_india_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_canada_london_on.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_italy_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_uk_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_russia_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_sweden_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_santa_clara_ca.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_spain_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_australia_south_australia.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_boston_ma.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_san_benito_ca.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_belgium_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_madison_wi.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_others_diamond_princess_cruise_ship.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_san_diego_county_ca.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_san_antonio_tx.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_egypt_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_iran_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_omaha_ne_(from_diamond_princess).png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_travis_ca_(from_diamond_princess).png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_australia_from_diamond_princess.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_lackland_tx_(from_diamond_princess).png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_lebanon_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_humboldt_county_ca.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_sacramento_county_ca.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_iraq_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_unassigned_location_(from_diamond_princess).png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_oman_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_afghanistan_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_bahrain_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_kuwait_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_algeria_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_croatia_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_switzerland_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_austria_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_israel_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_pakistan_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_brazil_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_georgia_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_greece_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_north_macedonia_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_norway_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_romania_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_denmark_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_estonia_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_netherlands_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_san_marino_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_belarus_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_canada_montreal_qc.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_iceland_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_lithuania_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_mexico_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_new_zealand_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_nigeria_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_australia_western_australia.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_ireland_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_luxembourg_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_monaco_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_qatar_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_snohomish_county_wa.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_ecuador_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_azerbaijan_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_czech_republic_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_armenia_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_dominican_republic_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_providence_ri.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_indonesia_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_portugal_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_andorra_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_australia_tasmania.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_latvia_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_morocco_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_saudi_arabia_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_senegal_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_new_york_city_ny.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_sarasota_fl.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_sonoma_county_ca.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_umatilla_or.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_fulton_county_ga.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_washington_county_or.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_argentina_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_chile_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_jordan_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_norfolk_county_ma.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_berkeley_ca.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_maricopa_county_az.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_wake_county_nc.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_ukraine_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_saint_barthelemy_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_orange_county_ca.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_hungary_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_australia_northern_territory.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_faroe_islands_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_gibraltar_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_liechtenstein_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_poland_.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_tunisia_.png" width="425"/>

<img src="./figures/forecast_arima_maxhorizon_7_us_contra_costa_county_ca.png" width="425"/>