# covid_19_forecast

Data provided by the Johns Hopkins University Center for Systems Science and Engineering (JHU CCSE) can be found here:
https://github.com/CSSEGISandData/COVID-19

The gretl code can be executed by the following steps
1) Clone the repo
2) open ./script/run.inp
3) Set your project path by setting the variable "DIR_WORK" accordingly
4) Execute

The script downloads the latest CCSE-data, processes the raw data and aggregates things for getting a single time-series of worldwide confirmed cases. ARIMA types of models will be fitted and used for computing out-of-sample forecasts.
