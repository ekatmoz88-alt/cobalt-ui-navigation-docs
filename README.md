# Cobalt UI Navigation

[English](README.md) | [Русский](README.ru.md)

[**Open the documentation website →**](https://ekatmoz88-alt.github.io/cobalt-ui-navigation-docs/)

Source-based technical documentation for the Cobalt UI Navigation C++ component.

## Overview

This repository documents the UI navigation abstraction implemented in the supplied Cobalt source excerpts.

The component provides a C++ wrapper around native UI navigation operations. Depending on the Starboard API version and platform interface availability, it uses either Starboard function pointers or local fallback implementations.

The documentation focuses on:

- Component architecture and native interface abstraction.
- Navigation item lifecycle and configuration.
- Focus management and callbacks.
- Container relationships and geometry-related operations.
- Queued updates and batch processing.
- API compatibility and fallback behavior.

## Documentation

| Document | Description |
|---|---|
| [Architecture](docs/architecture.md) | Component structure and execution paths |
| [Native Interface](docs/native-interface.md) | Native types and function-pointer interface |
| [Navigation Item](docs/nav-item.md) | NavItem lifecycle and method reference |
| [Focus Management](docs/focus-management.md) | Focus operations and callbacks |
| [Containers and Geometry](docs/containers-and-geometry.md) | Containers, windows, size, transforms and offsets |
| [Batch Updates](docs/batch-updates.md) | Queued operations and update processing |
| [Compatibility](docs/compatibility.md) | Version-dependent behavior and fallback implementation |
| [Source Evidence](references/source-evidence.md) | Mapping between documentation and source evidence |

## Architecture diagram

See [architecture.mmd](diagrams/architecture.mmd).

# Scope and evidence

This documentation is based on the C++ source excerpts and GYP configuration available for review.

It describes the wrapper implementation and the fallback behavior visible in those sources. It does not claim to describe every platform-specific implementation or the complete Cobalt runtime.

Platform-specific behavior, threading guarantees, coordinate units, transform conventions and callback invocation conditions require additional source code or platform documentation.

## Publication note

Before publishing, verify that the source material and all derived documentation are authorized for public distribution. Do not include proprietary source code, credentials, personal data or confidential platform details.


## Tags

`cpp` · `ui-navigation` · `component-architecture` · `api-reference` · `source-analysis`
