# Open Science Catalog Extension Specification

- **Title:** Open Science Catalog
- **Identifier:** <https://stac-extensions.github.io/osc/v1.0.0/schema.json>
- **Field Name Prefix:** osc
- **Scope:** Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @constantinius

This document explains the Open Science Catalog Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec)
(STAC) specification.

- Examples:
  - [Root Collection example](examples/collection.json): Shows the root level of an OSC catalog
  - [Project Collection example](examples/4dionosphere-swarm-vip/collection.json): Shows a project STAC Collection
  - [Product Collection example](examples/4dionosphere-swarm-vip/model-ionosphere-4dionosphere/collection.json): Shows a product STAC Collection
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Terms

This extension makes use of certain terms and definitions.

### Theme

A thematic grouping of science projects and products, such as "Oceans", "Atmosphere" or "Land".

### Variable

An physical variable observed by a science product.

### Project

A science project dealing with theme, often bound to a certain geographic region. Each project can produce a a number of products,
which are sub-collections of type [`product`](#product).

In the OSC STAC Extension, projects are depicted as STAC Collections with additional [fields](#fields).

### Product

The description of a science product which was produced in the context of a [project](#project).

In the OSC STAC Extension, products are depicted as STAC Collections with additional [fields](#fields).

A product can be linked to sub-collections/catalogs of any shape or form dor directly reference the STAC Items actually referencing
the data files the Product is comprised of.

## Fields

The fields in the table below can be used in these parts of STAC documents:
- [ ] Catalogs
- [x] Collections
- [ ] Item Properties (incl. Summaries in Collections)
- [ ] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections)
- [ ] Links

| Field Name    | Type      | Description                                                                            | Available for |
| ------------- | --------- | -------------------------------------------------------------------------------------- | ------------- |
| osc:type      | string    | The underlying type of this collection. Either `"project"` or `"product"`. This field then defines what other fields are allowed. | project, product |
| osc:name      | string    | The descriptive name of the project or product. Can be distinct from `title` or `id`, but is available for historic reasons. | project, product |
| osc:status    | string    | This field details whether the project has already been completed (`"completed"`) or whether it is still ongoing (`"ongoing"`). | project, product |
| osc:project   | string    | The name of the project this product is associated with.                               | product |
| osc:region    | string    | The name of the geographic region this project or product is dealing with if any, e.g `"Arctic"` or `"Agulhas"`. | project, product |
| osc:variables | \[string] | The names of the variables the product is observing, e.g `"Wind stress"` or `"Geomagnetic field"`. | product |
| osc:missions  | \[string] | The names of the satellite missions which provided input for this project or product.  | project, product |

### Contacts

The following fields should be implemented from the [Contacts extension](https://github.com/stac-extensions/contacts):

| Field Name | Type | Description | Available for |
| ---------- | ---- | ----------- | ------------- |
| contacts   | \[[Contact Object](https://github.com/stac-extensions/contacts/blob/main/README.md#contact-object)] | A list of contacts qualified by their role. | project |

The following `roles` for contacts SHALL be used:

- The role for the technical officer of a project is `technical_officer`.
- The role for consortium partners is `consortium_member`.

### Themes

The following fields should be implemented from the [Themes extension](https://github.com/stac-extensions/themes):

| Field Name | Type | Description | Available for |
| ---------- | ---- | ----------- | ------------- |
| themes     | \[[Theme Object](https://github.com/stac-extensions/themes/blob/main/README.md#theme-object)] | The names of the themes the project or product is dealing with. | project, product |

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
