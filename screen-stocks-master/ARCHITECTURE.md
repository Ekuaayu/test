# Screen Stocks Master Bot v7

Consolidated from the supplied bot trainer, GUI, trading bot, and StockManagerBridge.

## Design

- In-process C# BepInEx plugin is the live execution engine.
- Unity/game mutations are confined to the Unity main thread.
- Market ticks are processed in-process; there is no Python HTTP hop in the live execution path.
- Orders use a concurrent queue into a dedicated order worker, then a main-thread dispatch queue for game API calls.
- Forecasting uses the existing Holt damped-trend mathematics as the baseline model.
- Entry requires a thresholded forecast and bounded signal-strength confidence; open positions use hard-stop, trailing-stop and forecast-reversal exits.
- Auto/manual master control, F8 UI, F9 auto toggle, F10 emergency stop, and close-all are included.
- The web UI is local-only on port 8080.
- The supplied legacy Python GUI uses port 8090 and the supplied legacy bridge uses port 8080; v7 is intended to replace that split architecture.

## Latency priorities

1. Avoid Python/network round trips in the trade decision path.
2. Avoid blocking calls from tick processing.
3. Keep Unity API calls on the main thread.
4. Cache stock IDs after first discovery.
5. Keep high-frequency state in memory and persist asynchronously.
6. Keep UI polling separate from execution.
7. Use bounded histories and avoid per-tick allocations where practical.

## Model/training compatibility

The supplied historical trainer uses a 70/30 chronological split and an unseen-test acceptance gate. The first implementation should port that logic into a background C# trainer rather than changing the validation assumptions silently.

## Operational note

BepInEx plugins are compiled as DLLs and placed in the BepInEx/plugins directory.
