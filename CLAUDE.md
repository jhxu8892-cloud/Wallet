# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Wallet is a single-file, client-only expense tracker. The entire application — markup, styles, and logic — lives in [expense-tracker/index.html](expense-tracker/index.html). There is no build step, package manager, or test suite; it's plain HTML/CSS/vanilla JS meant to be opened directly in a browser.

## Running the app

Open `expense-tracker/index.html` directly in a browser (e.g. double-click it, or `start expense-tracker/index.html` on Windows). No server, install, or build step is required.

## Architecture

- All transaction state lives in a single in-memory `transactions` array, persisted to `localStorage` under the key `wallet_tx` via `save()`.
- `render()` is the single source of truth for the UI: it recomputes income/expense/balance totals and rebuilds the transaction list from `transactions` on every call. Any state mutation (add, delete, clear) must be followed by `save()` + `render()`.
- Transaction objects have the shape `{ id, desc, amount, category, type, date }`, where `type` is `'income'` or `'expense'` and `id` is `Date.now()`.
- User-provided text (`tx.desc`) is rendered via `escapeHtml()` before insertion into the DOM — preserve this when touching the rendering path to avoid reintroducing XSS.
- UI strings are in Spanish (`es` locale); keep new user-facing text consistent with that.
