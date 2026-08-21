# whalebot 🐋

A multi-indicator confluence trading strategy for [TradingView](https://www.tradingview.com/), written in Pine Script v6.

## Overview

`whalebot` is an algorithmic trading strategy that only enters a position when **multiple independent technical signals agree at the same time**. Instead of trading off a single indicator (which tends to produce a lot of false signals), it scores 9 separate signals every bar and only opens a trade when a configurable number of them line up in the same direction (8 out of 9 by default).

This confluence-based approach was inspired by how institutional/"whale" trades tend to align several confirming factors — trend, momentum, volatility, and volume before committing size, hence the name.

## How it works

On every bar, the strategy independently evaluates:

| # | Signal | What it measures |
|---|--------|-------------------|
| 1 | **ADX (Average Directional Index)** | Whether the market is trending strongly enough to trade (filter, not scored) |
| 2 | **Range Filter** | Smoothed price trend direction, filters out noise |
| 3 | **Parabolic SAR** | Short-term trend reversal points |
| 4 | **RSI** | Overbought / oversold momentum |
| 5 | **TWAP Trend** | Time-weighted average price bias (used as a directional filter) |
| 6 | **JMA (Jurik-style smoothed average)** | Lag-reduced trend direction |
| 7 | **MACD** | Trend/momentum crossover |
| 8 | **Volume Delta (CVD)** | Cumulative buy vs. sell volume pressure |
| 9 | **Volume Weight (RVOL)** | Whether current volume is significant relative to its moving average |
| 10 | **Dual Moving Average (5/30)** | Short vs. long-term trend |
| 11 | **MA Speed / Slope** | Rate of change of the trend average |

A **Long** signal fires when ADX confirms a strong uptrend, the TWAP filter is bullish, and at least `minSignals` (default 8 of 9) of the scored signals agree. A **Short** signal mirrors this on the downside.

## Risk management

Every trade is protected by:
- A **stop loss** (% based, default 1%)
- A **take profit** (% based, default 0.8%)
- An optional **trailing stop** (trigger + offset, both % based)
- Position sizing as a percentage of equity, with commission and slippage modeled in the backtest settings

## Usage

1. Open [TradingView](https://www.tradingview.com/), go to the Pine Editor.
2. Paste in [`whalebot.pine`](./whalebot.pine).
3. Add it to a chart as a **Strategy** to backtest, or adapt the entry logic into an alert-based indicator for live signals.
4. Tune the inputs (indicator lengths, `minSignals`, stop loss / take profit %) to the asset and timeframe you're trading.

## Tech

- **Language:** Pine Script v6 (TradingView's scripting language for indicators/strategies)
- **Type:** Strategy script (backtestable directly on TradingView, with built-in equity curve and performance report)

## Disclaimer

This project is for educational purposes. It is not financial advice, and past backtest performance does not guarantee future results. Trade at your own risk.
