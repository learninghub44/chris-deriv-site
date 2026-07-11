# chris-deriv-site — Deriv Trading Engine

This repo is a trimmed-down, engine-focused fork of a white-label DTrader build
(`nelsonmpanju/traderschemepro-trader`, itself derived from `deriv-com/dtrader-template`).

It keeps only the packages needed to run the actual trading engine and its UI,
and drops the parts that duplicate functionality chris-tech-trader-pro already
has on its own (app shell, login/session handling, reports/statement pages).

## What's in here

| Package | Purpose |
|---|---|
| `packages/trader` | The engine: `Stores/Modules/Trading/trade-store.ts` (proposal/buy/barrier/duration/multiplier logic), `Helpers/contract-type.ts` (live `contracts_for`-driven contract type engine), and the `AppV2` mobile-first trading UI (market selector, trade-type selector, trade parameters, purchase button, chart containers). |
| `packages/shared` | Constants, the `WS` singleton (`services/ws-methods.ts`) and helpers the engine depends on. |
| `packages/stores` | Base mobx stores (client, common, UI) the trade-store composes with. |
| `packages/components` | The `@deriv/components` UI kit used throughout `AppV2`. |
| `packages/api`, `packages/api-v2` | The WebSocket API abstraction (`WS.send`, `WS.subscribe`, `contracts_for`, `active_symbols`, etc.). |
| `packages/utils` | Shared utility helpers. |

## What was dropped from the source repo

- `packages/core` — the app shell, OAuth2/PKCE login, account switching, menu.
  Not needed here since the target app (chris-tech-trader-pro) has its own.
- `packages/reports` — positions/profit-table/statement pages. Out of scope for
  "manual trading engine."

## Why this exists

The original ask: chris-tech-trader-pro's Manual Trading page had hardcoded
market/contract-type/duration/barrier lists instead of asking the live trading
engine what's actually available. This repo is the reference/source engine —
the real DTrader logic — being pulled out so it (or the patterns in it) can be
integrated into chris-tech-trader-pro properly, instead of hand-rolling a
lighter approximation.

## Status

This is a **restructuring checkpoint**, not a standalone runnable app yet — the
root-level app shell was removed, so `packages/trader`'s `AppV2` currently
expects to be mounted inside a shell like the one in `packages/core` (removed
here). Wiring this into chris-tech-trader-pro's own shell/WS layer is the next
step.
