# Markdown Syntax for Object Notation
This document is a proposal of Markdown syntax for JSON & JSON Schema.

## What? 
MSON is a plain text, human and machine readable, description format for common markup formats such as JSON, XML or YAML.

## What for?
The aim of this description format is to facilitate the discussion (and thus validation) of data structures. The format, being agnostic to the common markup formats, suites well the "resource & representations" and "content negotiation" scenarios. 

In addition this format also offers (limited) serialization functionality.

Similarly to the original Markdown to HTML (markup) conversion the Markdown Syntax for Object Notation (hereafter MSON) enables a conversion to other markup formats. 

## Who & Why? 
This format is being developed by [@zdne][] at [Apiary][] as a part of [API Blueprint][] syntax to provide a means for description and validation of HTTP payloads, DRY media-type agnostic resource descriptions and to simplify content-negotiation.

> **NOTE**: While this document focuses primarily on JSON and JSON Schema it MUST be possible to produce an XML or YAML representation from the MSON as well.

## Example 1

#### JSON

```json
{
    "id": 1,
    "name": "A green door",
    "price": 12.50,
    "tags": ["home", "green"]
}
```

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

---

> **NOTE:** Escaping question? Look [here](#escaping).

## Example 2

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

#### MSON

```
# Product 
A product from Acme's catalog

## Properties

- id: 1 (number, required) - The unique identifier for a product
- name: A green door (string, required) - Name of the product
- price: 12.50 (number, required)
- tags: home, green (array: string)
```

#### Rendered Markdown

##### Product
A product from Acme's catalog

###### Properties

- id: 1 (number, required) - The unique identifier for a product
- name: A green door (string, required) - Name of the product
- price: 12.50 (number, required)
- tags: home, green (array: string)

---

> **NOTE:** In addition to the above schema the source MSON also captures the serialized data of the original JSON!

> **NOTE:** This proposal covers only basic features of JSON Schema. At this moment it is out of the scope of this syntax to support all the JSON Schema keywords (such as `uniqueItems`, `exclusiveMinimum` etc.).

> **NOTE:** Optional is the default for a property. Use `required` for required properties.

## Quick Links

- [Primitive types](#primitive-types)
- [Composite Types](#composite-types)
- [Objects & Arrays](#objects-arrays)
- [Advanced Objects](#advanced-objects)
- [Advanced Arrays](#advanced-arrays)
- [Escaping](#escaping)
- [Mutliline Description](#multiline-description)
- [Variable Property Name](#variable-property-name)
- [MSON Entity Definition](#mson-entity-definition)
- [Referencing](#referencing)
- [Mixins](#mixins)

## Primitive types
Following are the primitive data types of MSON entities:

- bool (boolean)
- number
- string

## Composite Types
MSON entity types composed of one or more other types are 

- array
- object
- one of

    The _one of_ type represent a choice of types for an MSON entity value. The types are mutually exclusive. 

#### MSON
```
- (one of)
    - (number)
    - (string)
```

## Objects & Arrays
By default, a Markdown list item is considered to be an object property:

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

#### MSON
```
- address
    - street
    - city
    - state
```

If a markdown list items represent array values the type of parents property must be explicitly set to array:

#### JSON
```json
{ 
    "address": ["street", "city", "state"]
}
```

#### MSON
```
- address (array)
    - street
    - city
    - state
```

Or, perhaps preferably: 

```
- address: street, city, state (array)
```

In this case, the type – `(array)` – can be omitted.

## Advanced Objects

### Non-uniform property
A Property which value can be of different types is defined of the `one of` composite type:

#### JSON

```json
{ "tag": "green" }
```

**or**

```json
{ "tag": { "tag_id": 1, "label": "green" } }
```

#### MSON

```
- tag
    - (one of)
        - green (string)
        - (object)
            - tag_id: 1
            - label: green
```

#### Rendered Markdown

- tag
    - (one of)
        - green (string)
        - (object)
            - tag_id: 1
            - label: green

---

### Mutually exclusive properites
By default all properties are optional and can be includuded in the object (any of). If there a choice of available properties use the `One of` keyword:

#### JSON
```json
{ "a": null, "b1": null, "c": null }
```

**or**

```json
{ "a": null, "b2": null, "c": null }
```

#### MSON
```
- a
- One of
    - b1
    - b2
- c
```

#### Rendered Markdown

- a
- One of
    - b1
    - b2
- c

---

## Advanced Arrays
### Array of mixed types

#### JSON

```json
{ "tags": ["hello", 42] }
```

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

---

#### JSON

```json
[{ "name": "snow", "description": null }, 42]
```

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

---

### Array of Arrays

#### JSON

```json
[[1, 2, 3, 4]]
```

#### MSON

```
- (array)
    - 1, 2, 3, 4 (array: number)
```

#### Rendered Markdown

- (array)
    - 1, 2, 3, 4 (array: number)

---

## Escaping
Markdown [code span][] element syntax (`` ` ` ``) is used to escape properties and values when needed. The use of code span is optional unless needed. A fully escaped first example would be:

Element values and property names and values with _reserved characters_ or containing keywords MUST be escaped. 

#### Reserved Characters

`:`, `(`,`)`, `<`, `>`, `{`, `}`, `[`, `]`, `_`, `*`, `-`, `` ` `` 

#### Keywords 
Keywords are case insensitive:

`Element`, `Elements`, `Inherit`, `Inherits`, `Property`, `Properties`, `Attribute`, `Attributes`, `One of`, `Trait`, `Traits`

#### MSON

```
- `id`: `1`
- `name`: `A green door`
- `price`: `12.50`
- `tags`: `home`, `green`
```

#### Rendered Markdown

- `id`: `1`
- `name`: `A green door`
- `price`: `12.50`
- `tags`: `home`, `green`

---

## Mutliline Description
In the case where one-liner description is not enough a mutli-paragraph list item is the way to go.

#### MSON

```
- id: 1 (required, number) - The unique identifier for a product
- name: A green door (required, string) 

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
    
    Sed sed lacus a arcu vehicula ultricies sed vel nibh. Mauris id cursus felis. 

    Interdum et malesuada fames ac ante ipsum primis in faucibus.

    - unus
    - duo
    - tres
    - quattuor    

- price: 12.50 (required, number)
- tags: home, green (array)
```

For multi-line description of an array or object the `Elements` or `Properties` keyword is needed to avoid any possible clash with potential description list items: 

```
- tags (array)
    
    Lorem ipsum dolor sit amet, consectetur adipiscing elit.

    Sed sed lacus a arcu vehicula ultricies sed vel nibh. Mauris id cursus felis.

    Interdum et malesuada fames ac ante ipsum primis in faucibus.

    - unus
    - duo
    - tres
    - quattuor

    - Elements
        - home
        - green 
```

## Variable Property Name
Variable property name (key) is defined using braces `{}`. Note a variable property can't be required. 

#### JSON

```json
{
    "_links" {
        "self": {
            "href": "an uri"
        }
    }
}
```

#### MSON

```
- _links
    - {self}
        - href: an uri
```

#### Rendered Markdown

- _links
    - {self}
        - href: an uri

---

Additionaly a variable property name can specify its key type:

#### JSON
```json
{
    "1": "1",
    "2": "2",
    "3": "Fizz",
    "4": "4",
    "5": "Buzz"
}
```

#### MSON

```
- {(number)} (string) - FizzBuzz number & answer pair
```

#### Rendered Markdown

- {(number)} (string) - FizzBuzz number & answer pair

---

## MSON Entity Definition
Top-level MSON entities definition

#### MSON 

```
# Address (object)
- street
- city
- state
- zip
```

> **NOTE:** The `object` as default type can be omitted. 

#### MSON 

```
# Address (one of)
- (one of)
    - z@apiary.io (string)
    - (object)
        - street
        - city
        - state
        - zip
```

## Referencing
Anywhere a type is expected, an MSON entity can be referenced. At its simplest an MSON entity is referenced by its name only:

#### JSON
```json
{
    "fist_name": null,
    "last_name": null,
    "address": {
        "street": null,
        "city": null,
        "state": null,
        "zip": null
    }
}
```

#### MSON

```
# Address (object)
- street
- city
- state
- zip

# User (object)
- fist_name 
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
- fist_name 
- last_name
- address (Address)

---

However both `[]()` and `[][]` [Markdown syntax](http://daringfireball.net/projects/markdown/syntax#link) are supported:

#### MSON

```
# User (object)
- fist_name 
- last_name
- address ([Address](#address-object))
```

```
# User (object)
- fist_name 
- last_name
- address ([Address][])
```

## Mixins
To inherit (mixin) object properties into another object use the `Inherit` keyword followed by a reference to the object.

#### JSON
```json
{
    "fist_name": null,
    "last_name": null,
    "street": null,
    "city": null,
    "state": null,
    "zip": null
}
```

#### MSON

```
# Address Object
- street
- city
- state
- zip

# User Object
- fist_name 
- last_name
- Inherit [Address]()
```

#### Rendered Markdown

##### Address Object
- street
- city
- state
- zip

##### User Object
- fist_name 
- last_name
- Inherit [Address]()

---

[API Blueprint]: https://github.com/apiaryio/api-blueprint
[code span]: http://daringfireball.net/projects/markdown/syntax#code
[@zdne]: https://github.com/zdne
[Apiary]: http://apiary.io
