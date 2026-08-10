# Application Extension Specification

- **Title:** Application
- **Identifier:** <https://stac-extensions.github.io/application/v0.1.0/schema.json>
- **Field Name Prefix:** application
- **Scope:** Item
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @m-mohr

This document explains the Application Extension to the
[SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

This extension allows to describe applications and examples.
Applications could potentially be executed by a platform and should be self-contained.
Example could be potentially incomplete code snippets that might be shown with code highlighting for users to copy.

This extension is the successor of the
[Example Links Extension](https://github.com/stac-extensions/example-links).

- Examples:
  - [Item example](examples/item.json): Shows the basic usage of the extension in a STAC Item
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Fields

The fields in the table below can be used in these parts of STAC documents:

- [x] Catalogs
- [x] Collections
- [x] Item Properties (incl. Summaries in Collections)
- [x] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections and Asset Templates)
- [x] Links (incl. Link Templates)
- [ ] Bands

| Field Name            | Type      | Description |
| --------------------- | --------- | ----------- |
| application:container | string    | Specifies whether the given application is embedded into a container format (e.g. PDF, HTML, Jupyter Notebook, Markdown, ...). |
| application:languages | \[string] | Lists all the languages the application is using. |

### application:container

This fields lists the container format of the application, if applicable.
Should be any of the [languages listed for Linguist](https://github.com/github-linguist/linguist/blob/master/lib/linguist/languages.yml),
if one exists. Otherwise, custom values can be used.

If not provided, the application is expected to run on its own, e.g. is simply a Python script (usually a `.py` file).

Some common examples include:

- `Jupyter Notebook`
- `HTML`
- `RMarkdown`

### application:languages

This fields lists all the languages the application is using, which can be programming or markup languages depending on the usecase.
Should be any of the [languages listed for Linguist](https://github.com/github-linguist/linguist/blob/master/lib/linguist/languages.yml),
e.g. `Python` or `R`.

This field MUST NOT contain the container language (see above).

Clients MAY use this field as a hint for syntax highlighting when rendering code inline as a code block.
See the [relation types](#relation-types) for when a reference is rendered inline (`example`) versus shown as a link (`describedby`).

Highlighting SHOULD only be applied when exactly one language is listed and no `application:container` is set.

## Usage Examples

Examples can be provided directly as source code files or as a document with embedded code (e.g. web page, PDF document, Jupyter Notebook).

### Examples: Source Code Files

The Link Object for a Python application that is rather large and is expected to be executed directly:

```json
{
  "rel": "application",
  "href": "https://stac.example/run.py",
  "title": "Fancy Python application",
  "type": "text/x-python",
  "description": "This describes a complex *Python* application...",
  "file:size": 123547,
  "application:languages": ["Python"]
}
```

The Link Object for an example script that a client can render inline as a
highlighted code block:

```json
{
  "rel": "example",
  "href": "https://stac.example/example.py",
  "title": "Example: read the data with rasterio",
  "description": "Shows how to open and read the data in *Python*",
  "type": "text/x-python",
  "file:size": 842,
  "application:languages": ["Python"]
}
```

### Examples: Code in a Container

The Link Object for a PDF document with C code:

```json
{
  "rel": "describedby",
  "href": "https://stac.example/py-example.pdf",
  "title": "Example PDF containing C",
  "description": "Describes how to do fancy stuff with the data in C",
  "type": "application/pdf",
  "application:container": "PDF",
  "application:languages": ["C"]
}
```

The Link Object for a Jupyter Notebook containing Python code:

```json
{
  "rel": "describedby",
  "href": "https://stac.example/py-notebook.ipynb",
  "title": "Example Notebook containing Python",
  "description": "Describes how to do fancy stuff with the data in Python",
  "type": "application/x-ipynb+json",
  "application:container": "Jupyter Notebook",
  "application:languages": ["Python"]
}
```

The Link Object for a HTML document that shows JavaScript code:

```json
{
  "rel": "describedby",
  "href": "https://stac.example/js-example.html",
  "title": "Example webpage containing JS",
  "description": "Describes how to do fancy stuff with the data in JavaScript",
  "type": "text/html",
  "application:container": "HTML",
  "application:languages": ["JavaScript"]
}
```

The Link Object for a RMarkdown document that shows R code:

```json
{
  "rel": "describedby",
  "href": "https://stac.example/js-example.rmd",
  "title": "Example RMarkdown document containing R",
  "description": "Describes how to do fancy stuff with the data in R",
  "type": "text/markdown",
  "application:container": "RMarkdown",
  "application:languages": ["R"]
}
```

## Relation types

The following types should be used as applicable `rel` types in the
[Link Object](https://github.com/radiantearth/stac-spec/tree/master/item-spec/item-spec.md#link-object).

| Type                 | Description |
| -------------------- | ----------- |
| example              | A reference to example code. The files could potentially be loaded and rendered as code examples. |
| describedby          | A reference to prose documentation (e.g. an HTML page or PDF) that describes the application (or data). Use this for content that "renders itself", i.e. should not be loaded and rendered as code example. |
| application          | A reference to an application. |
| application-platform | A reference to a platform that can execute applications. |
| vcs                  | A reference to a version control system, e.g. the GitHub repository of the catalog or application. |
| manifest             | A reference to a document describing the application in more detail, e.g. `package.json` (JavaScript), `pyproject.toml` (Python), or a CodeMeta file. |

## Media Types

The following media types could be used as applicable `type` in the Link and Asset Objects:

| Media Type                       | Description |
| -------------------------------- | ----------- |
| application/vnd.codemeta.ld+json | Refers to a [CodeMeta](https://codemeta.github.io/) file/response. |

## Roles

The following types should be used as applicable `roles` in the Link or
[Asset Object](https://github.com/radiantearth/stac-spec/blob/master/commons/assets.md#asset-object).

| Type    | Description |
| ------- | ----------- |
| example | A reference to example data. |

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PRs are part of the repository and can be run locally to verify that changes are valid.
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on
your command line run:

```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:

```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:

```bash
npm run format-examples
```
