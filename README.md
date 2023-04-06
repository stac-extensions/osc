# Open Science Catalog Extension Specification

- **Title:** Open Science Catalog
- **Identifier:** <https://stac-extensions.github.io/osc/v1.0.0/schema.json>
- **Field Name Prefix:** osc
- **Scope:** Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @constantinius

This document explains the Open Science Catalog Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec)
(STAC) specification.

## Terms

This extension makes use of certain terms and definitions.

### Theme

A thematic grouping of science projects and products, such as "Oceans", "Atmosphere" or "Land".

### Variable

An physical variable observed by a science product.

### Project

A science project dealing with theme, often bound to a certain geographic region. Each project can produce a a number of products,
which are sub-collections of type [`Product`](#product).

In the OSC STAC Extension, projects are depicted as STAC Collections with additional [fields](#fields).

### Product

The description of a science product which was produced in the context of a [project](#project).

In the OSC STAC Extension, products are depicted as STAC Collections with additional [fields](#fields).

A product can be linked to sub-collections/catalogs of any shape or form dor directly reference the STAC Items actually referencing
the data files the Product is comprised of.

- Examples:
  - [Root Collection example](examples/collection.json): Shows the root level of an OSC catalog
  - [Project Collection example](examples/4dionosphere-swarm-vip/collection.json): Shows a project STAC Collection
  - [Product Collection example](examples/4dionosphere-swarm-vip/model-ionosphere-4dionosphere/collection.json): Shows a product STAC Collection
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Fields

The fields in the table below can be used in these parts of STAC documents:
- [ ] Catalogs
- [x] Collections
- [ ] Item Properties (incl. Summaries in Collections)
- [ ] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections)
- [ ] Links

| Field Name            | Type                                                  | Description                                                                            | Available for |
| --------------------- | ----------------------------------------------------- | -------------------------------------------------------------------------------------- | ------------- |
| osc:type              | string                                                | The underlying type of this collection. Either `"Project"` or `"Product"`.             | project, product |
| osc:name              | string                                                | The descriptive name of the project or product. Can be distinct from `title` or `id`.  | project, product |
| osc:status            | string                                                | Either `"COMPLETED"` or `"ONGOING"`.                                                   | project, product |
| osc:project           | string                                                | The name of the project this product is associated with.                               | product |
| osc:region            | string                                                | The name of the geographic region this project or product is dealing with if any.      | project, product |
| osc:themes            | \[string]                                             | The names of the themes the project or product is dealing with.                        | project, product |
| osc:variables         | \[string]                                             | The names of the variables the product is observing.                                   | product |
| osc:missions          | \[string]                                             | The names of the satellite missions which provided input for this project or product.  | project, product |
| osc:technical_officer | [Technical Officer Object](#technical-officer-object) | The technical officer supervising the project.                                         | project |
| osc:consortium        | \[string]                                             | The names of the participating organizations                                           | project |

### Additional Field Information

#### osc:type

The type of this collection, as either `"Project"` if it is a project collection or `"Product"`. This field then
defines what other fields are allowed.

#### osc:name

A more descriptive name of the project/product. This is for historic reason and usually more descriptive than the `title`.

#### osc:status

This field details whether the project has already been completed (`"COMPLETED"`) or whether it is still ongoing (`"ONGOING"`).

#### osc:region

This is the name of the region, the project or product operates in, e.g `"Arctic"` or `"Agulhas"`.

#### osc:variable

The name of the variable that this product observes, e.g `"Wind stress"` or `"Geomagnetic field"`.

#### osc:missions

The names of the EO Satellite missions contributing to this particulate science product.

### Technical Officer Object

This object describes the contact details of the technical officer supervising a project.

| Field Name  | Type   | Description |
| ----------- | ------ | ----------- |
| name        | string | **REQUIRED**. The name of the technical officer. |
| e-mail      | number | **REQUIRED**. The e-mail address of the technical officer. |

## Roles

The following asset roles can be used to more accurately describe root level metadata files.

| Role        | Description |
| ----------- | ----------- |
| eo-missions | The role of the JSON file describing all contributing EO-Missions |
| themes      | The role of the JSON file describing all available themes of the catalog. |
| variables   | The role of the JSON file describing all available variables of the catalog. |

### EO Missions JSON file

The JSON file structure is an array of objects, each having at least he `name` field, depicting the EO Missions name.

```json
[
  {
    "name": "AEOLUS"
  },
  {
    "name": "AMSR-E"
  },
  {
    "name": "AMSR2"
  },
  {
    "name": "ASCAT"
  },
  ...
]
```

### Themes JSON file

The JSON file structure is an array of objects, each having at least he `name`, `description`, `link` and `image` fields, describing the theme.

```json
[
  {
    "name": "Atmosphere",
    "description": "Trace gases are produced and destructed by physical, biological and chemical processes. This natural cycle has been perturbed over the past decades by the global population increase and by the related boost of anthropogenic activities. The composition of the atmosphere is undergoing major and rapid changes with significant impacts on a global scale, affecting the environment and human health. Funding agencies and the scientific community have been putting enormous efforts into studying the impact of these changes. Nonetheless, there is still a need to fill knowledge gaps and enhance the understanding of precursors and the formation mechanisms of the different atmospheric compounds and the relative contribution of biogenic and anthropogenic sources and sinks to their global budget. Such knowledge improvements shall serve as input for policy makers in order to adopt mitigation and adaptation strategies.",
    "link": "https://eo4society.esa.int/communities/scientists/esa-atmosphere-science-cluster/",
    "image": "https://metadata.opensciencedata.esa.int/open-science-catalog-metadata/themes/images/EO_Atmosphere.webp"
  },
  ...
]
```

### Variable JSON file

The JSON file structure is an array of objects, each having at least he `name`, `description`, `link` and `themes` fields, describing the theme.

```json
[
  {
    "name": "(13)CH4 delta",
    "description": "3D field of\u00a0Delta C-13 in CH4 (Methane) (isotopic signature).\u00a0Isotopic ratio, expressed as deviations from an agreed-upon international reference measurement standard (which defines the corresponding isotope scales) using the delta notation: \u03b4 = (Rsample/Rreference \u2013 1), with R = [rare isotope]/[abundant isotope].",
    "link": "https://space.oscar.wmo.int/variables/view/13_ch4_delta",
    "themes": ["Atmosphere"]
  },
  {
    "name": "(13)CO Delta",
    "description": "3D field of\u00a0Delta C-13 in CO (Carbon monoxide) (isotopic signature)",
    "link": "https://space.oscar.wmo.int/variables/view/13_co_delta",
    "themes": ["Atmosphere"]
  },
  ...
]
```

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
