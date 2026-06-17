# Confirmed VWAP Pullback Futures Scripts

Files:

- `vwap_pullback_indicator.pine` is the discretionary charting/alert version.
- `vwap_pullback_strategy.pine` is the backtest version using `strategy.entry()` and `strategy.exit()`.

## 1. Strategy Logic

The system trades confirmed VWAP pullbacks on a 5-minute futures chart. A long requires price above session VWAP, bullish VWAP slope bias, a pullback into VWAP or the selected VWAP band, and a bullish rejection candle closing back above VWAP. A short mirrors that logic below VWAP. Optional EMA, Supertrend, higher-timeframe, volume, chop, distance, session, and max-signal filters gate the final signal. Signals only fire after `barstate.isconfirmed`.

## 2. Main Settings

Use the contract preset first, then switch `Use preset values` off if you want full manual control.

- VWAP: show/hide VWAP, show/hide bands, standard-deviation or ATR-offset bands, band multiplier, slope lookback, minimum slope.
- Trend: 50/200 EMA defaults, optional Supertrend, optional confirmed higher-timeframe EMA or VWAP bias.
- Volume: volume must exceed SMA volume times the configured multiplier.
- Rejection: strict or relaxed candle confirmation, minimum wick percent, body percent, and close location.
- Chop: ADX plus ATR compression filter.
- Distance: maximum ATR distance from VWAP, with optional tick and percent caps.
- Risk: stop beyond rejection candle high/low plus tick and optional ATR buffer, target by risk/reward.
- Session: default `0930-1555` in `America/New_York`; use a shortened session if you want forced flat before a market close.

## 3. Signal Grading

Grades are calculated from a quality score using VWAP side, VWAP bias, pullback touch, rejection quality, wick strength, volume, EMA, Supertrend, higher-timeframe bias, VWAP slope, chop, distance, and session state.

- `A+`: nearly all confirmations aligned.
- `A`: strong setup with most confirmations aligned.
- `B`: valid setup with moderate confirmation.
- `C`: valid but weaker or more marginal structure.

The strategy can trade only signals at or above the selected minimum grade. The indicator can also hide C-grade labels while still keeping the underlying logic available.

## 4. Alerts

Both scripts include `alertcondition()` entries for long entry, short entry, long exit, short exit, stop touched, target touched, opposite signal, A+ signal, A-or-better signal, and any valid signal.

For detailed dynamic messages with symbol, timeframe, direction, grade, entry, stop, target, risk/reward, and filter status, create a TradingView alert using "Any alert() function call" and keep `Use dynamic alert() messages` enabled.

## 5. Recommended Preset Behavior

- ES/MES: balanced VWAP band sensitivity, moderate volume filter, moderate ADX threshold, 6-tick stop buffer.
- NQ/MNQ: wider band and stop buffer, stronger ADX/chop requirement, tighter distance-from-VWAP control.
- CL/MCL: stronger rejection-candle requirement, volatility-aware distance, 8-tick stop buffer.
- GC/MGC: moderate volume filter, less aggressive chop threshold, ATR-aware behavior without over-filtering.

These are starting points, not optimized settings.

## 6. Backtesting Notes

Backtest one market at a time and use realistic tick value, point value, commission, slippage, and session assumptions. The strategy declaration uses a default `slippage=1` and `commission_value=0.00`; TradingView cost settings are controlled by the `strategy()` declaration or the Strategy Properties panel, so adjust those before trusting results.

Use enough sample size across trend days, range days, high-volatility days, and quiet sessions. Compare gross and net performance, maximum drawdown, average trade, profit factor, win rate, and distribution of A+/A/B/C outcomes.

## 7. Avoiding Overfitting

Do not optimize every input to one symbol and date range. Prefer broad, stable ranges for VWAP band multiplier, wick/body thresholds, ADX threshold, and risk/reward. Validate on out-of-sample dates and on nearby contracts such as ES vs MES or NQ vs MNQ. If a parameter only works in a tiny range, treat it as fragile.

## 8. Pine and TradingView Limitations

Indicators do not have broker-emulator position state, so the indicator tracks a virtual stop/target for visual alerts. If stop and target are both touched in the same candle, Pine cannot know the real intrabar order without lower-timeframe data.

Forced session exits occur when the script detects the session is no longer active. To exit earlier, shorten the session input, for example `0930-1550`.

Higher-timeframe values use `request.security()` with `lookahead=barmerge.lookahead_off` and previous confirmed higher-timeframe values. This avoids lookahead bias but means HTF confirmation naturally lags.

This is for analysis, education, and trade planning. It does not promise profitability and is not guaranteed to work live.
