# Application Extension Specification

- **Title:** Application
- **Identifier:** <https://stac-extensions.github.io/application/v0.1.0/schema.json>
- **Field Name Prefix:** application
- **Scope:** Item, Assets, Links
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

- [ ] Catalogs
- [ ] Collections
- [x] Item Properties (incl. Summaries in Collections)
- [x] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections)
- [x] Links

| Field Name           | Type    | Description |
| -------------------- | ------- | ----------- |
| application:type     | string  | Type of the application. This is a free-text field without pre-defined values. Please define a well-defined set of types for your usecase. |
| application:embedded | boolean | Specifies whether the given application is directly the example code (`false`, default, e.g. a Python file) or to a container format (`true`, e.g. PDF, HTML, Jupyter Notebook, Markdown, ...) that contains/describes code snippets. |
| application:language | string  | If the application is in a specific programming language, specify it here. Should be one of the [languages listed for Linguist](https://github.com/github-linguist/linguist/blob/master/lib/linguist/languages.yml). For example, `JavaScript` or `Python`. If a page contains multiple programming languages, you can use `Multiple`. |

## Usage Examples

Examples can be provided directly as source code files or as a document with embedded code (e.g. web page, PDF document, Jupyter Notebook).

### Examples: Source Code Files

The Link Object for a Python source code file:
```json
{
  "rel": "application",
  "href": "https://stac.example/run.py",
  "title": "Fancy Python app",
  "type": "text/x-python",
  "description": "This describes the *Python* script...",
  "file:size": 123547,
  "application:language": "Python"
}
```

The Link Object for a JavaScript source code file:
```json
{
  "rel": "application",
  "href": "https://stac.example/run.js",
  "title": "Fancy JavaScript app",
  "type": "application/javascript",
  "description": "This describes the *JavaScript* code...",
  "file:size": 231,
  "application:language": "JavaScript"
}
```

### Examples: Embedded Code

The Link Object for a PDF document with C code:
```json
{
  "rel": "example",
  "href": "https://stac.example/py-example.pdf",
  "title": "Example PDF containing C",
  "description": "Describes how to do fancy stuff with the data in C",
  "type": "application/pdf",
  "application:embedded": true,
  "application:language": "C"
}
```

The Link Object for a Jupyter Notebook containing Python code:
```json
{
  "rel": "example",
  "href": "https://stac.example/py-notebook.ipynb",
  "title": "Example Notebook containing Python",
  "description": "Describes how to do fancy stuff with the data in Python",
  "type": "application/x-ipynb+json",
  "application:type": "jupyter-notebook",
  "application:embedded": true,
  "application:language": "Python"
}
```

The Link Object for a HTML document that shows JavaScript code:
```json
{
  "rel": "example",
  "href": "https://stac.example/js-example.html",
  "title": "Example webpage containing JS",
  "description": "Describes how to do fancy stuff with the data in JavaScript",
  "type": "text/html",
  "application:embedded": true,
  "application:language": "JavaScript"
}
```

The Link Object for a RMarkdown document that shows R code:
```json
{
  "rel": "example",
  "href": "https://stac.example/js-example.rmd",
  "title": "Example RMarkdown document containing R",
  "description": "Describes how to do fancy stuff with the data in R",
  "type": "text/markdown",
  "application:embedded": true,
  "application:language": "R"
}
```

## Relation types

The following types should be used as applicable `rel` types in the
[Link Object](https://github.com/radiantearth/stac-spec/tree/master/item-spec/item-spec.md#link-object).

| Type                 | Description |
| -------------------- | ----------- |
| example              | A reference to example code. |
| application          | A reference to an application. |
| application-platform | A reference to a platform that can execute applications. |

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid. 
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
