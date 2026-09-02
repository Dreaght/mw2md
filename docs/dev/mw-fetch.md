# MediaWiki Fetcher

Fetch MediaWiki rendered HTML and XML dumps.

## Responsibilities

**Goal**:

- Minimize responsibilities. The best code is the one that is never written.

**MUST**:

1. Obtain MediaWiki API endpoint.
    1. Directly as a CLI argument.
    2. Via API discovery mechanism (RSD).
2. Fetch Parsoid-spec compliant HTML. See [parsoid.md](parsoid.md) by depth specified.

**Gap**: Can we delegate the further processing to parsoid Rust crate? See [dependencies.md](dependencies.md).

**Gap**: How to handle depth?

**MAY**:

1. Cache* necessary, repetitive request results.

**MUST NOT**:

1. Modify given HTML or XML dumps.
2. Handle XML dump conversion into Parsoid-spec compliant HTML.
3. Store temporary files outside of cache*.

Cache* is a directory, according to the OS base directory specification, such
as [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir/latest/).

## Dependencies

What dependencies can solve these responsibilities?

...

## CLI Specification

Yet to be documented.

Draft:

```sh
# Use RSD (API discoverability) feature with depth two

mw-fetch --domain "www.example.wiki" --page "Title" --depth 2

# Use RSD, bypass redirects by providing a link to a real page

mw-fetch --page-link "https://www.example.wiki/wiki/Title"

# Specify API endpoint directly

mw-fetch --api "https://www.example.wiki/w/api.php" --page "Title"
```

## API Access

**RSD (API discoverability)**

...

## Tests

You **SHALL** mock REST API requests. Assume the structure of MediaWiki pages is the same.

**Gap**: Any specification guarantees pages HTML schema?

***

### RSD

Follow redirects.

Extract RSD link from an HTML header. Normalize URL if needed and validate attributes.

Fetch RSD XML document. Test read permissions.

Extract `<api>` XML tag. Validate attributes.

Extract `apiLink` URL. Validate it's a valid, normalized URL.

## Implementation

You **SHOULD** rely on [Dependencies](#dependencies). Do not reinvent a wheel.
