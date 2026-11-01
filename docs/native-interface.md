# Native Interface

## Overview

The internal `NativeInterface` structure defines the operations used by the UI navigation wrapper.

It is implemented as a collection of function pointers. The interface is initialized with Starboard functions when supported and available, or with local fallback functions.

## Native type definitions

For `SB_API_VERSION >= 12`, `interface.h` aliases the corresponding Starboard types:

| Internal type | Starboard type |
|---|---|
| `NativeItem` | `SbUiNavItem` |
| `NativeItemType` | `SbUiNavItemType` |
| `NativeItemDir` | `SbUiNavItemDir` |
| `NativeMatrix2x3` | `SbUiNavMatrix2x3` |
| `NativeMatrix4` | `SbUiNavMatrix4` |
| `NativeCallbacks` | `SbUiNavCallbacks` |

The header also aliases native constants for focus and container item types and the invalid item value.

For older API versions, compatibility definitions are provided locally:

- `NativeItem` is defined as `void*`.
- `NativeItemType` contains focus and container item types.
- `NativeItemDir` contains left-to-right and top-to-bottom directions.
- `NativeMatrix2x3` contains six floating-point values.
- `NativeMatrix4` contains sixteen floating-point values.
- `NativeCallbacks` contains `onblur`, `onfocus` and `onscroll` function pointers.
- The invalid native item is represented by `nullptr`.

## Function-pointer reference

| Function pointer | Responsibility visible in the interface |
|---|---|
| `create_item` | Create a native item |
| `destroy_item` | Destroy a native item |
| `set_focus` | Set the focused item |
| `set_item_enabled` | Change item enabled state |
| `set_item_dir` | Set item direction |
| `set_item_size` | Set item size |
| `set_item_transform` | Set item transform |
| `get_item_focus_transform` | Retrieve the focus transform |
| `get_item_focus_vector` | Retrieve the focus vector |
| `set_item_container_window` | Associate an item with a container window |
| `set_item_container_item` | Associate an item with another container item |
| `set_item_content_offset` | Set content offset |
| `get_item_content_offset` | Retrieve content offset |
| `do_batch_update` | Execute a batch-update callback |

The exact parameter types and signatures are defined in `interface.h`.

## Access

`GetInterface()` returns a constant reference to the initialized `NativeInterface`.

The implementation uses this accessor to forward operations from `NavItem` to the selected native interface.

## Important limitation

The function-pointer names describe the wrapper's interface contract. They do not, by themselves, establish platform-specific behavior, coordinate systems, units or implementation guarantees.
