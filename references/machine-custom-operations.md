# Freya.Machine Reference

Freya.Machine uses a computation expression syntax to let you define resources in a declarative, type-safe way (for a full _guide_ to defining resources using Freya.Machine, see [Defining Resources with Machine][resources]). It uses custom operations to try and provide a clean and tidy syntax for doing this, but this does mean that you need to know what those operations are called, and how to use them. A full reference to all of those custom operations is provided by this document, with links to more detail on specific areas when helpful. This reference is also best read in combination with the guide on [Understanding the Machine Execution Model][graph].

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

Operation               | Default | Description
------------------------|---------|------------
allowed                 | `true`  | Fill In
allowPostToGone         | `true`  | Fill In
allowPostToMissing      | `true`  | Fill In
allowPutToMissing       | `true`  | Fill In
authorized              | `true`  | Fill In
charsetsStrict          | `false` | Fill In
conflicts               | `false` | Fill In
contentTypeKnown        | `true`  | Fill In
contentTypeValid        | `true`  | Fill In
created                 | `true`  | Fill In
deleted                 | `true`  | Fill In
encodingsStrict         | `false` | Fill In
entityLengthValid       | `true`  | Fill In
existed                 | `false` | Fill In
exists                  | `true`  | Fill In
languagesStrict         | `false` | Fill In
malformed               | `false` | Fill In
mediaTypesStrict        | `true`  | Fill In
movedPermanently        | `false` | Fill In
movedTemporarily        | `false` | Fill In
multipleRepresentations | `false` | Fill In
postRedirect            | `false` | Fill In
processable             | `true`  | Fill In
putToDifferentUri       | `false` | Fill In
respondWithEntity       | `true`  | Fill In
serviceAvailable        | `true`  | Fill In
uriTooLong              | `false` | Fill In

## Handlers

Handler operations are of type `FreyaMachineNegotiation -> Freya<FreyaMachineRepresenyation>`. See the guide to [Handlers and Content Negotiation in Machine][handlers] for a full explanation of how to implement handlers. By default the handlers will return an empty response.

Operation                     | Description
------------------------------|------------
handleAccepted                | Fill In
handleConflict                | Fill In
handleCreated                 | Fill In
handleForbidden               | Fill In
handleGone                    | Fill In
handleMalformed               | Fill In
handleMethodNotAllowed        | Fill In
handleMovedPermanently        | Fill In
handleMovedTemporarily        | Fill In
handleMultipleRepresentations | Fill In
handleNoContent               | Fill In
handleNotAcceptable           | Fill In
handleNotFound                | Fill In
handleNotImplemented          | Fill In
handleNotModified             | Fill In
handleOk                      | Fill In
handleOptions                 | Fill In
handlePreconditionFailed      | Fill In
handleRequestEntityTooLarge   | Fill In
handleSeeOther                | Fill In
handleServiceUnavailable      | Fill In
handleUnauthorized            | Fill In
handleUnknownMethod           | Fill In
handleUnprocessableEntity     | Fill In
handleUnsupportedMediaType    | Fill In
handleUriTooLong              | Fill In

[resources]: ../guides/machine-defining-resources.md
[graph]: ../guides/machine-understanding-the-execution-model.md
[handlers]: ../guides/machine-handlers-and-content-negotiation.md
