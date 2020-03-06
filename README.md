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

# Ex-Post and up-to-date out-of-sample 7-days ahead forecasts
The left panel shows forecast made in information available 7 days ago and the realization of 'confirmed cases' during this period. This may give you an idea of how 'well' the forecasted dynamics were. 

The right panel shows forecasts made on latest data for the forthcoming 7 days.


<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_anhui_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_anhui.png" width="425"/>
<em>mainland_china - anhui</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_beijing_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_beijing.png" width="425"/>
<em>mainland_china - beijing</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_chongqing_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_chongqing.png" width="425"/>
<em>mainland_china - chongqing</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_fujian_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_fujian.png" width="425"/>
<em>mainland_china - fujian</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_gansu_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_gansu.png" width="425"/>
<em>mainland_china - gansu</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_guangdong_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_guangdong.png" width="425"/>
<em>mainland_china - guangdong</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_guangxi_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_guangxi.png" width="425"/>
<em>mainland_china - guangxi</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_guizhou_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_guizhou.png" width="425"/>
<em>mainland_china - guizhou</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_hainan_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_hainan.png" width="425"/>
<em>mainland_china - hainan</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_hebei_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_hebei.png" width="425"/>
<em>mainland_china - hebei</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_heilongjiang_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_heilongjiang.png" width="425"/>
<em>mainland_china - heilongjiang</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_henan_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_henan.png" width="425"/>
<em>mainland_china - henan</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_hubei_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_hubei.png" width="425"/>
<em>mainland_china - hubei</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_hunan_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_hunan.png" width="425"/>
<em>mainland_china - hunan</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_inner_mongolia_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_inner_mongolia.png" width="425"/>
<em>mainland_china - inner_mongolia</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_jiangsu_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_jiangsu.png" width="425"/>
<em>mainland_china - jiangsu</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_jiangxi_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_jiangxi.png" width="425"/>
<em>mainland_china - jiangxi</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_jilin_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_jilin.png" width="425"/>
<em>mainland_china - jilin</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_liaoning_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_liaoning.png" width="425"/>
<em>mainland_china - liaoning</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_ningxia_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_ningxia.png" width="425"/>
<em>mainland_china - ningxia</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_qinghai_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_qinghai.png" width="425"/>
<em>mainland_china - qinghai</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_shaanxi_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_shaanxi.png" width="425"/>
<em>mainland_china - shaanxi</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_shandong_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_shandong.png" width="425"/>
<em>mainland_china - shandong</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_shanghai_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_shanghai.png" width="425"/>
<em>mainland_china - shanghai</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_shanxi_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_shanxi.png" width="425"/>
<em>mainland_china - shanxi</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_sichuan_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_sichuan.png" width="425"/>
<em>mainland_china - sichuan</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_tianjin_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_tianjin.png" width="425"/>
<em>mainland_china - tianjin</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_tibet_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_tibet.png" width="425"/>
<em>mainland_china - tibet</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_xinjiang_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_xinjiang.png" width="425"/>
<em>mainland_china - xinjiang</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_yunnan_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_yunnan.png" width="425"/>
<em>mainland_china - yunnan</em>

<img src="./figures/forecast_arima_maxhorizon_7_mainland_china_zhejiang_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mainland_china_zhejiang.png" width="425"/>
<em>mainland_china - zhejiang</em>

<img src="./figures/forecast_arima_maxhorizon_7_thailand__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_thailand_.png" width="425"/>
<em>thailand - </em>

<img src="./figures/forecast_arima_maxhorizon_7_japan__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_japan_.png" width="425"/>
<em>japan - </em>

<img src="./figures/forecast_arima_maxhorizon_7_south_korea__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_south_korea_.png" width="425"/>
<em>south_korea - </em>

<img src="./figures/forecast_arima_maxhorizon_7_taiwan_taiwan_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_taiwan_taiwan.png" width="425"/>
<em>taiwan - taiwan</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_king_county_wa_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_king_county_wa.png" width="425"/>
<em>us - king_county_wa</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_cook_county_il_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_cook_county_il.png" width="425"/>
<em>us - cook_county_il</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_tempe_az_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_tempe_az.png" width="425"/>
<em>us - tempe_az</em>

