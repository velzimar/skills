# Temporal

Time is the `Temporal` API everywhere, and the current time comes from an injected clock.

## Rule

- Use `Temporal` throughout (built into Node 26 and later). `Date` appears only at the database boundary, converted in one file under `src/core/database/`.
- Calendar dates are `Temporal.PlainDate`, stored as a database `date` and sent as `YYYY-MM-DD` strings; instants are `Temporal.Instant`.
- Read the current time from an injected `Clock`, never `Temporal.Now` in business code, so tests can fix it.
- The web app loads `temporal-polyfill` when the browser has no `Temporal`.

## Why

`Date` mixes instants, calendar dates and the machine's time zone in one mutable type, the source of off-by-a-day bugs; `Temporal` gives each its own type, so a date can't be mistaken for an instant. Keeping `Date` to one boundary file keeps that guarantee everywhere else. An injected clock makes every time-based rule (expiry, windows, "today") testable at its exact edges.
