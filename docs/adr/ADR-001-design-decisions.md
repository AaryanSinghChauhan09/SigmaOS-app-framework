# ADR-001: App Framework Design Decisions

**Date:** 2024-01-01  
**Status:** Accepted  
**Component:** SigmaOS-app-framework

---

## Context

SigmaOS desktop apps (file manager, text editor, settings) need a shared foundation to avoid duplicating widget, layout, theming, and IPC logic.

## Decisions

### 1. `Widget` trait as the composition root

**Rationale:** Composition over inheritance. Any struct that implements `Widget` can be placed in a container, composed into layouts, and queried for accessibility. No class hierarchy needed.

### 2. Accessibility is not optional

**Rationale:** Every `Widget` implementation must return an `AccessibilityNode`. This forces developers to think about accessible labels and roles at widget-creation time, not as an afterthought.

### 3. IPC via zbus D-Bus (in-process stub for tests)

**Rationale:** D-Bus is the Linux desktop IPC standard. zbus is a pure-Rust, async-native implementation. The `IpcChannel` stub enables headless testing without a session bus.

### 4. Layout primitives are `Widget` implementors

**Rationale:** `LayoutBox` and `LayoutGrid` implement `Widget`, so they can be nested arbitrarily. This is consistent with GTK4's widget tree model.

### 5. `AppContext` owns widget ID allocation

**Rationale:** Monotonically increasing IDs from a central context prevent collisions across widget trees in the same process.

## Consequences

- All widgets are testable without GTK or a display server.
- Accessibility nodes are structurally enforced at compile time.
- IPC bus is swappable (in-process for tests, D-Bus in production).