<img src="./figures/forecast_arima_maxhorizon_7_macau_macau_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_macau_macau.png" width="425"/>
<em>macau - macau</em>

<img src="./figures/forecast_arima_maxhorizon_7_hong_kong_hong_kong_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_hong_kong_hong_kong.png" width="425"/>
<em>hong_kong - hong_kong</em>

<img src="./figures/forecast_arima_maxhorizon_7_singapore__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_singapore_.png" width="425"/>
<em>singapore - </em>

<img src="./figures/forecast_arima_maxhorizon_7_vietnam__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_vietnam_.png" width="425"/>
<em>vietnam - </em>

<img src="./figures/forecast_arima_maxhorizon_7_france__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_france_.png" width="425"/>
<em>france - </em>

<img src="./figures/forecast_arima_maxhorizon_7_nepal__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_nepal_.png" width="425"/>
<em>nepal - </em>

<img src="./figures/forecast_arima_maxhorizon_7_malaysia__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_malaysia_.png" width="425"/>
<em>malaysia - </em>

<img src="./figures/forecast_arima_maxhorizon_7_canada_toronto_on_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_canada_toronto_on.png" width="425"/>
<em>canada - toronto_on</em>

<img src="./figures/forecast_arima_maxhorizon_7_canada_british_columbia_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_canada_british_columbia.png" width="425"/>
<em>canada - british_columbia</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_los_angeles_ca_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_los_angeles_ca.png" width="425"/>
<em>us - los_angeles_ca</em>

<img src="./figures/forecast_arima_maxhorizon_7_australia_new_south_wales_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_australia_new_south_wales.png" width="425"/>
<em>australia - new_south_wales</em>

<img src="./figures/forecast_arima_maxhorizon_7_australia_victoria_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_australia_victoria.png" width="425"/>
<em>australia - victoria</em>

<img src="./figures/forecast_arima_maxhorizon_7_australia_queensland_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_australia_queensland.png" width="425"/>
<em>australia - queensland</em>

<img src="./figures/forecast_arima_maxhorizon_7_cambodia__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_cambodia_.png" width="425"/>
<em>cambodia - </em>

<img src="./figures/forecast_arima_maxhorizon_7_sri_lanka__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_sri_lanka_.png" width="425"/>
<em>sri_lanka - </em>

<img src="./figures/forecast_arima_maxhorizon_7_germany__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_germany_.png" width="425"/>
<em>germany - </em>

<img src="./figures/forecast_arima_maxhorizon_7_finland__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_finland_.png" width="425"/>
<em>finland - </em>

<img src="./figures/forecast_arima_maxhorizon_7_united_arab_emirates__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_united_arab_emirates_.png" width="425"/>
<em>united_arab_emirates - </em>

<img src="./figures/forecast_arima_maxhorizon_7_philippines__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_philippines_.png" width="425"/>
<em>philippines - </em>

<img src="./figures/forecast_arima_maxhorizon_7_india__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_india_.png" width="425"/>
<em>india - </em>

<img src="./figures/forecast_arima_maxhorizon_7_canada_london_on_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_canada_london_on.png" width="425"/>
<em>canada - london_on</em>

<img src="./figures/forecast_arima_maxhorizon_7_italy__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_italy_.png" width="425"/>
<em>italy - </em>

<img src="./figures/forecast_arima_maxhorizon_7_uk__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_uk_.png" width="425"/>
<em>uk - </em>

<img src="./figures/forecast_arima_maxhorizon_7_russia__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_russia_.png" width="425"/>
<em>russia - </em>

<img src="./figures/forecast_arima_maxhorizon_7_sweden__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_sweden_.png" width="425"/>
<em>sweden - </em>

<img src="./figures/forecast_arima_maxhorizon_7_us_santa_clara_ca_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_santa_clara_ca.png" width="425"/>
<em>us - santa_clara_ca</em>

