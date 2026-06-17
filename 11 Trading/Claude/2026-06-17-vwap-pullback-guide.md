# VWAP Pullback Pro — Usage Guide

**Files**
- `2026-06-17-vwap-pullback-indicator.pine` — visual indicator for live/discretionary trading
- `2026-06-17-vwap-pullback-strategy.pine` — strategy/backtest version

Two separate scripts are provided because TradingView keeps indicator and strategy contexts cleaner when split. The signal logic (sections 0–7) is identical in both, so what you see on the indicator is what the strategy trades.

> **Important:** This is for analysis, education, and trade planning. It is not financial advice, and no profitability is promised or implied. Backtest results are hypothetical. Manage your own risk and news.

---

## 1. Strategy logic in brief

The system trades pullbacks *in the direction of the intraday VWAP trend*, not random VWAP touches.

1. **Bias** — price must be on the correct side of session VWAP, and VWAP slope must not be flat (slope filter).
2. **Pullback** — price has to pull back into VWAP (or, optionally, the deviation band).
3. **Rejection** — the pullback must be rejected by a candle with real structure: meaningful body, a rejection wick on the correct side, and a close in the favorable part of the range, closing back on the trend side of VWAP.
4. **Environment gates** — chop filter (ADX + ATR compression) and distance-from-VWAP filter must pass; the bar must be inside your session.
5. **Confirmations** — EMA, Supertrend, higher-timeframe, and volume each run in `Off` / `Grade` / `Require` mode.
6. **Confirmation on close** — signals only fire on a closed bar (`barstate.isconfirmed`). Entries in the strategy fill on the next bar's open. Nothing repaints.
7. **Exit** — one stop (below/above the rejection candle ± buffer) and one target at a configurable R multiple. Optional exit on opposite signal and forced flat at session end.

---

## 2. Settings overview

**0) Contract Preset** — pick `ES/MES`, `NQ/MNQ`, `CL/MCL`, `GC/MGC`, or `Manual`. Turn on *Use Manual Overrides* to ignore the preset bundle and drive everything from the manual inputs. Presets only set a curated bundle (bands, RR, stop buffer, volume multiplier, ADX min, slope min, distance, candle thresholds, Supertrend settings) — every other input is always live.

**1) VWAP & Bands** — show/hide VWAP and bands; stdev band multiplier; session vs. day anchor; VWAP slope filter (lookback + min ticks/bar); distance-from-VWAP filter (xATR, optional tick cap).

**2) Core Indicators** — ATR, fast/slow EMA (50/200), Supertrend ATR + factor, ADX/DMI lengths, volume MA length.

**3) Trend & Volume Filters** — EMA, Supertrend, HTF, and Volume each have a mode:
- `Off` — ignored entirely.
- `Grade` — alignment counts toward the signal grade but does **not** block signals.
- `Require` — must align or no signal is produced (and it still counts toward grade).
EMA condition selectable: *Price > Slow EMA*, *Fast > Slow EMA*, or *Both*. HTF defaults to 15-minute, non-repainting.

**4) Chop Filter** — minimum ADX plus optional ATR-compression check (current ATR vs. its average). Blocks flat/directionless conditions.

**5) Rejection Candle** — Strict/Relaxed mode (Relaxed loosens thresholds 30%), min body %, min wick %, close-location threshold, and whether the pullback must reach the band or just VWAP.

**6) Session Filter** — enable/disable, custom session string, timezone, and force-flat at session end.

**7) Grading & Signals** — enable longs/shorts, minimum grade to show/trade, hide C-grade visually (indicator).

**7b) Max Signals/Session** — caps on long, short, and total signals per session to prevent overtrading.

**8) Exits** — stop buffer in ticks, optional ATR stop buffer, risk/reward multiple (default 1.5R), exit on opposite signal.

**Backtest Costs (strategy only)** — contracts per trade, commission per order, slippage in ticks.

**9/10) Visuals** — VWAP line, bands + fill, entry arrows, entry labels with grade + SL/TP, stop/target lines, bias background, dashboard table.

---

## 3. Signal grading

Each signal already passed all hard gates (VWAP side, pullback, rejection candle, chop, distance, session, and any `Require` filters). Grading then measures *how many of the graded confirmations align*, plus a bonus point for a strong-bodied candle:

```
score   = (aligned graded filters) + (strong candle ? 1 : 0)
maxScore = (graded filters) + 1
ratio   = score / maxScore
```

- **A+** — ratio = 1.0 with at least 3 graded confirmations.
- **A** — ratio ≥ 0.75 (or perfect with fewer than 3 graded filters).
- **B** — ratio ≥ 0.5.
- **C** — below 0.5.

If you set every confirmation to `Off`, grade falls back to candle quality (strong → A, otherwise B). Use *Minimum Grade* to suppress weak setups; *Hide C-Grade Signals* hides them on the chart only.

---

## 4. Alerts

The scripts fire `alert()` calls on confirmed bar close (non-repainting) with a rich message: symbol, timeframe, direction, grade, entry, stop, target, RR, and which filters passed. They also expose `alertcondition()` entries so you can pick them in TradingView's *Create Alert* dialog:

