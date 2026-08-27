<h1 align="center">asciidoc.mq</h1>

An [AsciiDoc](https://docs.asciidoctor.org/asciidoc/latest/) to Markdown converter implemented as an [mq](https://github.com/harehare/mq) module.

## Features

- Document/section titles (`=` .. `======`) → ATX headings (`#` .. `######`)
- Paragraphs, with soft-wrapped lines joined and explicit line breaks (`+`) preserved
- Inline formatting: bold (`*bold*` / `**bold**`), italic (`_em_` / `__em__`), monospace (`` `code` ``, left untouched and markup-protected)
- Links (`link:url[text]`, bare `url[text]`) and images (`image:path[alt]`, `image::path[alt]`)
- Unordered lists (`*`, `-`), ordered lists (`.`, `1.`), arbitrarily nested, with checklist items (`[x]` / `[ ]` / `[*]`)
- Source/listing blocks (`----`, with optional `[source,lang]`) and literal blocks (`....`) → fenced code blocks
- Quote blocks (`____`, with optional `[quote, Author, Source]`) → blockquotes with attribution
- Admonitions (`NOTE:` / `TIP:` / `IMPORTANT:` / `WARNING:` / `CAUTION:`, both paragraph and `====` block form) → GitHub-style Markdown callouts
- Horizontal rules (`'''`)
- Tables (`|===`)
- Line comments (`//`) and block comments (`////`) are stripped

## Installation

Copy `asciidoc.mq` to your mq module directory, or place it anywhere and reference it with `-L`.

```sh
cp asciidoc.mq ~/.local/mq/config/
```

### HTTP Import (no local installation needed)

HTTP imports are disabled by default; pass `--allow-http-import` to import directly from GitHub without any local setup:

```sh
mq --allow-http-import -I raw 'import "github.com/harehare/asciidoc.mq" | asciidoc::asciidoc_to_markdown(.)' input.adoc
```

Pin to a specific release with `@vX.Y.Z`:

```sh
mq --allow-http-import -I raw 'import "github.com/harehare/asciidoc.mq@v0.1.0" | asciidoc::asciidoc_to_markdown(.)' input.adoc
```

## Usage

```sh
mq -L /path/to/modules -I raw \
  'import "asciidoc" | asciidoc::asciidoc_to_markdown(.)' input.adoc
```

If you copied it to the mq built-in module directory:

```sh
mq -I raw 'import "asciidoc" | asciidoc::asciidoc_to_markdown(.)' input.adoc
```

## API

### `asciidoc_to_markdown(input)`

Converts an AsciiDoc document string to a Markdown string.

| Input type | Output |
|---|---|
| String | Markdown string |

### `ad_inline(input)`

Converts only the inline formatting (bold, italic, links, images) of a single string, leaving `` `monospace` `` spans untouched. Useful for converting fragments rather than whole documents.

## Example

Given `article.adoc`:

```adoc
= Article Title

== Introduction

This is a *bold* claim, and this is _emphasized_. See https://example.com[the docs].

* First point
* Second point
** Nested detail

NOTE: Remember this important detail.
```

```sh
mq -L . -I raw 'import "asciidoc" | asciidoc::asciidoc_to_markdown(.)' article.adoc
```

```markdown
# Article Title

## Introduction

This is a **bold** claim, and this is *emphasized*. See [the docs](https://example.com).

- First point
- Second point
  - Nested detail

> [!NOTE]
> Remember this important detail.
```

## Limitations

This module targets a common, practical subset of AsciiDoc rather than the full specification. Notably absent: tables of contents, cross-references (`<<id>>`), footnotes, includes, attribute substitution (`{name}`), passthrough blocks, sidebar/example blocks without a recognized admonition/quote/source attribute, and nested inline emphasis (e.g. bold containing italic).

## Compatibility

Requires [mq](https://github.com/harehare/mq) v0.6 or later (for the `md` builtin module).

## License

MIT
