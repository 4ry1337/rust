---
related: "[[Rust]]"
---
# API Stability

When updating an API, these changes can break client code. Major changes (🔴) are **definitely breaking**, while minor changes (🟡) **might be breaking**.

## Crates

- 🔴 Making a crate that previously compiled for *stable* require *nightly*.
- 🔴 Removing Cargo features.
- 🟡 Altering existing Cargo features.

## Modules

- 🔴 Renaming / moving / removing any public items.
- 🟡 Adding new public items, as this might break code that does `use your_crate::*`.

## Structs

- 🔴 Adding a private field when all current fields are public.
- 🔴 Adding a public field when no private field exists.
- 🟡 Adding or removing private fields when at least one already exists (before and after the change).
- 🟡 Going from a tuple struct with all private fields (with at least one field) to a normal struct, or vice versa.

## Enums

- 🔴 Adding new variants; can be mitigated with an early `#[non_exhaustive]`.
- 🔴 Adding new fields to a variant.

## Traits

- 🔴 Adding a non-defaulted item, breaks all existing `impl T for S {}`.
- 🔴 Any non-trivial change to item signatures, will affect either consumers or implementors.
- 🔴 Implementing any "fundamental" trait, as *not* implementing a fundamental trait was already a promise.
- 🟡 Adding a defaulted item; might cause dispatch ambiguity with another existing trait.
- 🟡 Adding a defaulted type parameter.
- 🟡 Implementing any non-fundamental trait; might also cause dispatch ambiguity.

## Inherent Implementations

- 🟡 Adding any inherent items; might cause clients to prefer that over a trait fn and produce a compile error.

## Signatures in Type Definitions

- 🔴 Tightening bounds (e.g., `<T>` to `<T: Clone>`).
- 🟡 Loosening bounds.
- 🟡 Adding defaulted type parameters.
- 🟡 Generalizing to generics.

## Signatures in Functions

- 🔴 Adding / removing arguments.
- 🟡 Introducing a new type parameter.
- 🟡 Generalizing to generics.

## Behavioral Changes

- 🔴 / 🟡 Changing semantics might not cause compiler errors, but might make clients do the wrong thing.

---
Source: [cheats.rs - Coding Guides](https://cheats.rs/#coding-guides) · [API Evolution RFC](https://rust-lang.github.io/rfcs/1105-api-evolution.html)
