# .github

Stock Trading is a training project where me, Piotr Grochowiecki [@piotrgrochowiecki](https://github.com/piotrgrochowiecki), learn about event driven architecture by building working system. 

Topic of the project is stock trading.

Architecture of the system's backend is presented below: 

![image](https://github.com/Stock-Trading/.github/assets/42697061/24e4e347-deea-41b8-9ea9-9ec47241398b)

Portal where logged users can set price alerts for different financial assets, e.g. stock, currencies, comodities. 
Data loaders connect to publicly available APIs providing financial information. 
In initial phase of the project, different data loaders publish data for one stock, e.g. IBM from different sources (Alpha Vantage, https://finnhub.io/docs/api/websocket-trades)
Data loaders publish events to event broker
Data consumer is connected to the database in which information about user's alerts criteria is stored (relational database, e.g. Postgres)
Data writer stores (a lot of data) in database (consider NoSQL database).
