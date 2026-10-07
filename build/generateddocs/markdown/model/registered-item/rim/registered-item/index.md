
# Registered Item (Schema)

`ogc.model.registered-item.rim.registered-item` *v0.2*

Modular RDF implementation profile based on ISO/FDIS 19135:2026.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Registered Item

## Purpose

A **Registered Item** is an actual managed unit of information in a register. It is an instance of a Register Item Class and can realize a Concept Version from the concept plane.

This block represents operational register content rather than the template that constrains that content.

## Scope

This block introduces:

- `rim:RegisterItem`
- item-level object and functional identifiers
- `rim:itemClass`
- `rim:inRegister`
- `rim:realizes`
- `rim:relatedItem`
- `rim:supersededBy`
- `rim:supersedes`

## Conceptual position

```text
Register Item Class
  `-- instance --> Registered Item
                       |-- inRegister --> Register
                       |-- realizes --> Concept Version
                       `-- relates to / supersedes --> Registered Item
```

## Dependencies

- `register`
- `register-item-class`
- `concept-version`

## SHACL validation

A registered item requires one object identifier, one functional identifier, exactly one item class and exactly one containing register. It may realize a concept version and may be related to or supersede other registered items.

## Typical examples

- the registered entry for EPSG:4326
- an AHN dataset entry
- a registered release of DCAT-AP-NL
- an individual code-list value

## Register Item Class versus Registered Item

```text
Register Item Class: Coordinate Reference System
Registered Item:      EPSG:4326
```

The class supplies common requirements. The item supplies the actual governed content.

## ISO 19135 alignment

This block represents the concrete content-plane unit. Register actions that modify its state or record its history are defined separately in the Register Action block.

## Examples

### Valid ISO 19135 Registered Item example
#### turtle
```turtle
@prefix rim: <https://w3id.org/ogc/rim/>.
@prefix dct: <http://purl.org/dc/terms/>.
@prefix ex: <https://example.org/rim/>.

ex:i a rim:RegisterItem;
  rim:objectIdentifier <urn:uuid:44444444-4444-4444-8444-444444444444>;
  rim:functionalIdentifier <https://example.org/items/metre>;
  rim:itemClass ex:uc;
  rim:inRegister ex:r;
  rim:realizes ex:cv;
  dct:title "metre"@en.

ex:uc a rim:RegisterItemClass.
ex:r a rim:Register.
ex:cv a rim:ConceptVersion.


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: ISO 19135 Registered Item
type: object
properties:
  id:
    type: string
    format: uri
    x-jsonld-id: '@id'
  type:
    const: rim:RegisterItem
    x-jsonld-id: '@type'
required:
- id
- type
additionalProperties: true
x-jsonld-prefixes:
  rim: https://w3id.org/ogc/rim/
  dct: http://purl.org/dc/terms/
  skos: http://www.w3.org/2004/02/skos/core#
  prov: http://www.w3.org/ns/prov#

```

Links to the schema:

* YAML version: [schema.yaml](./annotated/model/registered-item/rim/registered-item/schema.json)
* JSON version: [schema.json](./annotated/model/registered-item/rim/registered-item/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "id": "@id",
    "type": "@type",
    "rim": "https://w3id.org/ogc/rim/",
    "dct": "http://purl.org/dc/terms/",
    "skos": "http://www.w3.org/2004/02/skos/core#",
    "prov": "http://www.w3.org/ns/prov#",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](./annotated/model/registered-item/rim/registered-item/context.jsonld)


# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/Kurrawong/registered-item-model](https://github.com/Kurrawong/registered-item-model)
* Path: `_sources/rim/registered-item`

