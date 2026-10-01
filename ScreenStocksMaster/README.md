# Screen Stocks MASTER Bot

A C#-native BepInEx master trading core for Screen Stocks.

## Latency design

- Market decisions are triggered directly from StockManager.OnStockPriceChanged.
- No Python/SSE/HTTP hop is present between a market event and a game order.
- The hot path uses O(1) online calculations and avoids disk/JSON/network I/O.
- CSV logging and the web server are background workers.
- Game cooldowns are read from the game's own state.
- Duplicate ticks, per-stock pending orders, attempt gaps and re-entry protection are enforced.
- AUTO is off by default.
- F8 opens the in-game UI, F9 toggles AUTO, F10 performs an emergency stop.

## UI

The master UI is available in-game and at localhost:8080. It exposes AUTO/MANUAL, training, CLOSE ALL, PANIC, cash/cooldowns, live stock scores and confidence.

## Source basis

The supplied v3 system used a Holt model, 70/30 validation, cooldowns, re-entry protection, an order worker and a web GUI. The master design moves latency-sensitive execution into the Unity process while keeping training/analytics off the critical path.

This repository branch contains the master architecture/documentation. The complete buildable package is supplied separately.
