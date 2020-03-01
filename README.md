# covid_19_forecast

Data provided by the Johns Hopkins University Center for Systems Science and Engineering (JHU CCSE) can be found here:
https://github.com/CSSEGISandData/COVID-19

The gretl code can be executed by two ways:
Option A:

	1) Clone the repo
	2) open ./script/run.inp
	3) Set your project path by setting the variable "DIR_WORK" accordingly
	4) Execute

Option B:

	1) Clone the repo
	2) Execute the shell-script run.sh


The script downloads the latest CCSE-data, processes the raw data for obtaining a clean panel data set.
Next, for each country-province combination two exercises are conducted:

	1) Expost forecasting analysis by restricting the training set to <CURRENT_DATE - MAX_HORIZON> where "CURRENT_DATE" refers to latest date for which data is available, and "MAX_HORIZON" is the set multi-step forecast horizon (default 7 days).
	2) Exante forecasting analysis for the forthcoming "MAX_HORIZON" days.

The chosen forecasting method is of the ARIMA(p,d,q)-type. The default is an ARIMA(2,1,1) model but the user can set parameters of choice. For all forecasting exercises, 90% forecast intervals will be computed.

# Up-to-date out-of-sample 7-days ahead forecasts

\n\n<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_anhui.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_beijing.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_chongqing.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_fujian.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_gansu.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_guangdong.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_guangxi.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_guizhou.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_hainan.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_hebei.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_heilongjiang.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_henan.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_hubei.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_hunan.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_inner_mongolia.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_jiangsu.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_jiangxi.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_jilin.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_liaoning.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_ningxia.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_qinghai.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_shaanxi.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_shandong.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_shanghai.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_shanxi.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_sichuan.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_tianjin.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_tibet.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_xinjiang.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_yunnan.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_mainland_china_zhejiang.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_thailand_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_japan_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_south_korea_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_taiwan_taiwan.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_us_seattle_wa.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_us_chicago_il.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_us_tempe_az.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_macau_macau.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_hong_kong_hong_kong.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_singapore_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_vietnam_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_france_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_nepal_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_malaysia_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_canada_toronto_on.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_canada_british_columbia.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_us_orange_ca.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_us_los_angeles_ca.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_australia_new_south_wales.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_australia_victoria.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_australia_queensland.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_cambodia_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_sri_lanka_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_germany_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_finland_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_united_arab_emirates_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_philippines_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_india_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_canada_london_on.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_italy_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_uk_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_russia_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_sweden_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_us_santa_clara_ca.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_spain_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_australia_south_australia.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_us_boston_ma.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_us_san_benito_ca.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_belgium_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_us_madison_wi.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_others_diamond_princess_cruise_ship.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_us_san_diego_county_ca.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_us_san_antonio_tx.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_egypt_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_iran_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_us_omaha_ne_(from_diamond_princess).png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_us_travis_ca_(from_diamond_princess).png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_australia_from_diamond_princess.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_us_lackland_tx_(from_diamond_princess).png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_lebanon_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_us_humboldt_county_ca.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_us_sacramento_county_ca.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_iraq_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_us_unassigned_location_(from_diamond_princess).png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_oman_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_afghanistan_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_bahrain_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_kuwait_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_algeria_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_croatia_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_switzerland_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_austria_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_israel_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_pakistan_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_brazil_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_georgia_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_greece_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_north_macedonia_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_norway_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_romania_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_denmark_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_estonia_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_netherlands_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_san_marino_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_belarus_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_canada_montreal_qc.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_iceland_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_lithuania_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_mexico_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_new_zealand_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_nigeria_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_australia_western_australia.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_ireland_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_luxembourg_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_monaco_.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_qatar_.png" width="425"/> <img src="./figures/forecast_arima_211_maxhorizon_7_us_portland_or.png" width="425"/>
<img src="./figures/forecast_arima_211_maxhorizon_7_us_snohomish_county_wa.png" width="425"/>
