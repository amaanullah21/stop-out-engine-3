# Risk Simulator Pro — Dynamic Trading Dashboard

> A browser-based trading risk simulator built with vanilla HTML, CSS, and JavaScript. No frameworks, no dependencies — runs entirely in the browser.

---

## What It Does

Risk Simulator Pro helps traders and risk analysts understand the real consequences of a trade **before placing it**. You input your trade parameters — entry price, lot size, leverage, account balance, and bonus type — and the dashboard instantly calculates whether your trade will survive or get stopped out at your projected price.

It simulates the broker-side margin engine, including bonus mechanics, strict equity modes, and stop-out logic — giving you a live, visual breakdown of your risk exposure.

---

## Live Demo

> [Risk Simulator Pro — Dynamic Trading Dashboard](https://amaanullah21.github.io/stop-out-engine-3/)

---

## Features

- **Real-time risk calculation** — every input change instantly recalculates all metrics
- **Buy & Sell (Long & Short) support** — direction-aware P/L and stop-out logic
- **Bonus system simulation** — supports Tradeable and Lossable bonus types
- **Strict Equity mode** — toggle to exclude bonus from equity calculations, simulating broker-side protection rules
- **Live price slider** — drag to simulate bullish or bearish price scenarios without changing your entry
- **Stop-Out price detection** — calculates the exact price at which your position would be liquidated
- **Floor price protection** — secondary stop-out threshold based on bonus floor logic
- **Visual margin gauge** — color-coded progress bar showing margin health at a glance
- **Trade status card** — instant Safe / Warning / Danger signal with plain-English explanation
- **Responsive layout** — adapts to mobile screens

---

## How to Use

1. **Open the link ["Risk Simulator Pro — Dynamic Trading Dashboard"](https://amaanullah21.github.io/stop-out-engine-3/) in any in any modern browser — no server required
2. **Fill in your trade parameters** in the left panel:
   - Trade direction (Buy or Sell)
   - Entry price and target price
   - Lot size and leverage (e.g. `1:100`)
   - Account balance and bonus amount
   - Bonus type and stop-out level
3. **Drag the price slider** to simulate different market scenarios
4. **Read the dashboard** on the right — it will tell you exactly what happens at your projected price

---

## Input Fields Explained

| Field | Description |
|---|---|
| **Trade Direction** | BUY (Long) profits when price rises; SELL (Short) profits when price falls |
| **Entry Price** | The price at which you open the trade |
| **Target Price** | Your projected or predicted exit price |
| **Live Price Slider** | Overrides the target price for scenario testing without editing the input |
| **Lots** | Number of standard lots being traded |
| **Leverage** | Broker leverage in `1:X` format (e.g. `1:100`) |
| **Account Balance** | Your deposited cash balance, excluding any bonus |
| **Bonus System** | Whether the bonus is Tradeable (counts toward margin) or Lossable (burns first) |
| **Strict Equity Toggle** | When ON (Tradeable bonus only), the broker ignores the bonus when calculating your equity, treating only the physical balance as real capital |
| **Bonus Amount** | Dollar value of any broker bonus added to your account |
| **Stop-Out %** | The margin level percentage at which the broker force-closes your position (commonly 50% or 100%) |
| **Contract Size** | Units per lot, as defined by your broker (commonly 1000 for metals, 100000 for forex) |

---

## Output Metrics Explained

### Status Card (top of dashboard)
The large card at the top gives you an immediate verdict:

- **Trade Will Survive** (green) — your position remains open at the projected price with healthy margin
- **Cannot Enter Trade** (orange) — your balance and bonus are insufficient to even open this position
- **Trade WILL hit Stop-Out** (red) — at your projected price, the broker will liquidate your position before you reach it

---

### Metric Boxes

**Projected Equity**
Your account equity at the projected price, calculated as:
- Strict mode OFF: `Balance + Bonus + Floating P/L`
- Strict mode ON: `Balance + Floating P/L` (bonus excluded from equity)

**Margin Level**
The ratio of your equity to your used margin, expressed as a percentage. A higher margin level means more breathing room. Your broker will stop you out when this drops to the Stop-Out % threshold.

**Used Margin**
The amount of your capital locked up as collateral to hold the position open. Calculated from lot size, contract size, entry price, and leverage.

**Distance to Stop-Out**
The number of price points (pips/ticks) between your projected price and the calculated stop-out price. A larger distance means you have more room before liquidation.

---

### Stop-Out Panel (lower section)

**Stop-Out Price**
The exact price at which the broker will force-close your position. The simulator calculates this using two thresholds and takes the more conservative one:

- **Margin % Threshold** — the price at which your margin level hits the broker's stop-out percentage
- **Floor Threshold** (Tradeable bonus only) — the price at which your equity hits either zero (Strict mode) or the bonus value (non-strict mode), whichever comes first

**Floating P/L**
The unrealised profit or loss at the projected price. Green means profit, red means loss.

**Reason Panel**
A plain-English explanation of why the trade survives or gets stopped out, and which threshold triggered the stop-out.

---

## The Math Behind It

### Margin Required
```
Margin = (Lots × Contract Size × Entry Price) / Leverage
```

### Floating P/L
```
P/L (Buy)  = (Current Price - Entry Price) × Lots × Contract Size
P/L (Sell) = (Entry Price - Current Price) × Lots × Contract Size
```

### Equity
```
Equity (normal) = Balance + Bonus + P/L
Equity (strict) = Balance + P/L
```

### Margin Level
```
Margin Level (%) = (Equity / Margin) × 100
```

### Stop-Out Price (Margin Threshold)
```
Stop Price = Entry + ((Target Equity - Capital Base) / (Lots × Contract Size × Direction))

where:
  Target Equity = Margin × (Stop-Out % / 100)
  Capital Base  = Balance (strict) or Balance + Bonus (normal)
  Direction     = +1 for Buy, -1 for Sell
```

### Stop-Out Price (Floor Threshold — Tradeable Bonus Only)
```
Floor Price = Entry + ((Floor Value - Capital Base) / (Lots × Contract Size × Direction))

where:
  Floor Value = 0 (strict mode) or Bonus Amount (non-strict mode)
```

The final stop-out price is whichever threshold is reached first — the higher price for a Buy trade, the lower price for a Sell trade.

---

## Known Limitations & Future Improvements

- Number formatting uses `toLocaleString()` without fixed decimal places — minor display inconsistency on some locales
- The price slider resets to the target price when other inputs change during active slider use
- No support for multi-position or hedged account scenarios

---

## Author

**Amaanullah Bhatti** — Junior Risk Analyst

---

## License

This project is for educational and simulation purposes. It does not constitute financial advice.
