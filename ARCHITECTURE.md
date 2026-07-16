# Architecture

<!-- TOC -->

* [Architecture](#architecture)
    * [Online export](#online-export)
        * [How can the data be extracted?](#how-can-the-data-be-extracted)
        * [How to normalize the data?](#how-to-normalize-the-data)
        * [How to convert to Markdown?](#how-to-convert-to-markdown)
    * [Offline conversion](#offline-conversion)
    * [Programming language(s)](#programming-languages)
    * [Dependencies](#dependencies)

<!-- TOC -->

## Online export

### How can the data be extracted?

| Method      | Content (output)              | Annotated [1*] | Expanded [2*] |
|-------------|-------------------------------|----------------|---------------|
| XML dump    | Wikitext [3*] + metadata [4*] | -              | -             |
| Parse API   | Wikitext / metadata           | -              | -             |
| Parsoid API | HTML                          | +              | +             |

**Meanings**:

- [1*] **Annotated** means that we can filter each fragment by semantics.
- [2*] **Expanded** means that the content is complete, does not contain templates
  and does not require additional scraping.
- [3*] **Wikitext** is MediaWiki markup language.
- [4*] **metadata**. Pages or wiki metadata needed for migration / searchability purposes.

**Why HTML DOM over plain Wikitext?** (Supportive sources)

- See [this post](https://qr.ae/pFkZg2) for an explanation for the architectural choice.
- HTML DOM over plain Wikitext, see
  [this statement](https://doc.wikimedia.org/Parsoid-PHP/master/md_docs_2apiuse.html#:~:text=If,responses).
- [SO discussion](https://stackoverflow.com/q/28782715/20399036) how Wikitext best converted into
  different output such as HTML. And the corresponding
  [blog article](https://www.gyford.com/phil/writing/2015/03/25/wikipedia-parsing/).

### How to normalize the data?

Define three subsets of HTML tags: to strip, preserve, and change.
Verify the version and check the sum of subsets is improper.

**Gap**: It is unclear for me where all the possible semantics per HTML tags documented.

**What can be the sources for that?**

1) [Parsoid HTML Specification (2.8.0)](https://www.mediawiki.org/wiki/Specs/HTML/2.8.0).
   Does not seem to cover all the HTML tags.

2) Maybe [Rich Attributes proposal](https://www.mediawiki.org/wiki/Parsoid/MediaWiki_DOM_spec/Rich_Attributes)
   can help.

### How to convert to Markdown?

**Possible conversion implementations**

| Dependency              | Size (uncompressed) | Flavors            |
|-------------------------|---------------------|--------------------|
| Pandoc [1]              | ~200 MB             | Many extensions... |
| html-to-markdown-rs [2] | ~20 MB              | CommonMark, GFM    |

**Sources**:

1. [Pandoc](https://pandoc.org)
2. [html-to-markdown-rs](https://crates.io/crates/html-to-markdown-rs)

## Offline conversion

XML dump conversion with proper stripping is not trivial.

- Requires templates expansion and conditionals evaluation.
- Re-implement extensions de-bloat mechanism that influence page rendering.
- Practically reinventing what Parsoid meant for.

See [this post](https://qr.ae/pFkZg2) for details.

A reliable solution is yet to be found.

## Programming language(s)

**Which programming language is best to choose for this project?**

Depends on:

- Library ecosystem around HTML to Markdown conversion.
- Runtime environment (RE) overhead.
- Dependency graph size, including RE (if required).

| Language  | "The" dependency [1*]                 | Flavors [2*]         |
|-----------|---------------------------------------|----------------------|
| **Rust**  | xberg-io/html-to-markdown-rs [1]      | CommonMark, GFM      |
| JVM-based | flexmark-java [2]                     | CommonMark, GFM, etc |
| Go        | Johanneskaufmann/html-to-markdown [5] | CommonMark, GFM      |
| C/C++     | xberg-io/html-to-markdown-ffi [4]     | CommonMark, GFM      |
| Python    | xberg-io/html-to-markdown [3]         | CommonMark, GFM      |

**Meanings**:

- [1*] **"The" dependency** means that it's the most relevant, reliable and fresh one.
- [2*] **Flavors** means which specifications the library guarantees to comply with.

**Sources**:

1. [xberg-io/html-to-markdown-rs](https://crates.io/crates/html-to-markdown-rs)
2. [flexmark-java](https://github.com/vsch/flexmark-java)
3. [Johanneskaufmann/html-to-markdown](https://github.com/Johanneskaufmann/html-to-markdown)
4. [xberg-io/html-to-markdown-ffi](https://github.com/xberg-io/html-to-markdown/tree/main/crates/html-to-markdown-ffi)
5. [xberg-io/html-to-markdown](https://github.com/xberg-io/html-to-markdown/tree/main/packages/python)

It appears obvious that **xberg-io/html-to-markdown-rs**
is the unmatched center of the universe. There is no point in using it through FFI.

Therefore, **Rust** was chosen.

## Dependencies

See [dependencies.md](docs/dev/dependencies.md) for dependencies choice explanation / motivation.
