# Extraction Log — SigmaOS-app-framework

## Source
Extracted from: `SigmaOS-desktop` monorepo, `lib/app-framework/` subtree.

## What Was Extracted

| Module | Source path | Notes |
|--------|------------|-------|
| widgets.rs | lib/app-framework/src/widgets.rs | Entry, ListBox, Dialog added |
| layout.rs | lib/app-framework/src/layout.rs | LayoutBox + LayoutGrid |
| ipc.rs | lib/app-framework/src/ipc.rs | zbus stub; in-process channel for tests |
| lib.rs core | lib/app-framework/src/lib.rs | Widget trait, Button, Label, Container |

## Decisions Made During Extraction

1. **Removed GTK widget pointers** — lib types are pure Rust; GTK wrappers are in a separate `sigma-gtk-widgets` crate (future).
2. **IpcChannel simplified to VecDeque** — real zbus integration is a wrapper; tests use the in-process channel.
3. **AppContext as ID allocator** — extracted from the global widget registry in the monorepo.

## Known TODOs

- [ ] zbus D-Bus channel adapter
- [ ] GTK4 widget bridge (`sigma-gtk-widgets`)
- [ ] Theme engine integration (CSS provider apply)
- [ ] State binding / reactive updates (signal/slot or channel-based)
- [ ] Async GLib event loop integration

## Date
2024-01-01
