---
title: NEAR Intents Provider lifecycle style
type: decision
status: confirmed
project: EllipalFinance-go
date: 2026-07-14
tags:
  - NEAR_INTENTS
  - provider
  - redis
  - preorder
---

# NEAR Intents Provider lifecycle style

- External Quote, status, deposit submit, and Token requests follow the provider request-log pattern. Request headers and secrets are excluded from logs.
- Rate calls persist elapsed time, request, response, and final rate through RateSwapRouterMongo.
- Order creation uses the standard singleflight key and 30-minute PreCreate duplicate guard. Mongo pre-order reuse retains the existing three-minute TTL and additionally checks order ID, status, saved full Quote signature, and required create fields.
- Token data caches raw /v0/tokens JSON in REDIS_KEY_EXCHANGE_PAIRS at NEAR_INTENTS/v0/tokens. Cache hits return immediately and refresh asynchronously; saveRedis=false bypasses the cache. Dynamic mapping/filtering occurs for every list build, and failed or empty data never replaces the last successful in-memory maps.
- HTTP endpoint transport methods stay in util.go; lifecycle orchestration stays in api.go; Token source/cache stays in redisTask.go; pair-map assembly stays in remoteTask.go.

Verification: local httptest validation, targeted go vet, go test ./..., go build ./..., and git diff --check passed. No real upstream was contacted.
