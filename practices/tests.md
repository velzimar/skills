# Tests

The API has integration tests only, through HTTP; the web app's critical flow has browser end-to-end tests; nothing has unit or component tests.

## Rule

- API tests go through HTTP against a real database (Testcontainers), with the whole app running. Only the edges are replaced: a capturing mailer (tests read what was sent from it) and a fixed clock. Our own services and repositories run for real.
- The web app's critical flow, the one a user cannot afford to have break, and any rule that lives only in the browser (such as client timeouts and retries) have browser end-to-end tests (Playwright). They drive the built web app as the in-process API serves it, find elements by role and accessible name, and run in a phone-sized viewport with reduced motion on. The browser's time is fixed at the fixed clock's instant and moves with it.
- The rest of the web app rests on typechecking against the contracts and trying it by hand.
- Name tests by behaviour, in the glossary's terms.
- Tests import business-rule constants from `packages/domain` instead of restating their numbers, arrange time and counts from them, and test the edges just inside and just outside each.

## Why

Tests at the highest seam check what a user or client sees, so they survive refactors that unit tests would break, and a refactor is exactly when tests matter. A real database catches what a mock hides (constraints, transactions, queries). Browser tests are slow and costly to keep, so they cover only what no API test can reach and whose breaking costs the most; the other screens are forms over rules the API tests already cover. Accepted cost: a slower loop, eased by running the test runner in watch mode so the database starts once.