<img src="./figures/forecast_arima_maxhorizon_7_spain__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_spain_.png" width="425"/>
<em>spain - </em>

<img src="./figures/forecast_arima_maxhorizon_7_australia_south_australia_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_australia_south_australia.png" width="425"/>
<em>australia - south_australia</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_boston_ma_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_boston_ma.png" width="425"/>
<em>us - boston_ma</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_san_benito_ca_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_san_benito_ca.png" width="425"/>
<em>us - san_benito_ca</em>

<img src="./figures/forecast_arima_maxhorizon_7_belgium__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_belgium_.png" width="425"/>
<em>belgium - </em>

<img src="./figures/forecast_arima_maxhorizon_7_us_madison_wi_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_madison_wi.png" width="425"/>
<em>us - madison_wi</em>

<img src="./figures/forecast_arima_maxhorizon_7_others_diamond_princess_cruise_ship_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_others_diamond_princess_cruise_ship.png" width="425"/>
<em>others - diamond_princess_cruise_ship</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_san_diego_county_ca_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_san_diego_county_ca.png" width="425"/>
<em>us - san_diego_county_ca</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_san_antonio_tx_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_san_antonio_tx.png" width="425"/>
<em>us - san_antonio_tx</em>

<img src="./figures/forecast_arima_maxhorizon_7_egypt__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_egypt_.png" width="425"/>
<em>egypt - </em>

<img src="./figures/forecast_arima_maxhorizon_7_iran__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_iran_.png" width="425"/>
<em>iran - </em>

<img src="./figures/forecast_arima_maxhorizon_7_us_omaha_ne_(from_diamond_princess)_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_omaha_ne_(from_diamond_princess).png" width="425"/>
<em>us - omaha_ne_(from_diamond_princess)</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_travis_ca_(from_diamond_princess)_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_travis_ca_(from_diamond_princess).png" width="425"/>
<em>us - travis_ca_(from_diamond_princess)</em>

<img src="./figures/forecast_arima_maxhorizon_7_australia_from_diamond_princess_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_australia_from_diamond_princess.png" width="425"/>
<em>australia - from_diamond_princess</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_lackland_tx_(from_diamond_princess)_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_lackland_tx_(from_diamond_princess).png" width="425"/>
<em>us - lackland_tx_(from_diamond_princess)</em>

<img src="./figures/forecast_arima_maxhorizon_7_lebanon__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_lebanon_.png" width="425"/>
<em>lebanon - </em>

<img src="./figures/forecast_arima_maxhorizon_7_us_humboldt_county_ca_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_humboldt_county_ca.png" width="425"/>
<em>us - humboldt_county_ca</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_sacramento_county_ca_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_sacramento_county_ca.png" width="425"/>
<em>us - sacramento_county_ca</em>

<img src="./figures/forecast_arima_maxhorizon_7_iraq__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_iraq_.png" width="425"/>
<em>iraq - </em>

<img src="./figures/forecast_arima_maxhorizon_7_us_unassigned_location_(from_diamond_princess)_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_unassigned_location_(from_diamond_princess).png" width="425"/>
<em>us - unassigned_location_(from_diamond_princess)</em>

<img src="./figures/forecast_arima_maxhorizon_7_oman__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_oman_.png" width="425"/>
<em>oman - </em>

<img src="./figures/forecast_arima_maxhorizon_7_afghanistan__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_afghanistan_.png" width="425"/>
<em>afghanistan - </em>

<img src="./figures/forecast_arima_maxhorizon_7_bahrain__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_bahrain_.png" width="425"/>
<em>bahrain - </em>

<img src="./figures/forecast_arima_maxhorizon_7_kuwait__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_kuwait_.png" width="425"/>
<em>kuwait - </em>

<img src="./figures/forecast_arima_maxhorizon_7_algeria__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_algeria_.png" width="425"/>
<em>algeria - </em>

<img src="./figures/forecast_arima_maxhorizon_7_croatia__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_croatia_.png" width="425"/>
<em>croatia - </em>

