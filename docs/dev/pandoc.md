# Interacting with Pandoc

[What is Pandoc?](https://pandoc.org/)

# Lua filters

Read [full documentation](https://pandoc.org/lua-filters.html).

## Structure

From [lua filter structure docs](https://pandoc.org/lua-filters.html#lua-filter-structure):

**CLI usage**

> Filters are expected to be put into separate files and are passed
> via the `--lua-filter` command-line argument.
> For example, if a filter is defined in a file `current-date.lua`,
> then it would be applied like this:
>
> ```
> pandoc --lua-filter=current-date.lua -f markdown MANUAL.txt
> ```
>
> The `--lua-filter` option may be supplied multiple times.
> Pandoc applies all filters (including JSON filters specified via `--filter`
> and Lua filters specified via `--lua-filter`)
> in the order they appear on the command line.

**Lua scripts**

> Pandoc expects each Lua file to return a filter.

> The return value of a filter function must be one of the following:
>
> 1. **nil**: this means that the object should remain unchanged.
> 2. **a pandoc object**: this MUST be of the same type as the input and will replace the original object.
> 3. **a list of pandoc objects**: these will replace the original object; the list is merged with the neighbors of the
     original objects (spliced into the list the original object belongs to); returning an empty list deletes the
     object.

> The function’s output MUST result in an element of the same type as the input.
> This means a filter function acting on an inline element MUST return
> either nil, an inline, or a list of inlines, and a function filtering a block element
> MUST return one of nil, a block, or a list of block elements.
>
> **Pandoc will throw an error if this condition is violated.**
