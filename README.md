# Markdown Syntax for Object Notation
Proposal of a Markdown syntax for JSON & JSON Schema.

The purpose of this plain text formatting syntax is to provide a human and machine readable serialization and validation format, agnostic to the current markup formats.

Similarly to the original Markdown to HTML (markup) conversion the Markdown Syntax for Object Notation (hereafter MSON) MUST enable a conversion to its markup counter part(s).

While this document focuses primarily on JSON and JSON Schema it MUST be possible to produce an XML or YAML representation from the MSON as well.

## Example (Serialization)
> **NOTE:** Examples are using JSON (markup) documents from <http://json-schema.org/example1.html>

### JSON

```json
{
    "id": 1,
    "name": "A green door",
    "price": 12.50,
    "tags": ["home", "green"]
}
```

### MSON
#### Source

```
- id: 1
- name: A green door
- price: 12.50
- tags: home, green
```

#### Rendered Markdown

---

- id: 1
- name: A green door
- price: 12.50
- tags: home, green

---


## Example (Validation)

### JSON Schema

> **NOTE:**  This proposal covers only basic features of JSON Schema. At this moment it is out of the scope of this syntax to support all the JSON Schema keywords (such as `uniqueItems`, `exclusiveMinimum` etc.).


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

### MSON
#### Source


```
# Product 
A product from Acme's catalog

- id: 1 (integer) - The unique identifier for a product
- name: A green door (string) - Name of the product
- price: 12.50 (number)
- tags: home, green (optional, array of strings)
```

#### Rendered Markdown

---

#### Product 
A product from Acme's catalog

- id: 1 (integer) - The unique identifier for a product
- name: A green door (string) - Name of the product
- price: 12.50 (number)
- tags: home, green (optional, array of strings)

----

This syntax fully captures original JSON payload thanks to example values as well as most of the basic features of JSON schema while it stays media-type agnostic (e.g. you can generate a YAML or XML from it).


---



#### Advanced Array Techniques

```

- tags: home, 42 (optional, array)
    - Item (string)
    - Item (number)

- tags
    - Item
        - name (string)
        - description (string)
    - Item (number)

- tags (array of arrays)
    - Item: 1, 2, 3, 4 (array of numbers)
        - Item (number)


- name: Josh
- address: josh@icloud.com
- tags
    - Item
        - label: A
        - address: a.com
    - Item
        - label: B
        - address: b.com

```