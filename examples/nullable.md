# Nullable type attribute

The `nullable` type attribute can be used to outline the instance of this type *Value* MAY be unset (e.g. `null` or `nil`).

## Unset a `string` or an `array` type

### MSON

```mson
- keyA (string, nullable, required) - Value of this field can be either unset or of the `string` type
- keyB (array, nullable, required) - Value of this field can be either unset or of the `array` type
```

### JSON

```json
{
   "keyA": null,
   "keyB": null
}
```

```json
{
   "keyA": "bob",
   "keyB": [1, 2, 3]
}
```

## Unset a value in an `array` of strings

### MSON

```mson
- keyA (array, required)  - Value of this array can be either unset or of the `string` type
    - (string, nullable)
```

### JSON

```json
{
    "keyA": ["dave", "cat", null, "house"]
}
```
