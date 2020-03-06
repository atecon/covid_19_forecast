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

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_anhui_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_anhui" width="425"/>
<em>mainland_china - anhui</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_beijing_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_beijing" width="425"/>
<em>mainland_china - beijing</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_chongqing_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_chongqing" width="425"/>
<em>mainland_china - chongqing</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_fujian_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_fujian" width="425"/>
<em>mainland_china - fujian</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_gansu_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_gansu" width="425"/>
<em>mainland_china - gansu</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_guangdong_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_guangdong" width="425"/>
<em>mainland_china - guangdong</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_guangxi_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_guangxi" width="425"/>
<em>mainland_china - guangxi</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_guizhou_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_guizhou" width="425"/>
<em>mainland_china - guizhou</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_hainan_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_hainan" width="425"/>
<em>mainland_china - hainan</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_hebei_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_hebei" width="425"/>
<em>mainland_china - hebei</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_heilongjiang_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_heilongjiang" width="425"/>
<em>mainland_china - heilongjiang</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_henan_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_henan" width="425"/>
<em>mainland_china - henan</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_hubei_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_hubei" width="425"/>
<em>mainland_china - hubei</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_hunan_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_hunan" width="425"/>
<em>mainland_china - hunan</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_inner_mongolia_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_inner_mongolia" width="425"/>
<em>mainland_china - inner_mongolia</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_jiangsu_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_jiangsu" width="425"/>
<em>mainland_china - jiangsu</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_jiangxi_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_jiangxi" width="425"/>
<em>mainland_china - jiangxi</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_jilin_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_jilin" width="425"/>
<em>mainland_china - jilin</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_liaoning_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_liaoning" width="425"/>
<em>mainland_china - liaoning</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_ningxia_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_ningxia" width="425"/>
<em>mainland_china - ningxia</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_qinghai_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_qinghai" width="425"/>
<em>mainland_china - qinghai</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_shaanxi_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_shaanxi" width="425"/>
<em>mainland_china - shaanxi</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_shandong_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_shandong" width="425"/>
<em>mainland_china - shandong</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_shanghai_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_shanghai" width="425"/>
<em>mainland_china - shanghai</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_shanxi_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_shanxi" width="425"/>
<em>mainland_china - shanxi</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_sichuan_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_sichuan" width="425"/>
<em>mainland_china - sichuan</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_tianjin_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_tianjin" width="425"/>
<em>mainland_china - tianjin</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_tibet_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_tibet" width="425"/>
<em>mainland_china - tibet</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_xinjiang_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_xinjiang" width="425"/>
<em>mainland_china - xinjiang</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_yunnan_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_yunnan" width="425"/>
<em>mainland_china - yunnan</em>

<img src="./figure//forecast_arima_maxhorizon_7_mainland_china_zhejiang_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mainland_china_zhejiang" width="425"/>
<em>mainland_china - zhejiang</em>

<img src="./figure//forecast_arima_maxhorizon_7_thailand__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_thailand_" width="425"/>
<em>thailand - </em>

<img src="./figure//forecast_arima_maxhorizon_7_japan__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_japan_" width="425"/>
<em>japan - </em>

<img src="./figure//forecast_arima_maxhorizon_7_south_korea__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_south_korea_" width="425"/>
<em>south_korea - </em>

<img src="./figure//forecast_arima_maxhorizon_7_taiwan_taiwan_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_taiwan_taiwan" width="425"/>
<em>taiwan - taiwan</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_king_county_wa_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_king_county_wa" width="425"/>
<em>us - king_county_wa</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_cook_county_il_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_cook_county_il" width="425"/>
<em>us - cook_county_il</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_tempe_az_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_tempe_az" width="425"/>
<em>us - tempe_az</em>

<img src="./figure//forecast_arima_maxhorizon_7_macau_macau_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_macau_macau" width="425"/>
<em>macau - macau</em>

<img src="./figure//forecast_arima_maxhorizon_7_hong_kong_hong_kong_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_hong_kong_hong_kong" width="425"/>
<em>hong_kong - hong_kong</em>

<img src="./figure//forecast_arima_maxhorizon_7_singapore__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_singapore_" width="425"/>
<em>singapore - </em>

<img src="./figure//forecast_arima_maxhorizon_7_vietnam__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_vietnam_" width="425"/>
<em>vietnam - </em>

<img src="./figure//forecast_arima_maxhorizon_7_france__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_france_" width="425"/>
<em>france - </em>

<img src="./figure//forecast_arima_maxhorizon_7_nepal__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_nepal_" width="425"/>
<em>nepal - </em>

