
# Exoity

### Python based trading strategy available for [MetaTrader 5](https://www.metatrader5.com/en)

---

## Strategy
The strategy used for this specific algorithm is quite simple to understand. After you specify a symbol and a
timeframe  the script will now determine if the current candle being created is on that specific timeframe is ```bearish``` or ```bullish``` . Whenever there is a bearish candle it will sell and whenever a candle is bullish it will buy. The position will be held until ```stop-loss```  or ```takep-rofit``` values are hit or if there is a new candle. The script will continue performing this procedure infinitely.

If some of the terms used previously were unclear, the following information might be useful
|  |  |
| ------------- |:-------------:|
| Candlestick charts     | [Understanding ___Candlestick Charts___](https://www.investopedia.com/trading/candlestick-charting-what-is-it/)     |
| Bullish candle    | Candle where ___close is higher than the open___     |
| Bearish candle      | Candle where ___close is lower than the open___     |
| Stop-loss      | Price where open position will close in ___loss___    |
| Take-profit      | Price where open position will close in ___profit___     |


![strategy image](https://i.ibb.co/YbLjgsj/Screenshot-2021-03-09-190621.png)

## Getting started
Make sure you have installed the dependencies of this project with ```pip install  -r requirements.txt```, have [MetaTrader 5](https://www.metatrader5.com/en) running with a trading account in Windows 10 and have the ```ipynb``` file opened in [Jupyter Notebook](https://jupyter.org/install)

____This bot can only be executed from [Windows 10](https://www.microsoft.com/en-us/software-download/windows10) OS___

## Trading conditions
The trading conditions section of the script is designed to tweak parameters and define new symbols.

>Although the strategy is currently applied for synthetic indexes, any financial instrument that can be traded trough MT5 can be added to this script.

In order to **add a new symbol**, append the following code to the **5th** input of the notebook.
```python
Symbol_name = Symbol(name="NAME_OF_SYMBOL_IN_MT5",
                       lot_size=LOT_SIZE_TO_BE_TRADED,
                       stop_loss=STOP_LOSS_POINTS,
                       take_profit=TAKE_PROFIT_POINTS)
```
This is the **example** code on how ***XAUUSD*** is added
```python
Gold = Symbol(name="XAUUSD",
                    lot_size=0.01,
                    stop_loss=500,
                    take_profit=300)
```
After the symbol is defined it has to be added to the ```SYMBOLS``` array on the **6th**  input of the notebook.
```python
SYMBOLS.append(Gold)
```

Changing the symbol the strategy is being executed on can be done from the **8th** input of the notebook.
```python
symbol_to_trade = Volatility_10_1s
timeframe = 60
close_order_deviation = 20
current_ea_comments = "Exoity V1.3"
```
```Volatility_10_1s``` is the selected symbol to run the strategy on  

```60``` the amount of minutes it will last from one price check to another  
in this specific case it will the **H1** timeframe  

```20``` is how far in points an order can differ from the actual price while  
being executed

```"Exoity V1.3"``` is the comments that will be registered on the MT5 transaction log
***
Possible timeframes

| Indicator | Minutes |
| ------------- |:-------------:|
| *M15* |   15   |
| *M30*| 30    |
| *H1*  |  60   |

***
## Testing strategy on historical data
Once a new symbol is added, backtesting of the strategy can be done on this new symbol in cells **13** to **16**. Which corresponds to the  **Testing the best symbol to run**  **the strategy on**  section of the notebook.

Executing cells **13** to **16** will test the strategy on the symbols included in the ```SYMBOLS``` array with the data from the dates specified in **cell 14** from the following timeframes **M15**, **M30** and **H1**

Changing testing dates can be done on **cell 14**
```python
date_from = dt(2019,1,1)
date_to = dt(2021,3,1)
```

The backtesting procedure is done by retrieving the data from the given dates for each mentioned timeframe and classifying each data point as ```bullish``` or ```bearish```. With the previous information the script will now iterate through the whole dataset and start to count how many times the **last two candles** of a given iteration are of the same type (_simulating a profitable trade_) and how many times these two candles are different (_simulating an unprofitable trade_). The **division** between the **quantity of profitable trades** and the **gross quantity of trades** will allow the calculation of the **percentage** of trades would've been profitable.

 In the scenario where **no** ```stop-loss``` and ```take-profit``` targets are placed, the **win-rate statistic is meaningless** as an unprofitable trade can represent a **greater loss** than a profitable trade.

Placing ```take-profit``` and ```stop-loss``` targets would allow us to **avoid** a situation where an **unprofitable trade represents more loss than a profitable trade**.

After performing the whole backtesting procedure information about the combination   of timeframes and symbols that have a win-rate superior to **50.41%** will be printed out in the following way ```SYMBOL at TIMEFRAME => WINRATE```.

After testing tradeable synthetic indexes available at the following [broker](https://www.binary.com/), the following results were obtained.

| Symbol | Timeframe | Win-rate |
| ------------- |:-------------:|:-------------:|
| Volatility 50 (1s) Index    | M30 | 50.41 %|
| Volatility 10 (1s) Index    | H1  | 50.45 %|
| Crash 500 Index | M15 | 51.02 %|
| Boom 1000 Index | M15 | 52.65 %|




The previous results would indicate that **historically**, the ___Boom 100 index___ at the ***M15*** timeframe would've been the most profitable symbol. Although this ***doesn't mean*** it will be the most profitable symbol in the future.


## Executing the strategy
In order to execute the strategy, the last **3** cells must be executed. The operations will occur based on the parameters given at the **Trading conditions** section of the notebook.



## Disclaimer

I am ***not a licensed advisor***. This  EA ***does not guarantee*** results of any kind and should be used with ***extreme caution***. The content disclosed here is done so with an educational purpose and ***NOT as investment
advice***.
