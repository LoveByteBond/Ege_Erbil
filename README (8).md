# Production Trading Systems — Portfolio

**Ege Erbil** · Algorithmic Trader → Quantitative Developer
Ankara, Türkiye · [LinkedIn](https://www.linkedin.com/in/ege-erbil)

I design, build, and operate production algorithmic trading systems across three venues:
**Borsa Istanbul (VIOP)** derivatives, **CME** futures & options, and **Binance** USDT-M
perpetuals. I own the full lifecycle: strategy research, signal validation, execution
engineering, risk controls, and live operation.

This repository documents a selection of those systems.

> **What this repo deliberately is not.** No source code, no signal logic, no parameter
> values, and no performance claims. These are production tools; the strategies inside
> them are proprietary. What a portfolio can honestly show is documented here instead:
> system design, execution safety, and research discipline. All screenshots were taken
> in paper/disconnected mode.

## Systems

| System | Venue | One-liner |
|---|---|---|
| [Delta Hedger](docs/delta-hedger.md) | CME · IBKR | Multi-underlying options-delta auto-hedger with broker reconciliation and hard risk gates |
| [Slow-Paste MM Station](docs/slow-paste-mm-station.md) | CME · IBKR | Spread-quoting station across the CME index/metals universe with a TD-SAFE exit engine |
| [Joint IB Station](docs/joint-ib-station.md) | CME · IBKR | Dual-engine strategy station with an integrated backtest → assess → forward pipeline |
| [Proj-Leg Station](docs/proj-leg-station.md) | CME · IBKR + Binance | Projection-strategy station with per-row live scanner and exchange-truth position guards |
| [VIOP Options Toolkit](docs/viop-options-toolkit.md) | Borsa Istanbul | Options risk, Greeks synthesis, and official-bulletin delta/P&L reporting for VIOP |
| [Research Infrastructure](docs/research-infrastructure.md) | — | Pre-registered backtesting, invariant sweeps, deterministic selftest batteries |

## Engineering doctrine

Every station follows the same house rules, developed in production:

- **Single-file deployables.** Each station is a self-contained Python application,
  packaged with PyInstaller where an `.exe` is needed. One artifact, no dependency drift.
- **GUI/async split.** Tkinter main thread for the operator, an asyncio worker for market
  data and order flow, a queue bridge between them. The UI never blocks execution and
  execution never blocks the UI.
- **ARMED is never persisted.** Every station boots disarmed, every time. Live trading is
  an explicit human action after restart — never a restored state.
- **Exchange truth wins.** Broker/exchange positions are the source of truth. Stations
  reconcile their internal book against it on connect and on demand, and surface drift
  instead of hiding it.
- **No silent fallbacks.** Missing data, failed feeds, and unexpected states fail loudly.
  Placeholder values are banned in production paths.
- **Deterministic selftest batteries.** Stations ship with embedded test suites that must
  pass before deployment — see [Research Infrastructure](docs/research-infrastructure.md).

## Stack

Python · Tkinter · asyncio · IBKR TWS API · Binance USDT-M API · pandas / NumPy ·
PyInstaller · Telegram Bot API · atomic JSON config/state

## Contact

The fastest route is [LinkedIn](https://www.linkedin.com/in/ege-erbil). Deeper technical
walkthroughs of any system here — architecture, failure modes, design trade-offs — are
available on request.