<img src="./figure//forecast_arima_maxhorizon_7_malaysia__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_malaysia_" width="425"/>
<em>malaysia - </em>

<img src="./figure//forecast_arima_maxhorizon_7_canada_toronto_on_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_canada_toronto_on" width="425"/>
<em>canada - toronto_on</em>

<img src="./figure//forecast_arima_maxhorizon_7_canada_british_columbia_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_canada_british_columbia" width="425"/>
<em>canada - british_columbia</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_los_angeles_ca_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_los_angeles_ca" width="425"/>
<em>us - los_angeles_ca</em>

<img src="./figure//forecast_arima_maxhorizon_7_australia_new_south_wales_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_australia_new_south_wales" width="425"/>
<em>australia - new_south_wales</em>

<img src="./figure//forecast_arima_maxhorizon_7_australia_victoria_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_australia_victoria" width="425"/>
<em>australia - victoria</em>

<img src="./figure//forecast_arima_maxhorizon_7_australia_queensland_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_australia_queensland" width="425"/>
<em>australia - queensland</em>

<img src="./figure//forecast_arima_maxhorizon_7_cambodia__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_cambodia_" width="425"/>
<em>cambodia - </em>

<img src="./figure//forecast_arima_maxhorizon_7_sri_lanka__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_sri_lanka_" width="425"/>
<em>sri_lanka - </em>

<img src="./figure//forecast_arima_maxhorizon_7_germany__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_germany_" width="425"/>
<em>germany - </em>

<img src="./figure//forecast_arima_maxhorizon_7_finland__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_finland_" width="425"/>
<em>finland - </em>

<img src="./figure//forecast_arima_maxhorizon_7_united_arab_emirates__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_united_arab_emirates_" width="425"/>
<em>united_arab_emirates - </em>

<img src="./figure//forecast_arima_maxhorizon_7_philippines__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_philippines_" width="425"/>
<em>philippines - </em>

<img src="./figure//forecast_arima_maxhorizon_7_india__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_india_" width="425"/>
<em>india - </em>

<img src="./figure//forecast_arima_maxhorizon_7_canada_london_on_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_canada_london_on" width="425"/>
<em>canada - london_on</em>

<img src="./figure//forecast_arima_maxhorizon_7_italy__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_italy_" width="425"/>
<em>italy - </em>

<img src="./figure//forecast_arima_maxhorizon_7_uk__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_uk_" width="425"/>
<em>uk - </em>

<img src="./figure//forecast_arima_maxhorizon_7_russia__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_russia_" width="425"/>
<em>russia - </em>

<img src="./figure//forecast_arima_maxhorizon_7_sweden__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_sweden_" width="425"/>
<em>sweden - </em>

<img src="./figure//forecast_arima_maxhorizon_7_us_santa_clara_ca_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_santa_clara_ca" width="425"/>
<em>us - santa_clara_ca</em>

<img src="./figure//forecast_arima_maxhorizon_7_spain__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_spain_" width="425"/>
<em>spain - </em>

<img src="./figure//forecast_arima_maxhorizon_7_australia_south_australia_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_australia_south_australia" width="425"/>
<em>australia - south_australia</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_boston_ma_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_boston_ma" width="425"/>
<em>us - boston_ma</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_san_benito_ca_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_san_benito_ca" width="425"/>
<em>us - san_benito_ca</em>

<img src="./figure//forecast_arima_maxhorizon_7_belgium__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_belgium_" width="425"/>
<em>belgium - </em>

<img src="./figure//forecast_arima_maxhorizon_7_us_madison_wi_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_madison_wi" width="425"/>
<em>us - madison_wi</em>

<img src="./figure//forecast_arima_maxhorizon_7_others_diamond_princess_cruise_ship_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_others_diamond_princess_cruise_ship" width="425"/>
<em>others - diamond_princess_cruise_ship</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_san_diego_county_ca_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_san_diego_county_ca" width="425"/>
<em>us - san_diego_county_ca</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_san_antonio_tx_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_san_antonio_tx" width="425"/>
<em>us - san_antonio_tx</em>

<img src="./figure//forecast_arima_maxhorizon_7_egypt__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_egypt_" width="425"/>
<em>egypt - </em>

<img src="./figure//forecast_arima_maxhorizon_7_iran__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_iran_" width="425"/>
<em>iran - </em>

<img src="./figure//forecast_arima_maxhorizon_7_us_omaha_ne_(from_diamond_princess)_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_omaha_ne_(from_diamond_princess)" width="425"/>
<em>us - omaha_ne_(from_diamond_princess)</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_travis_ca_(from_diamond_princess)_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_travis_ca_(from_diamond_princess)" width="425"/>
<em>us - travis_ca_(from_diamond_princess)</em>

