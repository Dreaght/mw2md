# MediaWiki to Markdown

Convert MediaWiki knowledge base to garbage-free Markdown.

<!-- TOC -->

* [MediaWiki to Markdown](#mediawiki-to-markdown)
    * [Usage](#usage)
    * [Features](#features)
    * [Flavors support](#flavors-support)
    * [What it solves?](#what-it-solves)
    * [How this works?](#how-this-works)
    * [Alternatives](#alternatives)
    * [Contributing](#contributing)
    * [Licence](#licence)

<!-- TOC -->

---

## Usage

No specification yet. No public API is defined.

## Features

- [ ] **NO JUNK**. Strip garbage surrounding the human-written content.
- [ ] **NO CONFLICTS**. Choose what Markdown-flavor to target for proper rendering.
- [ ] **CRAWLER**. Includes any linked pages to the depth specified.
- [ ] ~~**XML dump conversion**. Export selected pages for conversion.~~ (Not _yet_ planned)

## Flavors support

| Flavor       | Supported | Test coverage |
|--------------|-----------|---------------|
| CommonMark   | Planned   | 0%            |
| GFM (GitHub) | Planned   | 0%            |
| Other        | Planned   | 0%            |

---

## What it solves?

- **Non-expanded garbage in the output**. One-to-one mapping from Wikitext to Markdown is difficult, as it's orders of
  magnitude more powerful/fine-grained than Markdown.
- **Lossy conversion** with semantic preservation. Extreme compression of knowledge bases.
- **Compatibility** with Markdown rendering engines.

## How this works?

**Internet access required!**

1) **Render into HTML first.**
   Request [Parsoid REST API](https://www.mediawiki.org/wiki/Parsoid/API#Wikitext_-%3E_HTML)
   for a complete, highly annotated HTML version of Wikitext.
2) **Normalize the HTML.** Clean up and refactor HTML tags for specs compliance.
3) **Finally convert into Markdown as is.**
   Use [Pandoc](https://github.com/jgm/pandoc) to convert the HTML to Markdown or a library.

More details in [ARCHITECTURE.md](ARCHITECTURE.md).

## Alternatives

| Tool                      | Lang    | Input[1*]        | Flavors[2*] | Stripped[3*] |
|---------------------------|---------|------------------|-------------|--------------|
| **mw2md** *this*          | Rust    | In-depth crawler | +           | **PROPERLY** |
| Pandoc [1]                | Haskell | Wikitext / HTML  | +           | -            |
| mediawiki-to-github [2]   | PHP     | XML dump         | GFM         | -            |
| mediawiki-to-gfm [3]      | PHP     | XML dump         | GFM         | -            |
| mediawiki-to-markdown [4] | Python  | XML dump         | Obsidian    | -            |
| wiki2md [5]               | C       | Wikitext         | -           | -            |
| markitdown [6]            | Python  | -                | -           | -            |

*Mostly operate on raw Wikitext, do not understand how to clean input and fix output.*
*This tool does*.

**Sources (links)**:

- [1] [Pandoc](https://github.com/jgm/pandoc)
- [2] [mediawiki-to-github](https://github.com/dbookstaber/mediawiki-to-github)
- [3] [mediawiki-to-gfm](https://github.com/outofcontrol/mediawiki-to-gfm)
- [4] [mediawiki-to-markdown](https://github.com/mak-kirkland/mediawiki-to-markdown)
- [5] [wiki2md](https://gitlab.com/oelmekki/wiki2md)
- [6] [markitdown](https://github.com/microsoft/markitdown)

**Meanings**:

- [1*] **Input.** Supports recursive crawling for scraping pages in-depth (using Parsoid).
- [2*] **Flavors**. The output complies with Markdown flavor specifications.
- [3*] **Stripped**. Strips the output for not having garbage surrounding the content.

---

## Contributing

Appreciated! Please, read [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## Documentation and questions

Read [documentation](docs/README.md). I will try to answer all your questions there.

## Licence

[MIT](LICENSE)
