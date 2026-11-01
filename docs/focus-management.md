# Focus Management

## Overview

Focus operations are exposed through `NavItem` and forwarded through the native interface.

The implementation also coordinates focus requests with queued navigation changes.

## Focus request

`NavItem::Focus()` first checks whether the item is enabled.

- If the pending-update queue is empty, the focus operation is forwarded immediately.
- If updates are pending, the item is stored as the pending focus target.

The pending focus request is processed after queued changes have been executed.

## Deferred focus ordering

`ProcessPendingChangesLocked()` performs the following operations:

1. Executes queued closures in vector order.
2. Clears the pending-update queue.
3. Applies the pending focus target if it is valid.
4. Resets the pending focus value.

The source comment explains that focus should be applied after updates so that the target reflects the current UI state.

## Clearing focus

`UnfocusAll()` clears the pending focus target.

For builds satisfying `SB_API_VERSION >= SB_UI_NAVIGATION2_VERSION`, it also queues a call to `set_focus` with `kNativeItemInvalid`.

The version guard is important: this operation is not queued in the same way for older API configurations.

## Enabled state

Disabling an item is applied immediately. If the item is the current pending focus target, that pending request is cleared.

Enabling an item is queued.

## Callbacks

The callback wrappers forward native blur, focus and scroll notifications to the closures registered with the `NavItem`.

The reviewed sources do not specify callback timing, event ordering beyond the visible wrapper code, or the platform conditions that generate these notifications.