<img src="./figures/forecast_arima_maxhorizon_7_switzerland__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_switzerland_.png" width="425"/>
<em>switzerland - </em>

<img src="./figures/forecast_arima_maxhorizon_7_austria__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_austria_.png" width="425"/>
<em>austria - </em>

<img src="./figures/forecast_arima_maxhorizon_7_israel__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_israel_.png" width="425"/>
<em>israel - </em>

<img src="./figures/forecast_arima_maxhorizon_7_pakistan__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_pakistan_.png" width="425"/>
<em>pakistan - </em>

<img src="./figures/forecast_arima_maxhorizon_7_brazil__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_brazil_.png" width="425"/>
<em>brazil - </em>

<img src="./figures/forecast_arima_maxhorizon_7_georgia__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_georgia_.png" width="425"/>
<em>georgia - </em>

<img src="./figures/forecast_arima_maxhorizon_7_greece__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_greece_.png" width="425"/>
<em>greece - </em>

<img src="./figures/forecast_arima_maxhorizon_7_north_macedonia__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_north_macedonia_.png" width="425"/>
<em>north_macedonia - </em>

<img src="./figures/forecast_arima_maxhorizon_7_norway__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_norway_.png" width="425"/>
<em>norway - </em>

<img src="./figures/forecast_arima_maxhorizon_7_romania__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_romania_.png" width="425"/>
<em>romania - </em>

<img src="./figures/forecast_arima_maxhorizon_7_denmark__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_denmark_.png" width="425"/>
<em>denmark - </em>

<img src="./figures/forecast_arima_maxhorizon_7_estonia__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_estonia_.png" width="425"/>
<em>estonia - </em>

<img src="./figures/forecast_arima_maxhorizon_7_netherlands__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_netherlands_.png" width="425"/>
<em>netherlands - </em>

<img src="./figures/forecast_arima_maxhorizon_7_san_marino__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_san_marino_.png" width="425"/>
<em>san_marino - </em>

<img src="./figures/forecast_arima_maxhorizon_7_belarus__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_belarus_.png" width="425"/>
<em>belarus - </em>

<img src="./figures/forecast_arima_maxhorizon_7_canada_montreal_qc_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_canada_montreal_qc.png" width="425"/>
<em>canada - montreal_qc</em>

<img src="./figures/forecast_arima_maxhorizon_7_iceland__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_iceland_.png" width="425"/>
<em>iceland - </em>

<img src="./figures/forecast_arima_maxhorizon_7_lithuania__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_lithuania_.png" width="425"/>
<em>lithuania - </em>

<img src="./figures/forecast_arima_maxhorizon_7_mexico__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_mexico_.png" width="425"/>
<em>mexico - </em>

<img src="./figures/forecast_arima_maxhorizon_7_new_zealand__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_new_zealand_.png" width="425"/>
<em>new_zealand - </em>

<img src="./figures/forecast_arima_maxhorizon_7_nigeria__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_nigeria_.png" width="425"/>
<em>nigeria - </em>

<img src="./figures/forecast_arima_maxhorizon_7_australia_western_australia_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_australia_western_australia.png" width="425"/>
<em>australia - western_australia</em>

<img src="./figures/forecast_arima_maxhorizon_7_ireland__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_ireland_.png" width="425"/>
<em>ireland - </em>

<img src="./figures/forecast_arima_maxhorizon_7_luxembourg__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_luxembourg_.png" width="425"/>
<em>luxembourg - </em>

<img src="./figures/forecast_arima_maxhorizon_7_monaco__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_monaco_.png" width="425"/>
<em>monaco - </em>

<img src="./figures/forecast_arima_maxhorizon_7_qatar__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_qatar_.png" width="425"/>
<em>qatar - </em>

<img src="./figures/forecast_arima_maxhorizon_7_us_snohomish_county_wa_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_snohomish_county_wa.png" width="425"/>
<em>us - snohomish_county_wa</em>

