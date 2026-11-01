# Compatibility and Fallback Behavior

## API version selection

The implementation uses compile-time API version checks.

### `SB_API_VERSION >= 12`

When this condition is met, internal native types are aliases for Starboard UI navigation types.

The implementation also attempts to retrieve the platform interface through `SbUiNavGetInterface`.

### `SB_API_VERSION < 12`

The header provides compatibility definitions for native items, item types, directions, matrices and callbacks.

The implementation selects local fallback functions instead of retrieving the Starboard interface.

### `SB_API_VERSION >= SB_UI_NAVIGATION2_VERSION`

For this version range, the implementation uses the platform `do_batch_update` function when the platform interface is successfully retrieved.

The `UnfocusAll()` implementation also includes a version-guarded path that queues a request to clear focus using the invalid native item value.

## Fallback implementation

The fallback functions visible in `interface.cc` have deliberately limited behavior.

| Operation | Fallback behavior |
|---|---|
| `CreateItem` | Allocates an `ItemImpl` and converts its pointer to `NativeItem` |
| `DestroyItem` | Deletes the `ItemImpl` |
| `SetFocus` | No operation |
| `SetItemEnabled` | No operation |
| `SetItemDir` | No operation |
| `SetItemSize` | No operation |
| `SetItemTransform` | No operation |
| `GetItemFocusTransform` | Returns `false` |
| `GetItemFocusVector` | Returns `false` |
| `SetItemContainerWindow` | No operation |
| `SetItemContainerItem` | No operation |
| `SetItemContentOffset` | Stores offsets only for container items |
| `GetItemContentOffset` | Reads stored offsets |
| `DoBatchUpdate` | Calls the supplied callback directly |

`ItemImpl` stores the item type, a spin lock and content-offset values initialized to zero.

## Callback limitation

The fallback `CreateItem` implementation does not store the callback structure or context. The reviewed fallback code therefore does not implement native callback dispatch.

The `NavItem` callback wrappers exist, but their presence does not establish that fallback items can generate native callbacks.

## Interpretation

The fallback path provides compatibility scaffolding and limited local state handling. It must not be described as a complete implementation of platform UI navigation.
