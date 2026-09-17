# ML Activity Profile

A [profile](https://ogcincubator.github.io/bblocks-docs/) of the
[Registered Item Model](../core-ontology) that describes machine learning training and inference
runs as typed register actions, built on the
[STAC Machine Learning Model (MLM) extension ontology](https://github.com/ogcincubator/bblocks-stac/tree/master/_sources/extensions/mlm-ontology).

## Why this is a profile, not a new model

MLM describes a model as a static artifact: a STAC Item or Collection with declared `mlm:tasks`,
`mlm:framework`, `mlm:accelerator`, and `mlm:input`/`mlm:output` specifications. It has no notion
of a particular *run* of that model — and the base Registered Item Model already has exactly the
class for that: `rim:RegisterAction`, a `prov:Activity` recording who or what did something, when,
and why. This block adds `mlact:MLActivity` as a sub-class of `rim:RegisterAction`, because in this
ecosystem running a model is itself a governed act — typically the one that adds a derived product
to a register as a new `rim:RegisterItem`.

## What it adds

- **Activity types** — `mlact:MLActivity`, with `mlact:TrainingActivity` and
  `mlact:InferenceActivity` sub-types.
- **Constraints on the activity** — `mlact:model` (which STAC Item/Collection was run),
  `mlact:performsTask` (a sub-property of `mlm:tasks`, constrained by SHACL to the same closed MLM
  task list), and `mlact:framework`/`mlact:accelerator` (the actual runtime environment, which can
  differ from what the model declares as default).
- **Typed inputs and outputs** — `mlact:ActivityInput`/`mlact:ActivityOutput` (both `prov:Entity`):
  the actual data consumed/produced by one run, as distinct from MLM's `ModelInput`/`ModelOutput`
  (the model's declared *specification*). Each links back to the specification it realises via
  `dct:conformsTo`, and to its actual tensor shape/dtype via `mlact:structure`, reusing MLM's own
  `InputStructure`/`ResultStructure` classes rather than redefining them.

Nothing here modifies the MLM ontology or the base Registered Item Model — it only adds new classes
and properties on top of both, which is what makes it a profile rather than a fork of either.