<img src="./figure//forecast_arima_maxhorizon_7_australia_from_diamond_princess_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_australia_from_diamond_princess" width="425"/>
<em>australia - from_diamond_princess</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_lackland_tx_(from_diamond_princess)_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_lackland_tx_(from_diamond_princess)" width="425"/>
<em>us - lackland_tx_(from_diamond_princess)</em>

<img src="./figure//forecast_arima_maxhorizon_7_lebanon__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_lebanon_" width="425"/>
<em>lebanon - </em>

<img src="./figure//forecast_arima_maxhorizon_7_us_humboldt_county_ca_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_humboldt_county_ca" width="425"/>
<em>us - humboldt_county_ca</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_sacramento_county_ca_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_sacramento_county_ca" width="425"/>
<em>us - sacramento_county_ca</em>

<img src="./figure//forecast_arima_maxhorizon_7_iraq__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_iraq_" width="425"/>
<em>iraq - </em>

<img src="./figure//forecast_arima_maxhorizon_7_us_unassigned_location_(from_diamond_princess)_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_unassigned_location_(from_diamond_princess)" width="425"/>
<em>us - unassigned_location_(from_diamond_princess)</em>

<img src="./figure//forecast_arima_maxhorizon_7_oman__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_oman_" width="425"/>
<em>oman - </em>

<img src="./figure//forecast_arima_maxhorizon_7_afghanistan__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_afghanistan_" width="425"/>
<em>afghanistan - </em>

<img src="./figure//forecast_arima_maxhorizon_7_bahrain__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_bahrain_" width="425"/>
<em>bahrain - </em>

<img src="./figure//forecast_arima_maxhorizon_7_kuwait__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_kuwait_" width="425"/>
<em>kuwait - </em>

<img src="./figure//forecast_arima_maxhorizon_7_algeria__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_algeria_" width="425"/>
<em>algeria - </em>

<img src="./figure//forecast_arima_maxhorizon_7_croatia__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_croatia_" width="425"/>
<em>croatia - </em>

<img src="./figure//forecast_arima_maxhorizon_7_switzerland__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_switzerland_" width="425"/>
<em>switzerland - </em>

<img src="./figure//forecast_arima_maxhorizon_7_austria__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_austria_" width="425"/>
<em>austria - </em>

<img src="./figure//forecast_arima_maxhorizon_7_israel__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_israel_" width="425"/>
<em>israel - </em>

<img src="./figure//forecast_arima_maxhorizon_7_pakistan__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_pakistan_" width="425"/>
<em>pakistan - </em>

<img src="./figure//forecast_arima_maxhorizon_7_brazil__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_brazil_" width="425"/>
<em>brazil - </em>

<img src="./figure//forecast_arima_maxhorizon_7_georgia__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_georgia_" width="425"/>
<em>georgia - </em>

<img src="./figure//forecast_arima_maxhorizon_7_greece__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_greece_" width="425"/>
<em>greece - </em>

<img src="./figure//forecast_arima_maxhorizon_7_north_macedonia__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_north_macedonia_" width="425"/>
<em>north_macedonia - </em>

<img src="./figure//forecast_arima_maxhorizon_7_norway__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_norway_" width="425"/>
<em>norway - </em>

<img src="./figure//forecast_arima_maxhorizon_7_romania__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_romania_" width="425"/>
<em>romania - </em>

<img src="./figure//forecast_arima_maxhorizon_7_denmark__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_denmark_" width="425"/>
<em>denmark - </em>

<img src="./figure//forecast_arima_maxhorizon_7_estonia__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_estonia_" width="425"/>
<em>estonia - </em>

<img src="./figure//forecast_arima_maxhorizon_7_netherlands__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_netherlands_" width="425"/>
<em>netherlands - </em>

<img src="./figure//forecast_arima_maxhorizon_7_san_marino__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_san_marino_" width="425"/>
<em>san_marino - </em>

<img src="./figure//forecast_arima_maxhorizon_7_belarus__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_belarus_" width="425"/>
<em>belarus - </em>

<img src="./figure//forecast_arima_maxhorizon_7_canada_montreal_qc_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_canada_montreal_qc" width="425"/>
<em>canada - montreal_qc</em>

<img src="./figure//forecast_arima_maxhorizon_7_iceland__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_iceland_" width="425"/>
<em>iceland - </em>

<img src="./figure//forecast_arima_maxhorizon_7_lithuania__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_lithuania_" width="425"/>
<em>lithuania - </em>

