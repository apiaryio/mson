# JSON Hypertext Application Language
<http://tools.ietf.org/html/draft-kelly-json-hal-06>

## Hal Resource (object)
Hal Resource is the base type for all HAL resource models. It contains two predefined elements containing hypertext links of the given resource and embedded resources. Derived resource models may add additional elements to specify the state of the resource.

### Properties

- `_links` (object)

    For each link relation the _links dictionary contains one or many link entries

    - Properties

        - {(Relation)} (One or many: Link)

- `_embedded` (object)

    For each embedded resource there exists an entry in a dictionary which uses the relation type of the embedded resource to the surrounding resource as key. The value can be one or many Resource objects.

    - Properties

        - {(Relation)} (One or many: Hal Resource)

## Relation (string)
HAL introduces for Link relation types an extended representation of RFC 5988:

- Registered link relation types use their registered string
- Custom link relation types should be (HTTP) URI, which should point to HTML documentation for that relation type.
- The reserved (but unregistered) relation type curies can get used for a compact URI representation in CURIE Syntax for custom link relation types.

- (string) 
    - min: `1`

## Link (object)
A LinkObject specifies a link to a target resource

### Properties

- `href` (Href, required) - The link URI or URI template

- `templated` (bool) - Indicates if the href attribute contains a URI Template
    - default: `false`

- `type` (string) - Gives a hint on the expected media type of the target resource

- `deprecation` (bool) - Indicates that the link is deprecated
    - default: `false`

- `name` (string) - A secondary key for selecting one out of multiple links

- `profile` (string) - A URI providing a hint about the profile of the target resource

- `title` (string) - A label for the link

- `hreflang` (string) - A BCP 47 language tag identifying the language of the target resource

## One or many (one of): T
The generic type `One or many` allows to model a data type that either contains a single entry of a given type or a collection of elements of the same type.

- (one of)
    - (T)
    - (array: T)
        - min: `1`
