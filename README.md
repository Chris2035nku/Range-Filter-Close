# Range-Filter-Close
# Solana Trading Companion – Indicator Pack

This workspace collects the Pine scripts and supporting files I use to track Solana scalps on TradingView. The repo now ships with a three-layer range filter system, the adaptive Keltner + RSI exhaustion script, and other helpers that evolved out of earlier SBST experiments.

## Range Filter “Close” Suite

You can run three specialised versions of the range filter together to stage entries, confirmations, and exits.

| Script | Path | Default Settings | Role |
| ------ | ---- | ---------------- | ---- |
| **Range Filter Close 1 (Scout)** | `misc/range_filter_close_1.pine` | `Sampling Period = 34`, `Multiplier = 2.0`, Discount tap required for longs | Fires earliest signals right off the premium/discount zones. Use it to detect the start of a swing. |
| **Range Filter Close 2 (Enforcer)** | `misc/range_filter_close_2.pine` | `Sampling Period = 89`, `Multiplier = 2.8`, Premium tap enforced for shorts | Confirms trend direction and manages mid-trend trades. Holds longs for at least 3 bars before exiting. |
| **Range Filter Close 3 (Guardian)** | `misc/range_filter_close_3.pine` | `Sampling Period = 233`, `Multiplier = 4.2`, both premium/discount taps required | Rarely flips—treat its exit as the regime change or final take-profit call. Trailing multiplier widened to 1.0 for breathing room. |

All three copies inherit the same base features:

- ATR-based breakout detection with premium/discount zones.
- Trade-state tracker so exits only fire on active positions.
- Configurable exit filters (bearish close, close below filter, minimum bars in trade) to prevent premature exits. Close 1 enforces 2 bars, Close 2 uses 3, Close 3 uses 5 by default.

**How to use them**

1. Load all three scripts into the TradingView chart (5‑minute Solana by default).
2. Enter only when the Scout prints a signal while the Guardian still agrees with the trend.
3. Add or reduce once the Enforcer confirms.
4. Scale out when the Enforcer exits; flatten the remainder only when the Guardian fires or flips.

## Keltner + RSI Exhaustion

`misc/keltner_rsi_exhaustion.pine` overlays a Keltner channel with RSI-based momentum gating. Key features:

- Band proximity toggle: choose whether the candle must pierce the band or just come within a percentage of it.
- Higher-timeframe EMA bias, volume filter, and cooldown window to remove noisy flips.
- On-chart status panel showing which conditions are currently satisfied (“Band”, “RSI”, “Trend”, “Volume”, “Cooldown”).

This indicator pairs nicely with the range filters for timing exits at the top of a run.

## Other Scripts

- `misc/range_filter_buy_sell.pine`: original enhanced range filter with ATR trail, premium/discount zones, and exit safeguards.
- `misc/smi_precision_signals.pine`, `misc/tops_bottoms.pine`, etc.: legacy studies kept for reference.

## Getting Started

1. Copy the `.pine` script you want to use into a new TradingView indicator (Pine v5).
2. Save and add it to your chart.
3. Tweak the exposed inputs to match the market you’re trading. All default values are tuned for the SOL/USDT 5‑minute chart but respond well to other pairs with minor adjustments.

## Contributing / Tweaks

Open an issue or drop a PR if you have ideas for other filters, exit logic, or automation hooks. Each indicator is written to be modular, so you can bolt on additional confirmations or outputs without rewriting the core logic.

