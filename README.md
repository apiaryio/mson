# Markdown Syntax for Object Notation
This document provides an introduction to Markdown Syntax for Object Notation (MSON), a Markdown syntax
compatible with describing JSON and JSON Schema.

## What?
MSON is a plain-text, human and machine readable, description format for describing data structures in common markup
formats such as JSON, XML or YAML.

## What for?
The aim of this description format is to facilitate the discussion (and thus validation) of data structures. The format,
being agnostic to the common markup formats, is well suited for "resource & representations" and "content negotiation"
scenarios.

In addition, this format also offers (limited) serialization functionality.

Similar to the original Markdown to HTML (markup) conversion, MSON enables conversion to other markup formats.

## Who & Why?
This format is being developed by [Apiary][] as a part of the [API Blueprint][] syntax to provide a means
for description and validation of HTTP payloads and DRY, media-type agnostic, resource descriptions and to simplify
content-negotiation.

> **NOTE**: While this document focuses primarily on JSON and JSON Schema examples, the underlying specification will
> ultimately allow producing XML or YAML representations from MSON.

## Notational Conventions
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and
"OPTIONAL" in this document are to be interpreted as described in [RFC2119][].

## Example 1
A simple `object` structure and it's associated JSON expression.

#### MSON

```
- id: 1
- name: A green door
- price: 12.50
- tags: home, green
```

#### Rendered Markdown

- id: 1
- name: A green door
- price: 12.50
- tags: home, green

#### JSON

```json
{
    "id": "1",
    "name": "A green door",
    "price": "12.50",
    "tags": [ "home", "green" ]
}
```

>**NOTE:** By default, a Markdown list item is considered to be a `string` type.

---

## Example 2
A Named Type with it's associated JSON expression and JSON Schema.

#### MSON

```
# Product
A product from Acme's catalog

## Properties

- id: 1 (number, required) - The unique identifier for a product
- name: A green door (string, required) - Name of the product
- price: 12.50 (number, required)
- tags: home, green (array[string])
```

#### Rendered Markdown

##### Product
A product from Acme's catalog

###### Properties

- id: 1 (number, required) - The unique identifier for a product
- name: A green door (string, required) - Name of the product
- price: 12.50 (number, required)
- tags: home, green (array[string])

#### JSON

```json
{
    "id": 1,
    "name": "A green door",
    "price": 12.50,
    "tags": [ "home", "green" ]
}
```

