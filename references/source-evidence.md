# Source Evidence and Documentation Boundaries

## Reviewed source material

This documentation is based on the following supplied source excerpts:

- `interface.h`
- `interface.cc`
- `nav_item.cc`
- The `ui_navigation` GYP target configuration

The complete upstream repository and all platform-specific implementations were not available for this review.

## Evidence map

| Topic | Source evidence |
|---|---|
| Native type aliases and compatibility types | `interface.h` |
| Native function-pointer contract | `interface.h` |
| Platform interface initialization | `interface.cc`, `InitializeInterface()` |
| Fallback implementation | `interface.cc`, fallback functions |
| Function-local static interface accessor | `interface.cc`, `GetInterface()` |
| Navigation item construction and destruction | `nav_item.cc`, constructor and destructor |
| Pending update queue | `nav_item.cc`, queue state and processing helpers |
| Focus ordering | `nav_item.cc`, `ProcessPendingChangesLocked()` and `Focus()` |
| Callback forwarding | `nav_item.cc`, `OnBlur`, `OnFocus`, `OnScroll` |
| Build target | `ui_navigation.gyp` excerpt |

## Confirmed implementation details

The supplied excerpts support the following statements:

- The component is implemented in the `cobalt::ui_navigation` namespace.
- The GYP target is a static library named `ui_navigation`.
- The target lists `interface.cc`, `interface.h`, `nav_item.cc` and `nav_item.h`.
- The implementation defines a function-pointer-based native interface.
- Starboard type aliases are used under the relevant API version guard.
- Platform interface retrieval is attempted for `SB_API_VERSION >= 12`.
- Local fallback functions are assigned when the platform interface is not used.
- Pending changes are stored as closures and processed in vector order.
- A deferred focus request is handled after pending changes.
- The fallback batch-update function invokes its callback directly.
- The fallback content-offset implementation stores values for container items.

## Details not established by the supplied excerpts

The following topics require additional source code, platform documentation or runtime verification:

- Platform-specific navigation algorithms.
- Actual focus-selection rules and directional navigation behavior.
- The conditions that trigger blur, focus and scroll callbacks.
- Coordinate units and geometry conventions.
- Transform matrix semantics.
- Thread affinity and complete synchronization guarantees.
- Window ownership and lifetime guarantees beyond the source comment.
- Runtime behavior of platform implementations behind Starboard function pointers.
- End-to-end UI behavior.

## Documentation policy

Statements in this repository are intentionally limited to the reviewed source evidence. Inferences are identified as such, and unknown implementation details are not presented as confirmed behavior.
