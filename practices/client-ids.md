# Client ids, ready for sync

Offline sync isn't built, but the schema and the API are kept ready for it from the first table.

## Rule

- Ids are UUIDv7 from one `newId()` in `packages/domain`. Create requests accept an optional client `id`; creating the same `id` twice returns the existing row, unless it was deleted, in which case the `id` is refused: a deleted row never comes back. The check that an `id` is in use looks at every row, whatever its owner or deletion.
- User-owned tables carry `createdAt`, `updatedAt` and `deletedAt`. Delete by setting `deletedAt`; never hard-delete a user-owned row.
- User-owned service methods take `userId` as their first parameter, and every query on user-owned data filters by `userId` and `deletedAt: null`.
- Each table is written by exactly one service or repository.
- A change that writes several aggregates runs in one transaction, so it is stored whole or not at all. The service starts it through one shared helper in `src/core/`, and each repository it writes reads and writes through that helper's client, which joins the running transaction. A service that changes another aggregate as part of its own change calls that aggregate's service, never its repository.
- Business rules live in `packages/domain`, so the browser can run them too.

## Why

These cost little on day one, and retrofitting them later means changing every table, id and delete. A client-made id makes a create safe to repeat, so a retried or replayed request never makes a second row; UUIDv7 sorts by time, so it indexes well. Soft deletes let a sync tell a deleted row from one it hasn't seen. One writer per table keeps each rule in one place; one transaction across those writers keeps a change that spans them from being half stored when a write fails, and calling the other aggregate's service keeps its rules with it.
