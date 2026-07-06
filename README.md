# Personal Financial Data Platform

Real-time stock watchlist + personal bank transaction analytics, built as a set of
Spring Boot microservices on Kafka and OpenShift.

Two data domains:

1. **Market data** — live S&P 500 trades from the Finnhub WebSocket API, streamed
   through Kafka to a browser UI. Users build a personal watchlist (unlimited symbols)
   from the S&P 500 constituent list.
2. **Bank data** — CIBC and Canadian Tire Bank transactions ingested from CSV exports
   into PostgreSQL for querying and spending analytics.

## Architecture

```
Finnhub WS ──> market-data-service ──> Kafka (stock-trades, Avro) ──> stream-gateway ──SSE──> React UI
                                                                          │
Bank CSVs ──> bank-ingest-service ──> PostgreSQL <── watchlist-service <──┘ (REST)
```

## Roadmap

- [ ] Kafka broker up on CRC + CLI produce/consume smoke test
- [ ] `stock-trades` KafkaTopic CR + AKHQ deployed
- [ ] market-data-service: WS client → console → JSON producer → Avro
- [ ] Apicurio (AVRO) registry + schema registration
- [ ] stream-gateway SSE endpoint
- [ ] watchlist-service CRUD + S&P 500 data
- [ ] bank-ingest-service: CIBC parser → Postgres, then Canadian Tire Bank
- [ ] React UI: watchlist picker + live quotes
- [ ] Spending dashboard
- [ ] Later: `watchlist-events` cache invalidation, GitLab CI pipeline
