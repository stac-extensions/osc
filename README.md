# Open Science Catalog Extension Specification

- **Title:** Open Science Catalog
- **Identifier:** <https://stac-extensions.github.io/osc/v1.0.0/schema.json>
- **Field Name Prefix:** osc
- **Scope:** Catalog, Collection, Item
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @constantinius

This document explains the Open Science Catalog Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec)
(STAC) specification.

- Examples:
  - [Project Collection example](examples/project/collection.json): Shows a project STAC Collection
  - [Product Collection example](examples/product/collection.json): Shows a product STAC Collection
  - [Product Item example](examples/product/item.json): Shows a product STAC Item
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Terms

This extension makes use of certain terms and definitions.

### Project

A science project dealing with theme, often bound to a certain geographic region. Each project can produce a a number of products,
which are sub-collections of type [`product`](#product).

In the OSC STAC Extension, projects are depicted as STAC Collections with additional [fields](#fields).

### Product

The description of a science product which was produced in the context of a [project](#project).

In the OSC STAC Extension, products are depicted as STAC Collections with additional [fields](#fields).

A product can be linked to sub-collections/catalogs of any shape or form or directly reference the STAC Items actually referencing
the data files the Product is comprised of.
STAC Items should mirror the fields of the product collection for search purposes.

### Theme

A thematic grouping of science projects and products, such as "Oceans", "Atmosphere" or "Land".

### Variable

An physical variable observed by a science product, such as "Wind stress" or "Geomagnetic field".

### EO Mission

A set of satellite missions which provided input for the product.

### Workflow

A definition of processing steps defined in a project that can be executed in an experiment to generate a product.

### Experiment

A specific execution of a workflow that generated a product.

## Fields

The fields in the table below can be used in these parts of STAC documents:

- [x] Catalogs
- [x] Collections
- [x] Item Properties
- [ ] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections)
- [ ] Links

**NOTE:** The Item fields can be added to Collection summaries, but should usually not
be added to the summaries as the same fields already exist in the collection as top-level properties anyway.
As such the extension does not validate Collection summaries.

### Projects and Products

| Field Name     | Type      | Description |
| -------------- | --------- | ----------- |
| osc:type       | string    | **REQUIRED.** The underlying type of this resource. Either `"project"` or `"product"`. This field then defines what other fields are allowed and required. |
| osc:status     | string    | **REQUIRED.** This field details whether the project or product is `planned`, `ongoing`, or has been `completed`. |

Additionally, the following fields from other extensions apply:

- [themes](#themes)

### Products only

Fields that apply when the `osc:type` is set to `product`:

| Field Name     | Type      | Description |
| -------------- | --------- | ----------- |
| osc:project    | string    | **REQUIRED.** The name of the project the product is associated with. |
| osc:region     | string    | The name of the geographic region the project or product is dealing with if any, e.g `"Arctic"` or `"Agulhas"`. |
| osc:variables  | \[string] | The names of the variables the product is observing, e.g `"Wind stress"` or `"Geomagnetic field"`. |
| osc:missions   | \[string] | The names of the satellite missions which provided input for this product.  |
| osc:experiment | string    | The name of the experiment that created the product. |

### Projects only

Fields that apply when the `osc:type` is set to `project`:

| Field Name     | Type      | Description |
| -------------- | --------- | ----------- |
| osc:workflows  | \[string] | The names of the workflows created by the project. |

Additionally, the following fields from other extensions apply:

- [contacts](#contacts)

### Contacts

The following fields should be implemented from the [Contacts extension](https://github.com/stac-extensions/contacts):

| Field Name | Type | Description |
| ---------- | ---- | ----------- |
| contacts   | \[[Contact Object](https://github.com/stac-extensions/contacts/blob/v0.1.1/README.md#contact-object)] | A list of contacts qualified by their role. |

The following `roles` for contacts SHALL be used:

- The role for the technical officer of a project is `technical_officer`.
- The role for consortium partners is `consortium_member`.

### Themes

The following fields should be implemented from the [Themes extension](https://github.com/stac-extensions/themes):

| Field Name | Type | Description |
| ---------- | ---- | ----------- |
| themes     | \[[Theme Object](https://github.com/stac-extensions/themes/blob/v1.0.0/README.md#theme-object)] | The names of the themes the project or product is dealing with. |

The themes field can contain concepts from different controlled vocabularies (via `scheme`).
By default this extension only asks to add concepts for the scheme `https://github.com/stac-extensions/osc#theme`.

## OGC API - Records

Although this extension is a STAC extension, similar fields with the same `osc:` prefix
are also used in OGC API - Records that describe workflows and experiments.

The following fields occur in workflows:

- `osc:project` (string, required)
- `osc:experiments` (\[string])

The following fields occur in experiments:

- `osc:project` (string, required)
- `osc:workflow` (string, required)
- `osc:product` (string, required)

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
