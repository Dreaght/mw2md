# Maintaining Cargo.toml

The following applies to the root's and all the workspace members Cargo.toml.

## Rules

- You **SHALL** declare all the options in the order they are
  documented in the [Cargo Manifest Format](https://doc.rust-lang.org/cargo/reference/manifest.html).
- You **SHOULD NOT** declare the options with default values. *This helps
  to know that all the options have responsibly overridden values.*

### Lints

- You **SHALL NOT** disable / lower linter levels.
    - All the lints and categories **SHOULD** be declared in the order
      they are documented (see [Lint sources](#lint-sources)).
    - You **MAY** lower / disable it **only** in the following cases:
        1. **Two lints conflict each other**. Disable only **one** (_any_) of them.
            * You **SHOULD** choose to leave the most appropriate one.
    - Bypassing a lint check in the source code, you **MUST** include
      a [reason](https://doc.rust-lang.org/rustc/lints/levels.html#via-an-attribute:~:text=reason)
      attribute with a perfect justification.

#### Lint sources

1. [Rust Lint Groups](https://doc.rust-lang.org/rustc/lints/groups.html)
2. [Clippy Documentation](https://doc.rust-lang.org/clippy/)
3. [Rustdoc Lints](https://doc.rust-lang.org/rustdoc/lints.html)
