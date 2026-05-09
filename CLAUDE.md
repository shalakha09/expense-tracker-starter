# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- `npm run dev` — start Vite dev server (default at http://localhost:5173)
- `npm run build` — production build
- `npm run preview` — serve the production build locally
- `npm run lint` — run ESLint over the repo

There is no test runner configured.

## Architecture

This is the starter project for the [Code with Mosh "Claude Code" course](https://codewithmosh.com/p/claude-code). Per the README, it **intentionally ships with a bug, poor UI, and messy code** that are meant to be fixed throughout the course — keep that in mind before "cleaning up" code unprompted.

A React 19 app bootstrapped by `src/main.jsx` via Vite. There is no router, no backend, and no persistence — transactions reset on reload because they're held in `useState` seeded with hard-coded data.

### Component structure

- `src/App.jsx` — owns the canonical `transactions` list and the shared `categories` array, exposes an `addTransaction` handler, and composes the three child components below.
- `src/Summary.jsx` — receives `transactions`, computes income/expense/balance internally, and renders the summary cards.
- `src/TransactionForm.jsx` — owns its own form input state (description, amount, type, category). Receives `categories` and an `onAdd` callback; on submit it constructs the new transaction (coercing `amount` to `Number`) and hands it to `onAdd`.
- `src/TransactionList.jsx` — owns its own filter state (`filterType`, `filterCategory`). Receives `transactions` and `categories`, applies filters locally, and renders the table.

State lives as close to where it's used as possible: form-input state inside `TransactionForm`, filter state inside `TransactionList`. Only the canonical `transactions` list is lifted to `App`.

### Resolved intentional bug

The starter originally stored `transactions[].amount` as a string, causing the income/expense reducers (`sum + t.amount`) to string-concatenate. This is fixed: seed data uses numeric amounts, and `TransactionForm` coerces the form input via `Number(amount)` before calling `onAdd`. If you add a new entry point that creates transactions, coerce `amount` to a number there too.