- Long Entry / Short Entry / Any Valid Signal
- A+ Signal, A or Better (indicator)
- Stop Loss Hit / Take Profit Hit / Opposite Signal (indicator)

To use the dynamic messages: create an alert on the script, choose the condition, and set the message to `{{strategy.order.alert_message}}` (strategy) or rely on the built-in `alert()` text. For grade-filtered alerting, use the **A+** or **A or Better** conditions.

---

## 5. Recommended default settings (presets)

All presets use 2.0 stdev bands, 1.5R, and the session/HTF defaults. The differences:

| Preset | Stop buf (ticks) | ATR stop (x) | Vol mult | Min ADX | Slope min (t/bar) | Max dist (xATR) | Min body | Min wick | ST ATR/Factor |
|---|---|---|---|---|---|---|---|---|---|
| **ES/MES** | 4 | 0.50 | 1.10 | 18 | 0.30 | 2.0 | 0.40 | 0.30 | 10 / 3.0 |
| **NQ/MNQ** | 8 | 0.75 | 1.10 | 22 | 0.40 | 1.5 | 0.40 | 0.30 | 10 / 3.5 |
| **CL/MCL** | 6 | 1.00 | 1.15 | 20 | 0.40 | 2.5 | 0.50 | 0.40 | 14 / 3.0 |
| **GC/MGC** | 5 | 0.75 | 1.05 | 15 | 0.30 | 2.5 | 0.35 | 0.30 | 10 / 3.0 |

Rationale: ES is balanced; NQ gets wider stops, stronger chop filtering and a tighter distance filter for its volatility; CL demands stronger rejection candles and uses a longer Supertrend ATR; GC avoids over-filtering (lower ADX min, looser body) with an ATR-aware stop. Treat these as starting points, not gospel — verify on your data.

---

## 6. Backtesting responsibly

- Use the **strategy** version on a 5-minute ES/MES, NQ/MNQ, CL/MCL, or GC/MGC chart.
- Set realistic **commission and slippage** (defaults: $2.50/order, 2 ticks). Futures fills are not free; thin pullback entries slip.
- TradingView's free/standard plans limit intraday history. Test on as much data as your plan allows and across **different regimes** (trending months, chop months, high- and low-volatility periods).
- Mind that entries fill on the **next bar's open** after a confirmed signal — this is intentional and non-repainting, but means your fill differs slightly from the signal candle's close.
- Check the **trade list**, not just net profit: win rate, average R, max drawdown, consecutive losers, and whether results depend on a handful of outlier trades.
- Compare in-sample vs. out-of-sample: tune on one date range, then validate on an untouched range.

## 7. Avoiding overfitting

- Prefer **fewer, robust filters** over stacking every filter on `Require`. If a setup only works with six filters perfectly aligned, it probably won't generalize.
- Don't optimize every input to a decimal. Round numbers that survive small perturbations are more trustworthy than a single magic value.
- Test **parameter neighborhoods**: if 1.5R works but 1.3R and 1.7R both collapse, the edge is fragile.
- Keep the same logic across instruments (the presets do this) rather than hand-fitting each symbol.
- Be suspicious of beautiful equity curves on small sample sizes. More trades across more conditions beats a perfect 30-trade backtest.

---

## 8. Pine Script / TradingView limitations

- **Next-bar fills.** A strategy entry triggered on a confirmed close fills at the next bar's open. This keeps the script non-repainting but means backtest fills won't equal the signal-candle close. The indicator labels show the signal-candle reference price; expect minor slippage live.
- **Intrabar fill assumptions.** When both stop and target could be hit inside one bar, TradingView's backtester makes assumptions about which came first. Real intrabar sequencing can differ. For more realism, enable bar-magnifier (paid) or test on lower timeframes.
- **Forced-flat timing.** Pine cannot look forward, so "force flat at session end" triggers on the first bar *after* the session closes rather than precisely at the bell. On futures with nearly continuous sessions, set your session window to the hours you actually trade.
- **HTF non-repainting cost.** The non-repainting higher-timeframe values use the *previous* confirmed HTF bar (`lookahead_on` + `[1]` offset). This is correct and stable but means HTF confirmation lags by up to one HTF bar.
- **VWAP anchor.** VWAP is computed manually with a running variance for the stdev bands and resets on the session (or day) anchor you choose. If your chart's session differs from the anchor session, set them consistently.
- **No news automation.** By design, there is no CPI/FOMC/NFP/inventory blackout logic. Manage news manually.
- **Object limits.** Labels/lines are capped (500 each). On very long histories older labels are dropped by TradingView — this is cosmetic only.

---

## 9. Files created

- `/Users/jkenney/Claude/2026-06-17-vwap-pullback-indicator.pine`
- `/Users/jkenney/Claude/2026-06-17-vwap-pullback-strategy.pine`
- `/Users/jkenney/Claude/2026-06-17-vwap-pullback-guide.md`
