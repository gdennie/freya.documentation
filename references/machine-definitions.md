# Freya.Machine Reference

Freya.Machine uses a computation expression syntax to let you define resources in a declarative, type-safe way. It uses custom operations to try and provide a clean and tidy syntax for doing this, but this does mean that you need to know what those operations are called, and what their specificaton is!

The following sections give a complete reference to all *user configurable* operations for a Freya.Machine resource. None of the operations are _"required"_, so all have defaults which will be used if not specified. These are also given in the tables where relevant, otherwise they are specified in the section description.

Other functions return a value specific to that operation, and are detailed in the table.

## Actions

Action operations are of type `Freya<unit>` (aliased as `FreyaMachineAction` in the Freya.Machine codebase). By default all actions will be a simple no-op (a safe default).

Operation | Description
----------|------------
doDelete  | Fill In
doPatch   | Fill In
doPost    | Fill In
doPut     | Fill In

## Configuration

Configuration operation types vary by required return type, so the types are given in the table. They are all of Freya<T>, where T is configuration operation specific. Default values returned (the `T` from `Freya<T>`) are given.

Operation            | Type                                   | Default | Description
---------------------|----------------------------------------|---------|------------
charsetsSupported    | `Freya<Charset list>`                  | Fill In | Fill In
corsHeadersExposed   | `Freya<string list>`                   | Fill In | Fill In
corsHeadersSupported | `Freya<string list>`                   | Fill In | Fill In
corsMethodsSupported | `Freya<Method list>`                   | Fill In | Fill In
corsOriginsSupported | `Freya<AccessControlAllowOriginRange>` | Fill In | Fill In
encodingsSupported   | `Freya<ContentCoding list>`            | Fill In | Fill In
eTag                 | `Freya<EntityTag>`                     | Fill In | Fill In
languagesSupported   | `Freya<LanguageTag list>`              | Fill In | Fill In
lastModified         | `Freya<DateTime>`                      | Fill In | Fill In
mediaTypesSupported  | `Freya<MediaType list>`                | Fill In | Fill In
methodsKnown         | `Freya<Method list>`                   | Fill In | Fill In
methodsSupported     | `Freya<Method list>`                   | Fill In | Fill In

## Decisions

Decision operations are of type `Freya<bool>` (aliased as `FreyaMachineDecision` in the Freya.Machine codebase). Default values returned are given.

Operation          | Default | Description
-------------------|---------|------------
allowed            | `true`  | Fill In
allowPostToGone    | `true`  | Fill In
allowPostToMissing | `true`  | Fill In
