# ADR-001: Widget as a trait, not an inheritance hierarchy

**Status:** Accepted  
**Date:** 2024-01-01

## Context

GUI frameworks historically use deep inheritance trees (GTK's GObject, Qt's
QObject). Rust does not have inheritance; it has traits and composition.

## Decision

`Widget` is a `trait`. All widget types (`Button`, `Label`, `Container`)
implement it independently. `Container` holds `Vec<Box<dyn Widget>>` allowing
heterogeneous child lists.

## Consequences

- Any type can become a widget by implementing the trait (open-closed, Liskov
  substitution).
- No shared mutable base state — widgets own their own fields (encapsulation).
- Third-party crates can implement `Widget` without modifying this crate.
- `dyn Widget` carries a vtable overhead — acceptable for a UI event loop.
