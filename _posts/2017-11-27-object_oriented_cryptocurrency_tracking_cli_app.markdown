---
layout: post
title:      "Object Oriented Cryptocurrency Tracking CLI App"
date:       2017-11-27 15:41:50 +0000
permalink:  object_oriented_cryptocurrency_tracking_cli_app
---

---

I had gotten into cryptocurrencies and blockchain a few years ago, this year bitcoin and cryptocurrencies have found their way into the mainstream with ICOs (initial coin offerings)happening constantly and the market-cap for cryptocurrencies growing to $250 Billion. More and more people are flocking to crypto as the prices sky rocket and more people start to learn about the power of blockchain (a decentralized ledger). Regulators and large banks that have historically seen it as a fad are now accepting it and even adding it as a product offering. With all that's going on I thought it would be fun to follow the markets and see where there is the most activity for altcoins (coins other than bitcoin and ethereum)



So I decided to start building a tracker to monitor the the volumes of cryptocurrencies. I built a basic command line interface that pulls data from Bittrex (one of the exchange with high volume and a wide variety of coins/tokens).
The app allows the user to sort the data by volume and displays the tickers, the volume,the bid price and the ask price.I’ve also included the ability to filter by a specific ticker.



[**Video Walkthrough**](https://www.youtube.com/watch?v=DpUTITVAgWU)

---

Before starting make sure to install rest-client 

`$gems install rest-client`

and require the necessary gems

```
require 'rest-client'
require 'json'
require 'pry'
```

* The first step in the program is pulling data from the API using RestClient. You can paste any API link from your exchange of choice or various exchanges to compare. I’m using Bittrex.com for this example since it has a high volume and wide variety of currency pairs (Binance.com works well too).

```
def self.get_markets
data = RestClient.get("https://bittrex.com/api/v1.1/public/getmarketsummaries")
@parsed_markets = JSON.parse(data)
```

* Then you’ll want to iterate through the hash to get to the data you’d like to use. For this example I’m pulling all the market summary data.

```
@parsed_markets.each do  |transaction|
if transaction.include?("result")
transaction.each do |coin_array|
if coin_array != "result"

coin_array.each do |coin_hash|
BankTracking::Currency.new(coin_hash)
```

* Now the program needs to create a new object for every currency pair and hold all the attributes and then store it in an array. Later we will be able to compare are the different instances stored in the class attribute `@@all`.

```
attr_accessor :Volume, :MarketName, :High, :Low, :Volume, :Last, :BaseVolume, :TimeStamp, :Bid, :Ask, :OpenBuyOrders, :OpenSellOrders, :PrevDay,
:Created, :DisplayMarketName

@@all = []

def initialize(currency)
#setter
currency.each {|key, value| self.send(("#{key}="), value)}
@@all << self
end
```

* Now we will create some class methods to sort and display the currency pairs by volume and search using the the ticker symbols. You edit this code to sort by any attribute by changing `Volume` to another attribute in the code below. Same idea applies for `#find_by_name` by changing `MarketName`.

```
def self.sort_by_volume
@@all = @@all.sort_by {|attribute| attribute.Volume}

@@all.reverse[0..10].each.with_index(1) do |name, i| 
puts "#{i}. #{name.MarketName} - Volume(000,000) #{name.Volume/100000} - Bid #{name.Bid} - Ask #{name.Ask}" end
end

def self.find_by_name(ticker)
@@all.each do |name| if name.MarketName.include? "#{ticker}"

puts "#{name.MarketName} - Volume(000,000) #{name.Volume/100000} - Bid #{name.Bid} - Ask #{name.Ask}"

end
end
end
```

* The last step is to build the CLI and call the methods we created previously to display the information.
The full source code can be found on [github](https://github.com/mmb16/bank_tracking_2)

This is the first in a series of steps to build a Machine Learning trading algorithm for the crypto markets by pulling data from various exchanges and using technical analysis on high volume liquid markets.