<img src="./figure//forecast_arima_maxhorizon_7_mexico__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_mexico_" width="425"/>
<em>mexico - </em>

<img src="./figure//forecast_arima_maxhorizon_7_new_zealand__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_new_zealand_" width="425"/>
<em>new_zealand - </em>

<img src="./figure//forecast_arima_maxhorizon_7_nigeria__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_nigeria_" width="425"/>
<em>nigeria - </em>

<img src="./figure//forecast_arima_maxhorizon_7_australia_western_australia_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_australia_western_australia" width="425"/>
<em>australia - western_australia</em>

<img src="./figure//forecast_arima_maxhorizon_7_ireland__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_ireland_" width="425"/>
<em>ireland - </em>

<img src="./figure//forecast_arima_maxhorizon_7_luxembourg__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_luxembourg_" width="425"/>
<em>luxembourg - </em>

<img src="./figure//forecast_arima_maxhorizon_7_monaco__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_monaco_" width="425"/>
<em>monaco - </em>

<img src="./figure//forecast_arima_maxhorizon_7_qatar__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_qatar_" width="425"/>
<em>qatar - </em>

<img src="./figure//forecast_arima_maxhorizon_7_us_snohomish_county_wa_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_snohomish_county_wa" width="425"/>
<em>us - snohomish_county_wa</em>

<img src="./figure//forecast_arima_maxhorizon_7_ecuador__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_ecuador_" width="425"/>
<em>ecuador - </em>

<img src="./figure//forecast_arima_maxhorizon_7_azerbaijan__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_azerbaijan_" width="425"/>
<em>azerbaijan - </em>

<img src="./figure//forecast_arima_maxhorizon_7_czech_republic__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_czech_republic_" width="425"/>
<em>czech_republic - </em>

<img src="./figure//forecast_arima_maxhorizon_7_armenia__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_armenia_" width="425"/>
<em>armenia - </em>

<img src="./figure//forecast_arima_maxhorizon_7_dominican_republic__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_dominican_republic_" width="425"/>
<em>dominican_republic - </em>

<img src="./figure//forecast_arima_maxhorizon_7_us_providence_ri_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_providence_ri" width="425"/>
<em>us - providence_ri</em>

<img src="./figure//forecast_arima_maxhorizon_7_indonesia__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_indonesia_" width="425"/>
<em>indonesia - </em>

<img src="./figure//forecast_arima_maxhorizon_7_portugal__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_portugal_" width="425"/>
<em>portugal - </em>

<img src="./figure//forecast_arima_maxhorizon_7_andorra__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_andorra_" width="425"/>
<em>andorra - </em>

<img src="./figure//forecast_arima_maxhorizon_7_australia_tasmania_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_australia_tasmania" width="425"/>
<em>australia - tasmania</em>

<img src="./figure//forecast_arima_maxhorizon_7_latvia__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_latvia_" width="425"/>
<em>latvia - </em>

<img src="./figure//forecast_arima_maxhorizon_7_morocco__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_morocco_" width="425"/>
<em>morocco - </em>

<img src="./figure//forecast_arima_maxhorizon_7_saudi_arabia__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_saudi_arabia_" width="425"/>
<em>saudi_arabia - </em>

<img src="./figure//forecast_arima_maxhorizon_7_senegal__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_senegal_" width="425"/>
<em>senegal - </em>

<img src="./figure//forecast_arima_maxhorizon_7_us_grafton_county_nh_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_grafton_county_nh" width="425"/>
<em>us - grafton_county_nh</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_hillsborough_fl_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_hillsborough_fl" width="425"/>
<em>us - hillsborough_fl</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_new_york_city_ny_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_new_york_city_ny" width="425"/>
<em>us - new_york_city_ny</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_placer_county_ca_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_placer_county_ca" width="425"/>
<em>us - placer_county_ca</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_san_mateo_ca_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_san_mateo_ca" width="425"/>
<em>us - san_mateo_ca</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_sarasota_fl_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_sarasota_fl" width="425"/>
<em>us - sarasota_fl</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_sonoma_county_ca_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_sonoma_county_ca" width="425"/>
<em>us - sonoma_county_ca</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_umatilla_or_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_umatilla_or" width="425"/>
<em>us - umatilla_or</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_fulton_county_ga_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_fulton_county_ga" width="425"/>
<em>us - fulton_county_ga</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_washington_county_or_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_washington_county_or" width="425"/>
<em>us - washington_county_or</em>

<img src="./figure//forecast_arima_maxhorizon_7_argentina__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_argentina_" width="425"/>
<em>argentina - </em>

