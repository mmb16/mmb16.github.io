---
layout: post
title:      "Explaining the “Crypto” behind Cryptocurrency"
date:       2017-12-27 18:50:04 +0000
permalink:  explaining_the_crypto_behind_cryptocurrency
---


Cryptocurrencies are all over the news these days so I thought I’d go through a an overview of the underlying cryptography that makes it all possible.

The characteristics of cryptocurrencies can be broken down into the following:

* Immutable: Transactions cannot be undone or edited, and they get more secure as times passes. Cryptocurrencies (for the most part) are built on blockchain technology where blocks of transactions are stacked on top of each other. So the more blocks are on top of a single transaction the harder it becomes to undo because you would need to undo everything on top of it first.

[!](https://media.giphy.com/media/9K6cyrl0JO1wY/giphy.gif)

* Unhackable accounts: Your account is protected by a mathematical problem where you can create a problem that is easy to verify but extremely difficult to compute and near impossible with the current state of computing. This is what allows you to have a public key and a private key that are not hackable. Think of your public key as your account number. Everyone can see it everyone knows what it is but they have read only access. They cannot edit anything in your account like make a transaction without your password. Your private key is like the password that lets you login and it is a mathematical solution to you public key that cannot be reverse engineered. So for now they are hackable, advances in computing may one day make this obsolete but other more secure systems are being developed and will likely be found before computers can crack this code. For more information on the underlying math look at P= NP https://en.wikipedia.org/wiki/P_versus_NP_problem

* Decentralized: The blockchain is stored on a bunch of different computers around the world that are storing the history of all transactions and verifying new transactions. This makes it so the network has no single point of attack and if one person tries to fake a transactions that does not match the rest of the network they are simply ignored. The main vulnerability here comes in if 51% of the network is controlled by one party or person. This would allow them to do whatever they want at a very high cost. They would have more to lose than gain since they would be harming their 51% stake. If someone had a lot of money and computing power they could potentially take down the network at the current price of Bitcoin it may cost them nearly $500B and give them nothing in return. So there is no economic incentive to take the network.

[!](<iframe src="https://giphy.com/embed/xSTXAKCGsgAZG" width="480" height="360" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/anonymous-xSTXAKCGsgAZG">via GIPHY</a></p>)

* Anonymous: Since you are just a public key no one knows your real identity however they can see all the transactions from your account. Several cryptocurrencies focused on privacy such as Monero and ZCash hide your transactions which give you a greater level of anonymity. 

* Transparent: With the exception of the privacy focused cryptocurrencies the blockchain is completely transparent.  You can look up any public key and see every transaction they've done and what their balance is. So even though people outside the cryptocurrency community tend to associate them with shady dealings they are actually easier to track than cash and if embraced by a country could prove to be better at preventing money laundering.
