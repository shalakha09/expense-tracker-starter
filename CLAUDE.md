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

The whole app is a single React 19 component (`src/App.jsx`) bootstrapped by `src/main.jsx` via Vite. State, derived totals, the add-transaction form, and the filtered transactions table all live in that one file. There is no router, no backend, and no persistence — transactions reset on reload because they're held in `useState` seeded with hard-coded data.

### Known intentional bug

`transactions[].amount` is stored as a **string** (both in the seed data and from the form's `<input type="number">`, whose `e.target.value` is a string). The income/expense reducers do `sum + t.amount`, which string-concatenates rather than sums. Any fix should coerce to a number at the boundary (form submission and/or the reducer) rather than just patching one site.
