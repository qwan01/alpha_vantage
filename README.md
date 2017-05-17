# alpha_vantage

[![Build Status](https://travis-ci.org/RomelTorres/alpha_vantage.png?branch=master)](https://travis-ci.org/RomelTorres/alpha_vantage)

*Python module to get stock data from the Alpha Vantage API*

Alpha Vantage delivers a free API for real time financial data and most used finance indicators in a simple json format. This module implements a python interface to the free API provided by Alpha
Vantage (http://www.alphavantage.co/). It requires a free API, that can be requested on http://www.alphavantage.co/support/#api-key.

## Install
```shell
git clone https://github.com/RomelTorres/alpha-vantage.git
cd alpha-vantage
python setup install
```

## Usage
To get data in a python, simply import the library and call the object with your api key and get ready for some awesome free realtime finance data.
```python
from alpha_vantage.api_calls.timeseries import TimesSeries
ts = TimesSeries(key='YOUR_API_KEY')
# Get json object with the intraday data and another with  the call's metadata
data, meta_data = ts.get_intraday('GOOGL')
```
Internally there is a retries counter, that can be used to minimize connection errors (in case that the api is not able to respond in time), the default is set to
3 but can be increased or decreased whenever needed.
```python
ts = TimesSeries(key='YOUR_API_KEY',retries='YOUR_RETRIES')
```
Finally the library supports giving its results as json dictionaries (default) or as pandas dataframe, simply pass the parameter output_format='pandas' to change
the format of the output for all the api calls.
```python
ts = TimesSeries(key='YOUR_API_KEY',output_format='pandas')
```

## Plotting
Using pandas support we can plot the intra-minute value for 'MSFT' stock quite easily:

```python
from alpha_vantage.api_calls.timeseries import TimesSeries
import matplotlib.pyplot as plt

ts = TimesSeries(key='YOUR_API_KEY', output_format='pandas')
data, meta_data = ts.get_intraday(symbol='MSFT',interval='1min', outputsize='full')
data['close'].plot()
plt.title('Intraday Times Series for the MSFT stock (1 min)')
plt.show()
```
Giving us as output:
![alt text](images/docs_ts_msft_example.png?raw=True "MSFT minute value plot example")

The same way we can get pandas to plot technical indicators like
bollinger Bolliger Bands®

```python
from alpha_vantage.api_calls.techindicators import TechIndicators
import matplotlib.pyplot as plt

ts = TechIndicators(key='YOUR_API_KEY', output_format='pandas')
data, meta_data = ti.get_bbands(symbol='MSFT', interval='60min', time_period=60)
data.plot()
plt.title('BBbands indicator for  MSFT stock (60 min)')
plt.show()
```
Giving us as output:
![alt text](images/docs_ti_msft_example.png?raw=True "MSFT minute value plot example")

## Tests

In order to run the tests you have to first export your API key so that the test can use it to run.
```shell
export API_KEY=YOUR_API_KEY
cd alpha-vantage
nosetests
```

## Documentation
To find out more about the available api calls, visit the alpha-vantage documentation at
http://www.alphavantage.co/documentation/

## Coming soon:
1. ~~Add basic functionality: 0.0.1~~
2. ~~Add retry in order to allow the calls to be retried in case of failure: 0.0.2~~
3. ~~Implement all functions described in the alpha vantage documentation 0.0.3~~
4. ~~Re-factor functions to have an unified method for accessing the api 0.1.0~~
5. Add pandas support through decorators 1.0.0
6. Add logging to tests to store api call duration 1.0.1
7. Add unit tests for all the function parameters in the module 1.0.2
