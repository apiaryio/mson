# MSON Tutorial

MSON is short for Markdown Syntax for Object Notation, and it's a way to represent data structures in a human readable plain text form. This tutorial will take you though the basics of MSON and how you can use MSON to describe a data structure.

> **NOTE:** **Additional MSON Resources**
>
> + [MSON Specification](https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md)
> + [Examples](https://github.com/apiaryio/mson#notational-conventions)

## Data structures in MSON

To get started, we're going to create a data structure in MSON to represent a Person which has a name.

A data structure to store a Person could be represented in MSON using the following:

```apib
# Person

+ name
```

Here we've declared our own named type called "Person". A heading is one or more `#` symbols followed by a string representing the name of our data structure. By default, named types like `Person` are object types.

An object is constructed with properties, and in this case we've listed them below the heading in a list. Lists are created by putting each list item on its own line and preceding each line with either a `+`, `*` or `-`.

In our `Person` data structure, we have a single property called `name`. Similarly to our Person definition, we can declare the type that our property is, with round brackets. For example, we can declare that our name is a string using `+ name (string)`. By default, properties on objects are string's unless specified to another type, as in our above example we have omitted the string type definition.

We can attach a description for our name property by following the type definition with a dash (`-`) followed by the description.

```apib
+ name (string) - The Person's name
```

We may also specify a sample value for the person's name by following the property name by a colon (`:`) and then the value.

```apib
+ name: Kyle (string) - The Person's name
```

**NOTE**: *It's important to note, that if a sample value contains reserved characters such as `:`, `(`,`)`, `<`, `>`, `{`, `}`, `[`, `]`, `_`, <code>&ast;</code>, `-`, `+`, `` ` `` then the sample value must be wrapped in a code-block using back-ticks:*

```apib
+ name: `Spencer-Churchill` (string) - The Person's name
```

So far we have used the `object` and `string` types. MSON includes a total of 6 base types. Three of these base types are primitive types such as `boolean`, `string` and `number`. There are also three structure types called `array`, `enum` and `object`. Let's use some of these new types to create another data structure.

```apib
# Company

+ name: Apiary
+ founder (Person)
+ founded: 2011 (number) - The year in which the company was founded
```

### Inheritance

When declaring a named type, you may also inherit from another type. For example, we could declare a data structure called "Administrator" which inherits from the `Person` type.

```apib
# Administrator (Person)

+ role (string) - The administrators role
```

When inheriting from another data structure, all of the parents properties will be inherited. For example, our `Administrator` structure will contain the `name` property from our previous definition.

#### Nesting

It's also possible to nest other data structures directly without a data structure instead of referencing another type as we have done above between our `Company` and `Person`.

```apib
# Company

+ name: Apiary
+ founder (Person)
+ founded: 2011 (number) - The year in which the company was founded
+ address
    + street: 235 Ninth Street
    + city: San Francisco
    + state: California
```

In this example, we've declared a property called `address` which is a nested structure within our `Company`.

**NOTE**: *In this case, since we have inline properties within our address property, the default type for the address is object instead of string.*

## Additional Learning

You can find more [examples](https://github.com/apiaryio/mson#example-1) of MSON, along with additional information from the [MSON Specification](https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md).

