# Markdown Syntax for Object Notation
Markdown syntax proposal for JSON & JSON Schema.

The purpose of format is to provide a media-type agnostic, human and machine readable serialization (and validation) format which can be represented various serialization formats.

While this document primarily focuses on JSON and JSON Schema it MUST be possible to produce an XML or YAML representation from the MSON notation.

This proposal covers only basic features of JSON Schema. At this moment it is out of the scope of this syntax to support all the JSON Schema keywords (such as `uniqueItems`, `exclusiveMinimum` etc.).

## Basic Example
As taken from <http://json-schema.org/example1.html>.

### JSON Payload

```json
{
    "id": 1,
    "name": "A green door",
    "price": 12.50,
    "tags": ["home", "green"]
}
```

### JSON Payload Schema

```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "title": "Product",
    "description": "A product from Acme's catalog",
    "type": "object",
    "properties": {
        "id": {
            "description": "The unique identifier for a product",
            "type": "integer"
        },
        "name": {
            "description": "Name of the product",
            "type": "string"
        },
        "price": {
            "type": "number",
            "minimum": 0,
            "exclusiveMinimum": true
        },
        "tags": {
            "type": "array",
            "items": {
                "type": "string"
            },
            "minItems": 1,
            "uniqueItems": true
        }
    },
    "required": ["id", "name", "price"]
}
```

### Proposed Syntax
#### Source

```
+ `product` ... A product from Acme's catalog
    + `id` (integer, `1`) ... The unique identifier for a product
    + `name` (string, `A green door`) ... Name of the product
    + `price` = `0` (number, optional, `12.50`)
    + `tags` (array, optional)
        + Items (string, `home`, `green`)
```

#### Rendered

+ `product` ... A product from Acme's catalog
    + `id` (integer, `1`) ... The unique identifier for a product
    + `name` (string, `A green door`) ... Name of the product
    + `price` = `0` (number, optional, `12.50`)
    + `tags` (array, optional)
        + Items (string, `home`, `green`)

This syntax fully captures original JSON payload thanks to example values as well as most of the basic features of JSON schema while it stays media-type agnostic (e.g. you can generate a YAML or XML from it).

### More Complex Example
This example is demonstrating nesting of objects.

```
+ `product` ... A product from Acme's catalog
    + `id` (integer) ... The unique identifier for a product
    + `name` (string, `A green door`) ... Name of the product
    + `price` = `0` (number, optional)
    + `author` ... Author of the product
        + `id` (integer) ... Id of the author
        + `name` (string) ... Name of the author 
        + `email` (string, optional) ... Email address of the author
    + `tags` (array)
        + Items
            + `name` (string) ... Name of the tag
            + `author_id` (integer) ... Id of the author
```