<img src="./figures/forecast_arima_maxhorizon_7_ecuador__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_ecuador_.png" width="425"/>
<em>ecuador - </em>

<img src="./figures/forecast_arima_maxhorizon_7_azerbaijan__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_azerbaijan_.png" width="425"/>
<em>azerbaijan - </em>

<img src="./figures/forecast_arima_maxhorizon_7_czech_republic__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_czech_republic_.png" width="425"/>
<em>czech_republic - </em>

<img src="./figures/forecast_arima_maxhorizon_7_armenia__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_armenia_.png" width="425"/>
<em>armenia - </em>

<img src="./figures/forecast_arima_maxhorizon_7_dominican_republic__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_dominican_republic_.png" width="425"/>
<em>dominican_republic - </em>

<img src="./figures/forecast_arima_maxhorizon_7_us_providence_ri_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_providence_ri.png" width="425"/>
<em>us - providence_ri</em>

<img src="./figures/forecast_arima_maxhorizon_7_indonesia__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_indonesia_.png" width="425"/>
<em>indonesia - </em>

<img src="./figures/forecast_arima_maxhorizon_7_portugal__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_portugal_.png" width="425"/>
<em>portugal - </em>

<img src="./figures/forecast_arima_maxhorizon_7_andorra__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_andorra_.png" width="425"/>
<em>andorra - </em>

<img src="./figures/forecast_arima_maxhorizon_7_australia_tasmania_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_australia_tasmania.png" width="425"/>
<em>australia - tasmania</em>

<img src="./figures/forecast_arima_maxhorizon_7_latvia__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_latvia_.png" width="425"/>
<em>latvia - </em>

<img src="./figures/forecast_arima_maxhorizon_7_morocco__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_morocco_.png" width="425"/>
<em>morocco - </em>

<img src="./figures/forecast_arima_maxhorizon_7_saudi_arabia__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_saudi_arabia_.png" width="425"/>
<em>saudi_arabia - </em>

<img src="./figures/forecast_arima_maxhorizon_7_senegal__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_senegal_.png" width="425"/>
<em>senegal - </em>

<img src="./figures/forecast_arima_maxhorizon_7_us_grafton_county_nh_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_grafton_county_nh.png" width="425"/>
<em>us - grafton_county_nh</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_hillsborough_fl_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_hillsborough_fl.png" width="425"/>
<em>us - hillsborough_fl</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_new_york_city_ny_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_new_york_city_ny.png" width="425"/>
<em>us - new_york_city_ny</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_placer_county_ca_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_placer_county_ca.png" width="425"/>
<em>us - placer_county_ca</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_san_mateo_ca_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_san_mateo_ca.png" width="425"/>
<em>us - san_mateo_ca</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_sarasota_fl_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_sarasota_fl.png" width="425"/>
<em>us - sarasota_fl</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_sonoma_county_ca_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_sonoma_county_ca.png" width="425"/>
<em>us - sonoma_county_ca</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_umatilla_or_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_umatilla_or.png" width="425"/>
<em>us - umatilla_or</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_fulton_county_ga_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_fulton_county_ga.png" width="425"/>
<em>us - fulton_county_ga</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_washington_county_or_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_washington_county_or.png" width="425"/>
<em>us - washington_county_or</em>

<img src="./figures/forecast_arima_maxhorizon_7_argentina__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_argentina_.png" width="425"/>
<em>argentina - </em>

<img src="./figures/forecast_arima_maxhorizon_7_chile__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_chile_.png" width="425"/>
<em>chile - </em>

<img src="./figures/forecast_arima_maxhorizon_7_jordan__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_jordan_.png" width="425"/>
<em>jordan - </em>

<img src="./figures/forecast_arima_maxhorizon_7_us_norfolk_county_ma_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_norfolk_county_ma.png" width="425"/>
<em>us - norfolk_county_ma</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_berkeley_ca_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_berkeley_ca.png" width="425"/>
<em>us - berkeley_ca</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_maricopa_county_az_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_maricopa_county_az.png" width="425"/>
<em>us - maricopa_county_az</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_wake_county_nc_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_wake_county_nc.png" width="425"/>
<em>us - wake_county_nc</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_westchester_county_ny_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_westchester_county_ny.png" width="425"/>
<em>us - westchester_county_ny</em>