>**NOTE:** The `id` and `price` sample values are numbers given the explicit declaration of their base type of `number`
> vs. the default of `string` as in [Example 1](#example-1).

#### JSON Schema

```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "title": "Product",
    "description": "A product from Acme's catalog",
    "type": "object",
    "properties": {
        "id": {
            "description": "The unique identifier for a product",
            "type": "number"
        },
        "name": {
            "description": "Name of the product",
            "type": "string"
        },
        "price": {
            "type": "number"
        },
        "tags": {
            "type": "array",
            "items": {
                "type": "string"
            }
        }
    },
    "required": ["id", "name", "price"]
}
```

> **NOTE:** This proposal covers only basic features of JSON Schema. At this moment, it is out of the scope of this
> syntax to support all the JSON Schema keywords (such as `uniqueItems`, `exclusiveMinimum`, etc.).

---

## MSON Language Specification
The rest of this document covers some advanced syntax examples. Refer to the [MSON Language Specification][] for the
complete MSON Grammar Reference.

## Quick Links

- [MSON Language Specification][]
- [Objects & Arrays](#objects--arrays)
- [Advanced Objects](#advanced-objects)
- [Advanced Arrays](#advanced-arrays)
- [Escaping](#escaping)
- [Multi-line Descriptions](#multi-line-descriptions)
- [Variable Property Name](#variable-property-name)
- [Type Definition](#type-definition)
- [Referencing](#referencing)
- [Mixins](#mixins)

## Objects & Arrays
By default, a Markdown list item with a nested Markdown list is considered to be an `object` structure:

#### MSON
```
- address
    - street
    - city
    - state
```

#### JSON
```json
{
    "address" : {
        "street": null,
        "city": null,
        "state": null
    }
}
```

If a Markdown list's items are intended to be literals (represent array values), the type of the parent list item MUST
be explicitly set to `array`:

#### MSON
```
- address (array)
    - street
    - city
    - state
```

Or, alternately:

```
- address: street, city, state (array)
```

In this latter case, using a comma-separated list of values, the type `(array)` is implied and thus MAY be omitted.

#### JSON
```json
{
    "address": [ "street", "city", "state" ]
}
```

> **NOTE:** The values "street", "city", and "state" are solely sample values of the `address` array in this example.
> No constraints are implied on the quantity or types of values in such an array other than that they MAY be `string`
> literals.

## Advanced Objects

### Non-uniform Property
A Property whose value can be of different types is defined by the `enum` structure type:

#### MSON

```
- tag (enum)
    - green (string)
    - (object)
        - tag_id: 1
        - label: green
```

#### Rendered Markdown

- tag (enum)
    - green (string)
    - (object)
        - tag_id: 1
        - label: green

#### JSON

```json
{
    "tag": "green"
}
```

**or**

```json
{
    "tag": {
        "tag_id": "1",
        "label": "green"
    }
}
```

>**NOTE:** In an `enum` structure, in contrast to an `array` type structure, a value like "green" is a fully-qualified
>value of the enumeration vs. being a sample value.

---

### Mutually Exclusive Properties
By default, all properties are optional and may or may not be included in the object. If there is a choice of
mutually exclusive properties available MSON defines a `One Of` type:

#### MSON
```
- city
- One Of
    - state
    - province
- country
```

#### Rendered Markdown

- city
- One Of
    - province
    - state
- country

#### JSON
```json
{
    "street": null,
    "province": null,
    "country": null
}
```

**or**

```json
{
    "street": null,
    "state": null,
    "country": null
}
```

>**NOTE:** Because an `enum` type MUST define a list of types vs. properties and by default an un-nested Markdown list
defines properties of an implied `object` structure, the `One Of` type declaration MUST only be used to indicate
mutually exclusive properties in an `object` structure.

---

## Advanced Arrays

### Array of Mixed Types

#### MSON

```
- tags (array)
    - hello (string)
    - 42 (number)
```

#### Rendered Markdown

- tags (array)
    - hello (string)
    - 42 (number)

#### JSON

```json
{
    "tags": [ "hello", 42 ]
}
```

---

#### MSON

```
- (array)
    - (object)
        - name: snow (string)
        - description (string)
    - 42 (number)
```

#### Rendered Markdown

- (array)
    - (object)
        - name: snow (string)
        - description (string)
    - 42 (number)

#### JSON

```json
[
    {
        "name": "snow",
        "description": null
    },
    42
]
```

---

### Array of Arrays

#### MSON

```
- (array)
    - 1, 2, 3, 4 (array[number])
```

#### Rendered Markdown

- (array)
    - 1, 2, 3, 4 (array[number])

#### JSON

```json
[
    [ 1, 2, 3, 4 ]
]
```

---

## Multi-line Descriptions
In the case where a one-liner description is not sufficient, a multi-paragraph text block is the way to go.

#### MSON

```
- id: 1 (number, required) - The unique identifier for a product
- name: A green door (string, required)

    Lorem ipsum dolor sit amet, consectetur adipiscing elit.

    Sed sed lacus a arcu vehicula ultricies sed vel nibh. Mauris id cursus felis.

    Interdum et malesuada fames ac ante ipsum primis in faucibus.

    - unus
    - duo
    - tres
    - quattuor

- price: 12.50 (number, required)
- tags: home, green (array)
```

For a multi-line description of a structure type, an `Items`, `Members` , or `Properties` keyword MUST be used to avoid
conflict with potential list item values that are part of the description:

```
- tags (array)

    Lorem ipsum dolor sit amet, consectetur adipiscing elit.

    Sed sed lacus a arcu vehicula ultricies sed vel nibh. Mauris id cursus felis.

    Interdum et malesuada fames ac ante ipsum primis in faucibus.

    - unus
    - duo
    - tres
    - quattuor

    - Items
        - home
        - green
```

>**NOTE:** Unus ... quattuor are considered part of the text block vs. defining items of the array.

## Escaping
Markdown [code span][] element syntax (`` ` ` ``) is used to escape reserved Keywords that should just be interpretted
as literals as well as other constructs that may be erroneously interpreted by an MSON parser as meaningful.

For non-Keywords, a [code span][] can be used for formatting Markdown as an aesthetic preference.

#### MSON

```
- listing (object)

    Our real estate listing has different properties available.

    - `Properties`
        - This one.
        - That one.

    - Properties
        - description (string)
        - date_listed (string)
        - `some:location`: local (string)
```

#### Rendered Markdown

- listing (object)

    Our real estate listing has different properties available.

    - `Properties`
        - This one.
        - That one.

    - Properties
        - description
        - date_listed
        - `some:location`: local (string)

>**NOTE:** In this example, the first "Properties" string is part of the multi-line block description whereas the second
> defines the properties of the `listing` object.

---

## Variable Property Name
Variable property name (key) is defined using *italics*. Note that a variable property cannot be required.

#### MSON

```
- _links
    - *self*
        - href: a URI
```

#### Rendered Markdown

- _links
    - *self*
        - href: a URI

#### JSON

```json
{
    "_links": {
        "self": {
            "href": "..."
        },
        "users": {
            "href": "..."
        }
    }
}
```

---

## Type Definition
Additional Named Types can be defined using a Markdown header:

#### MSON

```
# Address (object)
Description is here! Properties to follow.

## Properties
- street
- state
- zip
```

The same entity defined as an `address` property:

```
- address (object)

    Description is here! Properties to follow.

    - Properties
        - street
        - state
        - zip
```

The same `address` property referencing the previously Named type:

```
- address (Address)
```

## Referencing
Anywhere a type is expected, a top-level MSON Named Type can be referenced.

#### MSON

```
# Address (object)
- street
- city
- state
- zip

# User (object)
- first_name
- last_name
- address (Address)
```

#### Rendered Markdown

##### Address (object)
- street
- city
- state
- zip

##### User (object)
- first_name
- last_name
- address (Address)

#### JSON

```json
{
    "first_name": null,
    "last_name": null,
    "address": {
        "street": null,
        "city": null,
        "state": null,
        "zip": null
    }
}
```

---

## Mixins
To include (mixin) values or properties in another Named Type use the `Include` keyword followed by a reference to the  
MSON Named Type.

#### MSON

```
# Address Object
- street
- city
- state
- zip

# User Object
- first_name
- last_name
- Include Address
```

#### Rendered Markdown

##### Address Object
- street
- city
- state
- zip

##### User Object
- first_name
- last_name
- Include Address

#### JSON

```json
{
    "first_name": null,
    "last_name": null,
    "street": null,
    "city": null,
    "state": null,
    "zip": null
}
```

>**NOTE:** A mixin can only use a Named Type that is derived from the same type of structure including the mixin.
> Further, the parent structure inherits all the members of the included Named Type and maintains the order the
> members were originally defined.

[API Blueprint]: https://github.com/apiaryio/api-blueprint
[code span]: http://daringfireball.net/projects/markdown/syntax#code
[@zdne]: https://github.com/zdne
[Apiary]: http://apiary.io
[RFC2119]: https://www.ietf.org/rfc/rfc2119
[MSON Language Specification]: MSON%20Specification.md
