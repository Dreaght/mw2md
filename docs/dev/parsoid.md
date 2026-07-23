# Interacting with Parsoid

[What is Parsoid?](https://www.mediawiki.org/wiki/Parsoid)

## API Access

- [MediaWiki REST API Reference](https://www.mediawiki.org/wiki/API:REST_API/Reference)
- ~~[Parsoid REST API](https://www.mediawiki.org/wiki/Parsoid/API)~~
  (*not guaranteed [1*], use MediaWiki REST API instead*)
- [API reference](https://doc.wikimedia.org/Parsoid-PHP/master/md_docs_2apiuse.html)
- [MediaWiki API Guidelines](https://www.mediawiki.org/wiki/API:Etiquette)

> **[1*]** On Wikimedia wikis, Parsoid's API is not accessible on the public Internet.
> On these wikis, you can access Parsoid's content via
> RESTBase's REST API (e.g.: https://en.wikipedia.org/api/rest_v1/ ).

**Gap**: How to reliably get `api.php` endpoint?

### RSD Protocol

MediaWiki implements [**Really Simple Discovery (RSD)**](https://en.wikipedia.org/wiki/Really_Simple_Discovery)
protocol. The API endpoint can be discovered by querying any page (or following redirects from the absolute URL
(domain)) and checking for
`<link>` HTML tag with `rel="EditURI"` attribute.

**PoC**:

```sh
curl -s -G -L "https://en.wikipedia.org/" | rg -e 'rel="EditURI"'
```

**Output**:

```
<link rel="EditURI" type="application/rsd+xml" href="//en.wikipedia.org/w/api.php?action=rsd">`
```

**Note**: It is not guaranteed that redirects from the absolute URL will lead to the main page. So this may require
fallbacks / workarounds, up to requiring the user to research the API endpoint himself.

We'll strictly define a supported subset of MediaWiki instances by default.

## HTML DOM

- [HTML/2.8.0 Specification](https://www.mediawiki.org/wiki/Specs/HTML/2.8.0)
- [Wikitext/1.0.0 Specification](https://www.mediawiki.org/wiki/Specs/wikitext/1.0.0)

**Based on**:

- [HTML5](https://en.wikipedia.org/wiki/HTML5)
- [RDFa](https://en.wikipedia.org/wiki/RDFa) (*might not pass strict validation*)
- Also has [data attributes](https://www.w3.org/TR/html-data-guide/)

### RDFa

**typeof** and **rel** attributes:

Multiple, space-separated values. Example:

1. `typeof="mw:Extension/TemplateStyles mw:Tranclusion"`
2. `rel="mw:ExtLink nofollow"`

### DOM Ranges

Attributes:

- `about` - Example: `#mwt1`. Links multiple elements into one group (range).
    - According to the
      [proposal](https://www.mediawiki.org/wiki/Parsoid/MediaWiki_DOM_spec/Template_Continuity#Range_runs_from_start_to_end):

      An algorithm **SHALL NOT** enumerate ranges as series of sibling nodes. But instead, in tree-order walk between
      unique **start** and **end**, inclusive.

      > The "**start**" node is the first node in tree-order with the appropriate `typeof`
      and `about` attributes, and the "**end**" node is the last node in tree-order with an `about` attribute matching
      the "**start**" node.

- `data-mw`: Schema is specific to the `typeof` value. It is a **JSON** object. The most useful one, a great fallback
  for unsupported HTML tags.

### Sections

The HTML body consists from `<section>` tags on the top level. Therefore, it makes sense to filter only them, as they
contain the nested content.

- [Specification](https://www.mediawiki.org/wiki/Specs/HTML/2.8.0#Headings_and_Sections)
- [Detailed design proposal](https://www.mediawiki.org/wiki/Parsing/Notes/Section_Wrapping)

Attributes:

- `data-mw-section-id`
    - Normally 0 or greater.
    - -1 and -2 are pseudo-sections.

Assumption:
`<section>` belongs either to the `<section>` or `<body>`.

### Content-only HTML tags

From the specification only these are documented:

- `<p>` - Text paragraph.
- `<i>` - _Italic_ text.
- `<b>` - **Bold** text.
- `<a>` (with `rel="mw:WikiLink"`) - Wikilinks.

What about the rest? These are not documented:

- [Full Wikitext markup](https://www.mediawiki.org/wiki/Help:Formatting)
- [Allowed HTML tags](https://www.mediawiki.org/wiki/Help:HTML_in_wikitext)
- [Tables](https://www.mediawiki.org/wiki/Help:Tables)

**Gap**: Find other content-relevant HTML tags to support.

- This is probably it: [Wikitext specification (1.0.0)](https://www.mediawiki.org/wiki/Specs/wikitext/1.0.0).

### Extension tags

> Content inside extension tags is not guaranteed to be MediaWiki DOM spec;
> it may be completely arbitrary HTML.

We can rely on `data-mw` attribute for whatever unsupported extensions.

## Source code

- [GitHub Mirror](https://github.com/wikimedia/mediawiki-services-parsoid/tree/master),
  ([original](https://gerrit.wikimedia.org/g/mediawiki/services/parsoid/))
