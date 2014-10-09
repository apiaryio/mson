# ALPS MSON Structure
Describes an [ALPS][] document in [MSON][].

## ALPS Document ([ALPS Base][])
An ALPS document contains a machine-readable collection of identifying strings and their human-readable explanations.

### Properties
- `version`: 1.0 (string, default)

## ALPS Base
Base type for select [ALPS][] elements.

### Properties
- `links` (array[[Link][]], optional) - Links to other resources.
- `descriptors` (array[[Descriptor][]]) - Data and transition definitions.
- `doc` ([Text][], optional) - A description of the element.
- `ext` ([Extension][]) - Author-specific extension to an element.

## Link
Identifies a link between an ALPS element and some other (possibly external) resource.

### Properties
- `rel` (string) - A [RFC5988][] approved relation type.
- `href` ([URL][]) - The URL of the resource described by the associated `rel`.

## Descriptor ([ALPS Base][])
Defines the semantics of specific data elements or state transitions that MAY exist in an associated representation.

### Properties
- One Of
    - `id` (string, optional) - A document-wide unique identifier for the descriptor element.
    - `href` (enum, optional)
        - ([URL][]) - References an external ALPS descriptor element.
        - ([Fragment][]) - References a local descriptor element.
    - Properties
        - `id` - Not dry. Need access to re-use inline elements.
        - `href` - Ditto.
- `name` (string, optional) - A name for a descriptor element.
- `type` (enum[string], optional) - The type of hypermedia control within the associated representation.
    - `semantic` (default) - A state (data) element.
    - `safe` 
    - `idempotent`
    - `unsafe`
- `rt` (string, optional) - ID of another descriptor in the document. Valid for `safe`, `idempotent` and `unsafe` types.

## Extension
Defines additional properties that extend [ALPS][] elements.

### Properties
- `id` - A document-wide unique identifier for the extension element.
- `href` ([URL][]) - References the definition of the extension.
- `value` (string) - Specifies the value.

## Text
Free-form, usually human-readable, text.

### Properties
- `format` (enum[string]) - Indicates how the content should be parsed.
    - `text` (default)
    - `html`
    - `asciidoc`
- `href` ([URL][]) - References a related document containing human readable text.
- `content` (string) - The text contents.

## URL (string)
A resolvable [RFC3986] URL.

## Fragment (string)
A relative [RFC3986] URI fragment with a value of the ID of a local descriptor.

[ALPS]: http://tools.ietf.org/html/draft-amundsen-richardson-foster-alps-00
[MSON]: ../MSON%20Specification.md
[RFC5988]: https://tools.ietf.org/html/rfc5988
[RFC3986]: http://tools.ietf.org/html/rfc3986

[ALPS Base]: #alps-base
[Link]: #link
[Descriptor]: #descriptor-alps-base
[Text]: #text
[Extension]: #extension
[URL]: #url-string
[Fragment]: #fragment-string
