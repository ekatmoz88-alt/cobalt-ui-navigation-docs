# Containers and Geometry

## Overview

The `NavItem` interface exposes operations for item direction, size, transforms, container associations and content offsets.

This document describes only the forwarding and state-management behavior visible in the reviewed C++ implementation.

## Direction

`SetDir()` forwards the direction to `set_item_dir` immediately.

The compatibility definition in `interface.h` includes left-to-right and top-to-bottom direction values.

## Size

`SetSize()` queues a call to `set_item_size`.

The reviewed sources do not define the size units or the platform's interpretation of the supplied dimensions.

## Transform

`SetTransform()` queues a transform update.

The implementation copies the matrix into the queued closure and later forwards it through `set_item_transform`.

The internal compatibility definitions include 2x3 and 4x4 matrix types. The reviewed excerpts do not establish the transform convention, coordinate space or multiplication order.

## Container window

`SetContainerWindow()` forwards the supplied window immediately.

A source comment notes that the window lifetime is not guaranteed after the call. Callers must not infer that the wrapper retains ownership or extends the window lifetime.

## Container item

`SetContainerItem()` updates the stored C++ container reference immediately and queues the native association.

When the supplied container is null, the queued native operation uses the invalid native item value.

`GetContainerItem()` returns the stored reference.

## Content offset

`SetContentOffset()` forwards the offset immediately. The source comment explains that this is done because the offset may be queried immediately.

`GetContentOffset()` forwards the query to the native interface.

In the fallback implementation, content offsets are stored only for items whose type is `kNativeItemTypeContainer`. Access is protected by a spin lock.

## Unspecified details

The reviewed sources do not establish coordinate units, pixel density behavior, layout semantics, transform conventions or platform-specific geometry rules.
