# Activity Type Register Profile

A profile of the [Registered Item Model](../core-ontology) for registers whose items are **types
of `prov:Activity`**: kinds of activity such as a quality review, a reprojection or a model
inference run, governed with the same identifiers, statuses and change history as any other
register item.

## What it adds

- **`acttype:ActivityType`**: a sub-class of `rim:RegisterItem` whose instances are classes, each
  declared `rdfs:subClassOf prov:Activity` (directly or through other registered activity types).
  The register item and the type are one resource, so an individual activity is declared with
  `rdf:type` pointing straight at the governed register item.
- **A narrowed `rim:itemClass` domain**: the register item class `acttype:activityTypeItemClass`,
  plus SHACL in both directions. Every `acttype:ActivityType` must use that item class and be a
  sub-class of `prov:Activity`. Every register item using that item class must be an
  `acttype:ActivityType`.
- **Related PROV object types**: type-level counterparts of the PROV relations:
  `acttype:usedEntityType` (`prov:used`), `acttype:generatedEntityType` (`prov:generated`),
  `acttype:associatedAgentType` (`prov:wasAssociatedWith`) and `acttype:informedByActivityType`
  (`prov:wasInformedBy`). Values are classes.
- **An optional plan**: `acttype:plan` (at most one), pointing to a `prov:Plan` that describes the
  steps an activity of this type must carry out. Use [P-Plan](https://www.opmw.org/model/p-plan/)
  for the steps (`p-plan:Step`, `p-plan:isStepOfPlan`, `p-plan:isPrecededBy`). An individual
  activity records the plan it followed with PROV's qualified association
  (`prov:qualifiedAssociation` / `prov:hadPlan`).

## Profiling this block further

A further profile, such as [Geoprocessing Activity](../geoprocessing-activity), typically:

1. declares a root activity class (for example `geoproc:GeoprocessingActivity rdfs:subClassOf
   prov:Activity`) that its registered types specialise;
2. constrains the related types that those registered types must declare, with a shape targeting
   every sub-class of the root that is also a registered `acttype:ActivityType`:

```turtle
ex:MyRootTypesShape a sh:NodeShape ;
    sh:targetNode ex:MyRootActivity ;
    sh:property [
        sh:path [ sh:oneOrMorePath [ sh:inversePath rdfs:subClassOf ] ] ;
        sh:node [ sh:or ( [ sh:not [ sh:class acttype:ActivityType ] ]
                          ex:MyActivityTypeConstraints ) ] ;
    ] .
```

The `sh:not` branch skips sub-classes that are only declared in an ontology and are not being
registered. `ex:MyActivityTypeConstraints` then constrains `acttype:usedEntityType`,
`acttype:generatedEntityType` and similar properties, for example with `sh:qualifiedValueShape`
over an `rdfs:subClassOf*` path.

Each constraining profile can itself be profiled: a sub-type's shapes add to its parent's and never
replace them.
