# Freya.Machine Reference

Freya.Machine uses a computation expression syntax to let you define resources in a declarative, type-safe way. It uses custom operations to try and provide a clean and tidy syntax for doing this, but this does mean that you need to know what those operations are called, and what their specificaton is!

The following table gives a complete reference to all *user configurable* operations for a Freya.Machine resource. All of the properties that can be set are a `Freya<T>` function of some kind. Some of those function signatures are very commonly used and so have their own name, like so:

* `FreyaMachineAction` -- a `Freya<unit>` function, simply an action with no return value.
* `FreyaMachineDecision` -- a `Freya<bool>` function, simply a "true/false" or "yes/no" function.
* `FreyaMachineHandler` -- a `FreyaMachineNegotiation -> Freya<FreyaMachineRepresentation` function - slightly more complicated, it's the final function called in a response, and is where you supply data (and some surrounding metadata) to be returned to the client. This is explained in more detail in the guide to [Freya Content Negotiation][conneg].

None of the operations are _"required"_, so all have defaults which will be used if not specified. These are also given in the table.

Other functions return a value specific to that operation, and are detailed in the table.

## User Configurable Operations

<table>
    <thead>
        <tr>
            <th>Operation</th>
            <th>Type</th>
            <th>Default</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>allowed</td>
            <td><code>FreyaMachineDecision</code></td>
            <td><code>true</code></td>
            <td>Called to determine whether the request is "allowed". A value of <code>false</code> returned from this function will cause the response to return a <em>403 Forbidden</em>. Note that this is different to <em>authorized</em>.</td>
        </tr>
    </tbody>
</table>

[conneg]: http://example.com