<img src="./figures/forecast_arima_maxhorizon_7_ukraine__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_ukraine_.png" width="425"/>
<em>ukraine - </em>

<img src="./figures/forecast_arima_maxhorizon_7_saint_barthelemy__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_saint_barthelemy_.png" width="425"/>
<em>saint_barthelemy - </em>

<img src="./figures/forecast_arima_maxhorizon_7_us_orange_county_ca_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_orange_county_ca.png" width="425"/>
<em>us - orange_county_ca</em>

<img src="./figures/forecast_arima_maxhorizon_7_hungary__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_hungary_.png" width="425"/>
<em>hungary - </em>

<img src="./figures/forecast_arima_maxhorizon_7_australia_northern_territory_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_australia_northern_territory.png" width="425"/>
<em>australia - northern_territory</em>

<img src="./figures/forecast_arima_maxhorizon_7_faroe_islands__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_faroe_islands_.png" width="425"/>
<em>faroe_islands - </em>

<img src="./figures/forecast_arima_maxhorizon_7_gibraltar__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_gibraltar_.png" width="425"/>
<em>gibraltar - </em>

<img src="./figures/forecast_arima_maxhorizon_7_liechtenstein__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_liechtenstein_.png" width="425"/>
<em>liechtenstein - </em>

<img src="./figures/forecast_arima_maxhorizon_7_poland__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_poland_.png" width="425"/>
<em>poland - </em>

<img src="./figures/forecast_arima_maxhorizon_7_tunisia__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_tunisia_.png" width="425"/>
<em>tunisia - </em>

<img src="./figures/forecast_arima_maxhorizon_7_us_contra_costa_county_ca_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_contra_costa_county_ca.png" width="425"/>
<em>us - contra_costa_county_ca</em>

<img src="./figures/forecast_arima_maxhorizon_7_palestine__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_palestine_.png" width="425"/>
<em>palestine - </em>

<img src="./figures/forecast_arima_maxhorizon_7_bosnia_and_herzegovina__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_bosnia_and_herzegovina_.png" width="425"/>
<em>bosnia_and_herzegovina - </em>

<img src="./figures/forecast_arima_maxhorizon_7_slovenia__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_slovenia_.png" width="425"/>
<em>slovenia - </em>

<img src="./figures/forecast_arima_maxhorizon_7_us_bergen_county_nj_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_bergen_county_nj.png" width="425"/>
<em>us - bergen_county_nj</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_harris_county_tx_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_harris_county_tx.png" width="425"/>
<em>us - harris_county_tx</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_san_francisco_county_ca_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_san_francisco_county_ca.png" width="425"/>
<em>us - san_francisco_county_ca</em>

<img src="./figures/forecast_arima_maxhorizon_7_south_africa__expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_south_africa_.png" width="425"/>
<em>south_africa - </em>

<img src="./figures/forecast_arima_maxhorizon_7_us_clark_county_nv_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_clark_county_nv.png" width="425"/>
<em>us - clark_county_nv</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_fort_bend_county_tx_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_fort_bend_county_tx.png" width="425"/>
<em>us - fort_bend_county_tx</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_grant_county_wa_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_grant_county_wa.png" width="425"/>
<em>us - grant_county_wa</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_queens_county_ny_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_queens_county_ny.png" width="425"/>
<em>us - queens_county_ny</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_santa_rosa_county_fl_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_santa_rosa_county_fl.png" width="425"/>
<em>us - santa_rosa_county_fl</em>

<img src="./figures/forecast_arima_maxhorizon_7_us_williamson_county_tn_expost.png" width="425"/> <img src="./figures/forecast_arima_maxhorizon_7_us_williamson_county_tn.png" width="425"/>
<em>us - williamson_county_tn</em>
