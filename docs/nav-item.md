# Navigation Item

## Overview

`NavItem` is the C++ wrapper for a native UI navigation item in the `cobalt::ui_navigation` namespace.

The constructor creates a native item through `GetInterface().create_item()` and associates it with a callback structure and the `NavItem` instance.

## Lifecycle

### Construction

The constructor:

1. Stores the callback closures and item type.
2. Creates a native item through the selected native interface.
3. Initializes the pending-update queue when the first navigation item is created.

### Destruction

The destructor:

1. Disables the item.
2. Clears the pending focus request if it refers to the item being destroyed.
3. Queues native item destruction.
4. Processes pending changes and releases the pending-update vector when the last item is destroyed.

The implementation therefore coordinates native destruction with pending updates.

## Method reference

| Method | Behavior visible in the implementation |
|---|---|
| `PerformQueuedUpdates()` | Passes the pending-change processor to `do_batch_update` |
| `Focus()` | Requests focus when the item is enabled; may defer the request |
| `UnfocusAll()` | Clears pending focus and, under the Navigation 2 API guard, queues an invalid focus item |
| `SetEnabled()` | Queues enabling; disabling is applied immediately |
| `SetDir()` | Forwards the direction immediately |
| `SetSize()` | Queues the size update |
| `SetTransform()` | Queues a transform update |
| `GetFocusTransform()` | Forwards the focus-transform query and returns its result |
| `GetFocusVector()` | Forwards the focus-vector query and returns its result |
| `SetContainerWindow()` | Forwards the container-window association immediately |
| `SetContainerItem()` | Updates the stored container reference and queues the native association |
| `GetContainerItem()` | Returns the stored container reference |
| `SetContentOffset()` | Forwards the content offset immediately |
| `GetContentOffset()` | Forwards the content-offset query |

## Callback handling

The class defines callback wrappers for:

- `OnBlur`
- `OnFocus`
- `OnScroll`

Each wrapper converts the callback context to a `NavItem` pointer, checks whether the corresponding closure is present and invokes it.

The reviewed code establishes the callback forwarding mechanism. It does not establish the platform conditions or user actions that trigger each callback.

## Immediate and queued operations

Not every method follows the same update strategy. See [Batch Updates](batch-updates.md) for the operation queue and processing order.
