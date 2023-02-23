# Vritti-Trading-Platform
A Crypto trading bot leveraging NLP\ML

**The Vision:** What outcome do we want to achieve?

+ A platform to quickly back-test strategies on historical data.
  + Starting with Crypto, Later expanding  to equity\FX\derivatives products
  + All Technical Analysis Patterns with provision to tune parameters.
  + Ability to define custom strategies easily or extend the existing code base.
  + Analyze price movement patterns across various assets to derive co-relation, to be used as input in the strategies. 
+ Leveraging NLP, using data from social media data such as Twitter, Reddit, etc.
  + Analyze social media and derive sentiment, Infuse the sentiment, (+ve / -ve) to influence trading strategies
+ Ability to backtest social media\news\influencers on historical data to find patterns that actually co-relate with price movements
+ Ability to paper trade and live trade based on backtested strategies
+ Extend the framework to incorporate deep reinforced learning. 

Other use cases:
+ Portfolio management 
+ Learning tool for day traders to hone their skills


The Architecture
================

Given the complexity and enormity of the desired outcome, microservices architecture is the chosen one!
The design should be flexible, scalable, and extendable.

The vision doesn't incorporate low latency\high frequency trading.

The proposed services:

1. **MarketData Service**
2. **Webservice for UI**
3. **NLP\\sentiment analytics Service**
4. **Algo\\TA\\ML service**
    

 Interconnectivity among these services is via REST API calls and Kafka for higher throughput data transfer.

###1.  MarketData Service:
    
This service is responsible for gathering historical and streaming quote and trade data from different sources. It organizes the data in databases for efficient retrieval for other services use. This service can have multiple instances to handle the capture of streaming data parallely, but writing to the same underlying DBs.
Choice of Technology:

1.  Databases: KDB+, or a combination of (Timescale DB and Redis)
2.  Fast API, Python
3.  Docker, Kubernetes
    

Market Data Source:
1.  CCXT - for consolidated feed 
2.  Individual exchange connectivity: example: Binance API
    

Messaging:
Kafka for streaming market data to UI\\Algo service

###2\. Webservice:

This is the central Command and Control service, which provides a user interface for backtesting, paper trading, using backtested strategies in live trading, managing market data, updating social media sources etc.

Choice of Technology: Django

###3\. NLP\\Sentiment analytic service:

This service is responsible for ingesting social media posts from sources such as Twitter, Reddit etc. It cleans, transforms, stores and derives sentiment value that can be used in making trading decisions. Should have the capability to run more than 1 instance for parallel ingestion and sentiment derivation.

Choice of Technology:

Fast API, Python, Timescale DB.

BERT language model: pre-trained model on financial feeds readily available, it can leverage CUDA architecture to run on NVIDIA GPUs.Explore GPT 3

###4\. Algo\\TA\\ML service:

This service is the Brain of this product. Capabilities include but are not limited to backtesting based on market data, backtesting based on sentiments, using various sources as input for the strategy executions, enabling paper-trading and live trading, risk management, pattern exploration, AI and machine learning capabilities.

This may need to be further split into more services each specializing in a specific area.

Choice of Technology: 
Fast API, Python, kafka, Redis, TA lib