<img src="./figure//forecast_arima_maxhorizon_7_chile__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_chile_" width="425"/>
<em>chile - </em>

<img src="./figure//forecast_arima_maxhorizon_7_jordan__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_jordan_" width="425"/>
<em>jordan - </em>

<img src="./figure//forecast_arima_maxhorizon_7_us_norfolk_county_ma_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_norfolk_county_ma" width="425"/>
<em>us - norfolk_county_ma</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_berkeley_ca_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_berkeley_ca" width="425"/>
<em>us - berkeley_ca</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_maricopa_county_az_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_maricopa_county_az" width="425"/>
<em>us - maricopa_county_az</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_wake_county_nc_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_wake_county_nc" width="425"/>
<em>us - wake_county_nc</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_westchester_county_ny_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_westchester_county_ny" width="425"/>
<em>us - westchester_county_ny</em>

<img src="./figure//forecast_arima_maxhorizon_7_ukraine__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_ukraine_" width="425"/>
<em>ukraine - </em>

<img src="./figure//forecast_arima_maxhorizon_7_saint_barthelemy__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_saint_barthelemy_" width="425"/>
<em>saint_barthelemy - </em>

<img src="./figure//forecast_arima_maxhorizon_7_us_orange_county_ca_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_orange_county_ca" width="425"/>
<em>us - orange_county_ca</em>

<img src="./figure//forecast_arima_maxhorizon_7_hungary__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_hungary_" width="425"/>
<em>hungary - </em>

<img src="./figure//forecast_arima_maxhorizon_7_australia_northern_territory_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_australia_northern_territory" width="425"/>
<em>australia - northern_territory</em>

<img src="./figure//forecast_arima_maxhorizon_7_faroe_islands__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_faroe_islands_" width="425"/>
<em>faroe_islands - </em>

<img src="./figure//forecast_arima_maxhorizon_7_gibraltar__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_gibraltar_" width="425"/>
<em>gibraltar - </em>

<img src="./figure//forecast_arima_maxhorizon_7_liechtenstein__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_liechtenstein_" width="425"/>
<em>liechtenstein - </em>

<img src="./figure//forecast_arima_maxhorizon_7_poland__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_poland_" width="425"/>
<em>poland - </em>

<img src="./figure//forecast_arima_maxhorizon_7_tunisia__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_tunisia_" width="425"/>
<em>tunisia - </em>

<img src="./figure//forecast_arima_maxhorizon_7_us_contra_costa_county_ca_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_contra_costa_county_ca" width="425"/>
<em>us - contra_costa_county_ca</em>

<img src="./figure//forecast_arima_maxhorizon_7_palestine__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_palestine_" width="425"/>
<em>palestine - </em>

<img src="./figure//forecast_arima_maxhorizon_7_bosnia_and_herzegovina__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_bosnia_and_herzegovina_" width="425"/>
<em>bosnia_and_herzegovina - </em>

<img src="./figure//forecast_arima_maxhorizon_7_slovenia__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_slovenia_" width="425"/>
<em>slovenia - </em>

<img src="./figure//forecast_arima_maxhorizon_7_us_bergen_county_nj_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_bergen_county_nj" width="425"/>
<em>us - bergen_county_nj</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_harris_county_tx_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_harris_county_tx" width="425"/>
<em>us - harris_county_tx</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_san_francisco_county_ca_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_san_francisco_county_ca" width="425"/>
<em>us - san_francisco_county_ca</em>

<img src="./figure//forecast_arima_maxhorizon_7_south_africa__expost" width="425"/> <img src="forecast_arima_maxhorizon_7_south_africa_" width="425"/>
<em>south_africa - </em>

<img src="./figure//forecast_arima_maxhorizon_7_us_clark_county_nv_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_clark_county_nv" width="425"/>
<em>us - clark_county_nv</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_fort_bend_county_tx_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_fort_bend_county_tx" width="425"/>
<em>us - fort_bend_county_tx</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_grant_county_wa_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_grant_county_wa" width="425"/>
<em>us - grant_county_wa</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_queens_county_ny_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_queens_county_ny" width="425"/>
<em>us - queens_county_ny</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_santa_rosa_county_fl_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_santa_rosa_county_fl" width="425"/>
<em>us - santa_rosa_county_fl</em>

<img src="./figure//forecast_arima_maxhorizon_7_us_williamson_county_tn_expost" width="425"/> <img src="forecast_arima_maxhorizon_7_us_williamson_county_tn" width="425"/>
<em>us - williamson_county_tn</em>
