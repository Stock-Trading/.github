# 📈 Stock Trading

**Stock Trading** is a training project by Piotr Grochowiecki ([@piotrgrochowiecki](https://github.com/piotrgrochowiecki)) built to explore **event-driven microservices architecture** through a realistic, working system: a real-time stock price alerting platform.

The idea: users set price alerts for financial assets (stocks, currencies, commodities). Multiple data loaders pull live prices from public financial APIs, publish price events to a message broker, and downstream services consume those events to evaluate alerts and persist data.

## 🏗️ System Architecture

![image](https://github.com/Stock-Trading/.github/assets/42697061/24e4e347-deea-41b8-9ea9-9ec47241398b)

**How it fits together:**

- **Portal** – user-facing app where logged-in users define price alert criteria for financial assets.
- **Data Loaders** – independently connect to public financial data APIs (e.g. [Alpha Vantage](https://www.alphavantage.co/), [Finnhub](https://finnhub.io/docs/api/websocket-trades)). In the initial phase, different loaders publish data for the same instrument (e.g. IBM) from different sources, to validate the event-driven design with multiple producers.
- **Message Broker** – decouples data loaders from consumers; all price events flow through here.
- **Data Consumer** – reads price events from the broker and cross-references them against user alert criteria stored in a relational database (PostgreSQL).
- **Data Writer** – persists high-volume raw price data for historical/analytical use (candidate for a NoSQL store).

## 📦 Repositories

| Repository | Purpose | Status |
|---|---|---|
| [`financialInstrumentsSubscriptionsManager`](https://github.com/Stock-Trading/financialInstrumentsSubscriptionsManager) | Manages subscriptions (Financial Instruments) assigned to Data Loaders. Data Loaders register and check in with this service; a round-robin load-balancing algorithm distributes instruments fairly across healthy, active loaders, and automatically rebalances when loaders go offline. Built with Java 25, Spring Boot 4, PostgreSQL, hexagonal architecture. | ✅ Actively developed, most mature service |
| [`data-loader-1`](https://github.com/Stock-Trading/data-loader-1) | A Data Loader implementation that connects to a public market data API, registers/checks in with the Subscriptions Manager, and publishes price events. | 🚧 In progress |
| [`message-broker-1`](https://github.com/Stock-Trading/message-broker-1) | Event broker configuration/infrastructure connecting Data Loaders to downstream consumers. | 📝 Scaffolding / not yet implemented |
| [`data-consumer-1`](https://github.com/Stock-Trading/data-consumer-1) | Consumes price events from the broker and evaluates them against stored user alert criteria (Postgres-backed). | 🚧 Early stage |

## ✅ What's done

- Overall system architecture and service boundaries defined.
- **Financial Instruments Subscriptions Manager** is functional: Data Loader registration, health-check (check-in) workflow, subscription assignment, and a documented round-robin load-balancing algorithm with automatic failure detection/recovery.
- Initial scaffolding for `data-loader-1` and `data-consumer-1`.

## 🚧 What's left to do

- Implement the **Message Broker** integration (`message-broker-1`) — currently just scaffolding.
- Finish `data-loader-1` to fully integrate with the Subscriptions Manager (register → check-in → fetch subscriptions → publish price events) and connect to a real public market data API.
- Build out `data-consumer-1` to consume events from the broker, match them against user-defined alert criteria, and trigger notifications.
- Implement the **Data Writer** service for persisting high-volume price data (NoSQL candidate) — not yet started.
- Build the **Portal** (user-facing frontend) for managing price alerts — not yet started.
- Add end-to-end integration across all services and observability (logging/metrics/tracing) for the full event-driven pipeline.

## 🎯 Learning goals

This project is primarily a learning exercise in:
- Event-driven architecture and asynchronous service communication
- Domain-Driven Design and hexagonal (ports & adapters) architecture
- Load balancing, health checking, and fault-tolerant distributed systems design
- Building and operating a multi-service system with Docker
