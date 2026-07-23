# Workspace members | Packages

The whole project is best split into four distinct packages:

1. [MediaWiki fetcher](#mw-fetch)
2. [MediaWiki normalizer](#mw-normalize)
3. [MediaWiki converter](#mw-convert)
4. [mw2md](#mw2md) (*Root: All in one*)

The **last** one orchestrates the **first three** in the root:
those can be tested and piped separately.

`mw-` prefix is used to avoid collisions.

## mw-fetch

## mw-normalize

## mw-convert

## mw2md

A single root API orchestrates the functionality of the previous ones.
