# Architecture

## Purpose

The `cobalt::ui_navigation` component provides a C++ abstraction for UI navigation operations.

The `NavItem` class exposes item-level operations. These operations are forwarded through a `NativeInterface`, which contains function pointers for creating, configuring and destroying native navigation items.

The selected interface is initialized according to the Starboard API version and the availability of the platform navigation interface.

## Main components

### NavItem

`NavItem` is the C++ wrapper around a native navigation item.

Its responsibilities include:

- Creating and destroying native items.
- Managing enabled state and focus requests.
- Configuring direction, size and transform.
- Assigning container windows and parent items.
- Managing content offsets.
- Dispatching native callbacks to registered closures.
- Coordinating queued updates.

### NativeInterface

`NativeInterface` defines the operations available to the wrapper.

It is represented by a structure of function pointers, including operations for item creation, focus, geometry, containers and batch updates.

### Starboard interface

For supported API versions, the implementation attempts to obtain the platform interface through `SbUiNavGetInterface`.

When the platform interface is available, its function pointers are copied into the internal interface structure.

### Fallback implementation

When the required API version is unavailable or platform interface initialization fails, local fallback functions are assigned.

These functions provide limited behavior. They should not be interpreted as a complete platform navigation implementation.

## Initialization flow

1. `GetInterface()` accesses a function-local static interface.
2. The interface is initialized once.
3. For `SB_API_VERSION >= 12`, the implementation attempts to retrieve the Starboard UI navigation interface.
4. If retrieval succeeds, the available function pointers are copied.
5. Otherwise, fallback functions are assigned.
6. `NavItem` operations use the resulting interface.

## Update flow

Some `NavItem` operations are queued rather than applied immediately.

Queued operations are stored as closures. `PerformQueuedUpdates()` invokes the interface's batch-update operation with a callback that processes the pending changes.

The pending-change processor executes queued closures in insertion order and handles a deferred focus request after those changes.

## Design boundary

This repository documents the wrapper and fallback code visible in the reviewed sources. It does not specify the internal behavior of platform implementations behind the Starboard interface.
