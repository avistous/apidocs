# Getting Started Guide

There are three main things you can do with the REST api:

1. Get real time currency prices
1. Get historical currency prices and charts
1. Trade currencies, metals, and CFDs on OANDA forex trading accounts

Get real time currency prices
-----------------------------

Currencies, metals, and CFD prices change multiple times per second. To get a price, specify the instrument name you want to retrieve, for
example EUR/USD.  Replace the '/' character with an underscore '_' in currency pair names.  You can get a list of all the tradable
pairs with [this command]().

#### Example
Get the current price of EUR/USD

    http://api-sandbox.oanda.com/v1/instruments/EUR_USD/price

Response:

	{
		"instrument":"EUR_USD",
		"time":1354226333.464834,
		"bid":1.29786,
		"ask":1.29791
	}    

#### Sample Code
[Simple Rate Panel](https://github.com/oanda/simple-rates-panel) is written in Javascript and gives you the current price for a chosen currency pair.  Check out a live version [here](http://oanda.github.com/simple-rates-panel/simplepanel.html).

[Android Rates](https://github.com/oanda/AndroidRatesAPISample) is written in java, and uses the API to show current prices.

#### Reference
[Reference documentation](https://github.com/oanda/apidocs/blob/master/sections/reference.md#price-api-overview) for real-time currency prices.

Get historical prices and charts
--------------------------------

The API can also be used to get both current and historical candles for a variety of uses, including creating your own charts.

#### Example
Get 2 of the most recent candles for EUR/USD
	
    http://api-sandbox.oanda.com/v1/instruments/EUR_USD/candles?count=2

Response:

	{
		"instrument":"EUR_USD",
		"granularity":"S5",
		"candles":
		[
			{
				"time":1354226425,
				"openMid":1.29797,
				"highMid":1.29797,
				"lowMid":1.29797,
				"closeMid":1.29797,
				"complete":"true"
			},
			{
				"time":1354226450,
				"openMid":1.29795,
				"highMid":1.29795,
				"lowMid":1.29794,
				"closeMid":1.29794,
				"complete":"false"
			}
		]
	}

#### Sample Code
[Candle Average Price](https://github.com/oanda/cl-restapi-demo) is written in Lisp, and will calculate the average price of a currency pair over the past 'X' days.

#### Reference
[Reference documentation](https://github.com/oanda/apidocs/blob/master/sections/reference.md) for historical prices and charts.

Trade currencies, metals, and CFD's
-----------------------------------

To trade with OANDA, you first need to create a user.  A user owns accounts, and accounts are what hold the money you’re using to trade.  When you want to place a trade, you must specify the account containing the funds you wish to trade on.

What does this mean, you ask?  Simply [generate a user and an  account](http://oanda.github.com/gen-account.html).  You will be given a username and an account id.

* The username can be used to retrieve the account id owned by the user
* The account id is required as a parameter to any requests related to making trades or getting trade information

The username and password can, in most cases, be thrown away.  We wanted to make it easy for people to trading as quickly as possible in our sandbox, and so the API is not authenticated.  **The account id is the only thing you need to trade.**

If you want to [generate a user and an account](http://oanda.github.com/gen-account.html) yourself, you can follow [these steps](https://github.com/oanda/apidocs/blob/master/sections/trading_quick_start.md).

Opening a trade
---------------

#### Example
Open a long EUR/USD trade for 1000 units.  This example uses curl to submit three parameters using POST data.

    $ curl -X POST -d "instrument=EUR/USD&units=1000&direction=long" http://api-sandbox.oanda.com/v1/accounts/6531071/trades

Response:

	{
		"ids" : [
			177810427
		],
		"instrument" : "EUR\/USD",
		"units" : 1000,
		"price" : 1.29787,
		"marginUsed" : 64.8935,
		"direction" : "long"
	}

#### Sample Code
[Api Trading](https://github.com/oanda/py-api-trading) is written in Python, and demonstrates opening trades and orders.

#### Reference
[Reference documentation](https://github.com/oanda/apidocs/blob/master/sections/reference.md#trading-api-overview) for opening trades and orders.

Get existing open trades
------------------------

#### Example
Get the list of open EUR/USD trades for account 6531071.

    http://api-sandbox.oanda.com/v1/accounts/6531071/trades?instrument=EUR/USD

Response:

	{
		"trades" : [
			{
				"id" : 177810427,
				"units" : 1000,
				"direction" : "long",
				"instrument" : "EUR\/USD",
				"time" : 1354227034,
				"price" : 1.29787,
				"takeProfit" : 0,
				"stopLoss" : 0,
				"trailingStop" : 0
			},
			{
				"id" : 177810261,
				"units" : 4,
				"direction" : "long",
				"instrument" : "EUR\/USD",
				"time" : 1354221692,
				"price" : 1.29736,
				"takeProfit" : 0,
				"stopLoss" : 0,
				"trailingStop" : 0
			}
		],
		"nextPage" : "http:\/\/api-sandbox.oanda.com\/v1\/accounts\/6531071\/trades?instrument=EUR\/USD&maxTradeId=177810260"
	}

#### Sample Code
[Account Search](https://github.com/oanda/AccountSearchPHP) is written in PHP, and demonstrates listing an account's open trades and orders.

#### Reference
[Reference documentation](https://github.com/oanda/apidocs/blob/master/sections/reference.md#trading-api-overview) for opening trades and orders.

Get open positions
------------------

Open positions are an aggregated view of your open trades.  Rather than average out all of your open trades for a given currency pair, you can just ask for the position.

#### Example

Get a list of open positions for account 6531071.

    http://api-sandbox.oanda.com/v1/accounts/6531071/positions

Response:

	{
		"positions" : [
			{
				"direction" : "long",
				"instrument" : "EUR\/USD",
				"units" : 1004,
				"avgPrice" : 1.29787
			},
			{
				"direction" : "long",
				"instrument" : "USD\/CAD",
				"units" : 298,
				"avgPrice" : 0.99287
			}
		]
	}

#### Reference
[Reference documentation](https://github.com/oanda/apidocs/blob/master/sections/reference.md#trading-api-overview) for viewing open positions.


Get transaction history
-----------------------

Transaction history is a record of all activity on an account.  This includes things such as trades opened, orders triggered, and funds deposited.
