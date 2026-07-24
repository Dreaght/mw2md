# Compatibility

This tool **MUST** have layers of **graceful degradation**.

---

MediaWiki versions required for the following features:

| Feature             | Min  | Max    |
|---------------------|------|--------|
| RSD* (**OPTIONAL**) | 1.17 | latest |

**Sources**:

1. API discovery ~ RSD* Specification was [implemented in
   **1.17**](https://www.mediawiki.org/wiki/Release_notes/1.17#API_changes_in_1.17).

See [Deprecation Policy](https://www.mediawiki.org/wiki/API/Deprecation) for any updates.

---

## RSD

### Assumptions

A particular MediaWiki instance is compatible with **RSD** (API discoverability) feature as long as:

Any wiki page contains a header with `<link>` HTML tag with the following attributes:

1. `rel="EditURI"`
2. `type="application/rsd+xml"`
3. `href` with a __valid__* URL pointing to **RSD** document.

__Valid__* means it is accessible (200 code)
and [read permissions](https://www.mediawiki.org/wiki/API:Rsd#Possible_Errors) are given by default.

In the **RSD** document, at least one `<api>` XML tag with the following attributes:

1. `name="MediaWiki"`
2. `apiLink` with a valid URL pointing to the API endpoint.

More information about specification is [here](https://github.com/danielberlinger/rsd)
(or [original RFC](https://archipelago.phrasewise.com/rsd/)).

### Fallback

A user **MUST** be able to provide an API endpoint directly, saving HTTP requests.
