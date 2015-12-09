# Rendering recommendation

These are the recommendation for **rendering an instance of a `nullable` type without any other sample value provided**.

### JavaScript, JSON, YAML

`null` value

### JSON Schema

```json
{ "type": [ "string", "null" ] }
```

(instance of a nullable string type)

### XML

`xsi:nil="true"` attribute

### Swift, GO

`nil` value

### C++, C, PHP, SQL

`NULL` value

### Python

`None` value

### Ruby

`nil` value

### PERL

`undef` value

## Rendering with other type attributes

### optional

```mson
- key (string, optional)
```

The instance of key will be omitted in the rendered JSON:

```json
{}
```

### optional, nullable

```mson
- key (string, optional, nullable)
```

The value of the instance will be unset in the rendered JSON:

```json
{
  "key": null
}
```

### required

```mson
- key (string, required)
```

The value of instance will be empty in the rendered JSON:

```json
{
  "key": ""
}
```

### required, nullable

```mson
- key (string, required, nullable)
```

The value of instance of key will be unset in the rendered JSON:

```json
{
  "key": null
}
```

