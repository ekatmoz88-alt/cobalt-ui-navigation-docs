# Batch Updates

## Purpose

The implementation maintains a queue of pending navigation changes.

A source comment describes this queue as a mechanism for batching UI navigation changes so they can be applied as an atomic UI update.

## Queue state

The implementation defines shared state for pending changes:

- `g_pending_updates_lock` — an atomic lock value.
- `g_pending_updates` — a pointer to a vector of pending closures.
- `g_pending_focus` — a deferred native focus target.
- `g_nav_item_count` — the number of active navigation items tracked by this implementation.

The queue is initialized when the first `NavItem` is created.

## Queued operations

The reviewed implementation queues these operations:

- Enabling an item.
- Setting item size.
- Setting item transform.
- Setting a container item.
- Destroying an item.
- Clearing focus through the Navigation 2 API path.

Other operations, including direction, content offset and container-window association, are forwarded immediately.

## Processing sequence

`PerformQueuedUpdates()` invokes `do_batch_update` with `ProcessPendingChanges` as the callback.

The pending-change processor:

1. Executes each queued closure in insertion order.
2. Clears the queue.
3. Applies a valid pending focus target.
4. Resets the pending focus target.

This ordering ensures that the deferred focus request is processed after the queued operations.

## Batch behavior depends on the selected interface

When the Starboard Navigation 2 interface is available, the implementation uses the platform's `do_batch_update` function.

The fallback `DoBatchUpdate` implementation invokes the callback directly and synchronously.

Therefore, the fallback path does not establish the same platform-level atomic-update behavior described by the queue's source comment.

## Concurrency and threading limitations

The source contains locking and atomic state, but the reviewed excerpts are not sufficient to document a complete threading model, callback thread affinity or platform scheduling guarantees.
