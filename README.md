# Markdown Syntax for Object Notation
This document is a proposal of Markdown syntax for JSON & JSON Schema.

## What? 
MSON is a plain text, human and machine readable, description format for common markup formats such as JSON, XML or YAML.

## What for?
The aim of this description format is to facilitate the discussion (and thus validation) of data structures. The format, being agnostic to the common markup formats, suites well the "resource & representations" and "content negotiation" scenarios. 

In addition this format also offers (limited) serialization functionality.

Similarly to the original Markdown to HTML (markup) conversion the Markdown Syntax for Object Notation (hereafter MSON) enables a conversion to other markup formats. 

## Who & Why? 
This format is being developed by [@zdne][] at [@apiaryio][] as a part of [API Blueprint][] syntax to provide a means for description and validation of HTTP payloads, DRY media-type agnostic resource descriptions and to simplify content-negotiation.

> **NOTE**: While this document focuses primarily on JSON and JSON Schema it MUST be possible to produce an XML or YAML representation from the MSON as well.

## Example

#### Source MSON

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

#### JSON Representation

```json
{
    "id": 1,
    "name": "A green door",
    "price": 12.50,
    "tags": ["home", "green"]
}
```

> **NOTE:** Escaping question? Look [here](#escaping).

> **NOTE:** Examples are using JSON (markup) documents from <http://json-schema.org/example1.html>

## Example

#### Source MSON

```
# Product 
A product from Acme's catalog

- id: 1 (integer) - The unique identifier for a product
- name: A green door (string) - Name of the product
- price: 12.50 (number)
- tags: home, green (optional, array of strings)
```

#### Rendered Markdown

#### Product 
A product from Acme's catalog

- id: 1 (integer) - The unique identifier for a product
- name: A green door (string) - Name of the product
- price: 12.50 (number)
- tags: home, green (optional, array of strings)

#### JSON Schema Representation

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

> **NOTE:** In addition to the above schema the source MSON also captures the serialized data of the original JSON!

> **NOTE:** This proposal covers only basic features of JSON Schema. At this moment it is out of the scope of this syntax to support all the JSON Schema keywords (such as `uniqueItems`, `exclusiveMinimum` etc.).


---

## Escaping
Markdown [code span][] element syntax (`` ` ` ``) is used to escape properties and values when needed. The use of code span is optional unless needed. A fully escaped first example would be:

#### Source MSON

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

## More description?
In the case where one-liner description is not enough a mutli-paragraph list item is the way to go.

#### Source MSON

```
- id: 1 (integer) - The unique identifier for a product
- name: A green door (string) 

    **Name of the product**

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
    Maecenas elementum in odio sit amet ultrices. 
    Phasellus placerat, nisl vel aliquam laoreet, quam nisi tempus mauris, non rhoncus arcu dolor sed erat. 
    Sed vestibulum nisl placerat erat varius, eget tristique nibh volutpat. Vestibulum ut dui lacus. 

    Sed sed lacus a arcu vehicula ultricies sed vel nibh. Mauris id cursus felis. 
    Suspendisse pellentesque justo ac erat tempor lobortis. 
    Proin aliquam adipiscing dolor at dictum. **Nulla facilisi**. 
    Donec venenatis velit eget posuere malesuada. Aenean vel mi nisl. 

    Interdum et malesuada fames ac ante ipsum primis in faucibus. 

- price: 12.50 (number)
- tags: home, green (optional, array of strings)
```

#### Rendered Markdown

- id: 1 (integer) - The unique identifier for a product
- name: A green door (string) 

    **Name of the product**

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
    Maecenas elementum in odio sit amet ultrices. 
    Phasellus placerat, nisl vel aliquam laoreet, quam nisi tempus mauris, non rhoncus arcu dolor sed erat. 
    Sed vestibulum nisl placerat erat varius, eget tristique nibh volutpat. Vestibulum ut dui lacus. 

    Sed sed lacus a arcu vehicula ultricies sed vel nibh. Mauris id cursus felis. 
    Suspendisse pellentesque justo ac erat tempor lobortis. 
    Proin aliquam adipiscing dolor at dictum. **Nulla facilisi**. 
    Donec venenatis velit eget posuere malesuada. Aenean vel mi nisl. 

    Interdum et malesuada fames ac ante ipsum primis in faucibus. 

- price: 12.50 (number)
- tags: home, green (optional, array of strings)

## Advanced Arrays
Some additional examples some more complex arrays, such as arrays of mixed objects or arrays of arrays

### Array of mixed primitive types

#### Source MSON

```
- tags: home, 42 (optional, array)
    - Item (string)
    - Item (number)
```

#### Rendered Markdown

- tags: home, 42 (optional, array)
    - Item (string)
    - Item (number)

### Array of mixed objects

#### Source MSON

```
- tags
    - Item
        - name (string)
        - description (string)
    - Item (number)
```

#### Rendered Markdown

- tags
    - Item
        - name (string)
        - description (string)
    - Item (number)

### Array of Arrays

#### Source MSON

```
- tags (array of arrays)
    - Item: 1, 2, 3, 4 (array of numbers)
        - Item (number)
```

#### Rendered Markdown

- tags (array of arrays)
    - Item: 1, 2, 3, 4 (array of numbers)
        - Item (number)

[API Blueprint]: https://github.com/apiaryio/api-blueprint
[code span]: http://daringfireball.net/projects/markdown/syntax#code
[@zdne]: https://github.com/zdne
[@apiaryio]: https://github.com/apiaryio