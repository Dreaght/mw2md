# Dependencies

## Statically linked

1) [parsoid](https://crates.io/crates/parsoid).
    - Convenience of Parsoid REST API access.
    - Not really trustworthy for enumerating all the possible HTML DOM tags and semantics.
2) [html-to-markdown-rs](https://crates.io/crates/html-to-markdown-rs).
    - Guarantees full CommonMark compliance.

## Runtime

1) [OPTIONAL] **Pandoc** - Universal markup converter.

* Hard-depending on this not exactly small (~200 MB+ uncompressed) package during
  runtime is questionable.
  Since the most of similar projects depended on it, so we do.
* Suitable to post-process the result.
