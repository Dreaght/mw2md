# Workspace members | Packages

> Although the **single responsibility principle** suggests that 'each module should only handle one hard problem', it
> is more important that '**each hard problem is only handled by one module**'
> ([source](https://programmingisterrible.com/post/139222674273/write-code-that-is-easy-to-delete-not-easy-to))

The whole project is best split into **four default** and **one optional** packages:

| Artifact ID                   | Name                 | Role                  |
|-------------------------------|----------------------|-----------------------|
| [mw-fetch](#mw-fetch)         | MediaWiki fetcher    | Member                |
| [mw-parsoid](#mw-parsoid)     | MediaWiki Parsoid    | Member (**OPTIONAL**) |
| [mw-normalize](#mw-normalize) | MediaWiki normalizer | Member                |
| [mw-convert](#mw-normalize)   | MediaWiki converter  | Member                |
| [mw2md](#mw2md)               | mw2md                | ROOT                  |

The **last** one orchestrates the **first four** in the root:
those can be tested and piped separately.

`mw-` prefix is used to avoid collisions.

If they can act as separate tools, then why not.

They work like that (piping):
`mw-fetch <html> | mw-normalize | mw-convert`

`mw-parsoid` participates if you choose to fetch XML:
`mw-fetch <xml> | mw-parsoid | mw-normalize | mw-convert`.

Output of one becomes the input of the other.

Not a chain, modules don't know anything about each other. mw2md is the root module rules them all. So `mw2md` is an
equivalent of one of those pipelines.

## mw-fetch

See more in [mw-fetch.md](mw-fetch.md).

## mw-parsoid

## mw-normalize

## mw-convert

## mw2md

A single root API orchestrates the functionality of the previous ones.
