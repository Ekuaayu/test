# Master trading path

Unity StockManager
-> OnStockPriceChanged
-> O(1) online model update
-> confidence/risk gate
-> game cooldown gate
-> Buy/Short/Sell/Cover directly on StockManager

The critical order path intentionally avoids localhost HTTP, SSE parsing, Python scheduling, disk writes, JSON serialization and the web UI.

Background services:
- CSV batch writer
- localhost dashboard
- training worker
- state/portfolio refresh

Safety:
- AUTO OFF by default
- one pending order per stock
- global buy/short cooldown awareness
- re-entry delay
- trailing/reversal exits
- CLOSE ALL
- F10 emergency stop

The supplied Python trainer remains the reference for validated Holt parameter selection; the master live core is designed so a full 70/30 walk-forward trainer can be plugged into the background training worker without changing the low-latency order path.
