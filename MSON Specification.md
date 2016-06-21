# MSON Specification
Markdown Syntax for Object Notation (MSON) is a plain-text syntax for the description and validation of data structures.

## Quick Links

- 1 [How to Read the Grammar][]
    - 1.1 [Markdown Syntax][]
    - 1.2 [Notational Conventions][]
    - 1.3 [Promises][]
- 2 [Types][]
    - 2.1 [Base Types][]
        - 2.1.1 [Primitive Types][]
        - 2.1.2 [Structure Types][]
    - 2.2 [Named Types][]
    - 2.3 [Member Types][]
        - 2.3.1 [Property Member Types][]
        - 2.3.2 [Value Member Types][]
- 3 [Type Declaration][]
    - 3.1 [Named Declaration][]
        - 3.1.1 [Generic Named Declaration][]
    - 3.2 [Property Member Declaration][]
        - 3.2.1 [Property Name][]
        - 3.2.2 [Variable Property Name][]
    - 3.3 [Value Member Declaration][]
    - 3.4 [Value Definition][]
        - 3.4.1 [Value][]
        - 3.4.2 [Literal Value][]
        - 3.4.3 [Variable Value][]
    - 3.5 [Type Definition][]
        - 3.5.1 [Type Specification][]
            - 3.5.1.1 [Variable Type Specification][]
        - 3.5.2 [Type Name][]
            - 3.5.2.1 [Variable Type Name][]
            - 3.5.2.2 [Wildcard Type Name][]
        - 3.5.3 [Type Attribute][]
    - 3.6 [Description][]
- 4 [Type Sections][]
    - 4.1 [Block Description][]
    - 4.2 [Member Type Group][]
        - 4.2.1 [Member Type Separator][]
    - 4.3 [Nested Member Type][]
    - 4.4 [Sample][]
    - 4.5 [Default][]
    - 4.6 [Validations][]
- 5 [Type Inheritance][]
    - 5.1 [Mixin Type][]
    - 5.2 [One Of Type][]
    - 5.3 [Generic Named Type][]
    - 5.4 [Member Type Precedence][]
- 6 [Reserved Characters & Keywords][]
    - 6.1 [Characters][]
    - 6.2 [Keywords][]
    - 6.3 [Additional Keywords][]

## 1 How to Read the Grammar
- An arrow (→) mark grammar productions that can be read as "is defined by|is defined by a(n)"
- A double arrow (⇒) marks grammar productions that can be read as "contains|contains a(n)"
- _Italic_ text indicates syntactic categories
- A `code span` indicates literal characters, words and punctuations
- A pipe character (|) separates alternative grammar productions
- A following _[opt]_ indicates optional syntactic categories and literals

### 1.1 Markdown Syntax
Note this reference is using ATX-style headers (#) and hyphen-style lists (-) exclusively. However you MAY use
Setext-style headers and/or asterisk (*) or plus (+) style lists if you prefer.

### 1.2 Notational Conventions
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and
"OPTIONAL" in this document are to be interpreted as described in [RFC2119][].

### 1.3 Promises
By default, MSON explicitly defines data structures and the meaning of their members without making any assertion to the
exclusivity of structures or their members. Further, no assertion is made concerning the presence of defined
structures or members. Rather, MSON describes structures that MAY be observed versus what MUST be observed.

However, much like JSON Schema being able to restrict "additionalProperties", MSON allows annotating structures to
indicate when they are strictly defined to preclude other structures and members and when they have strict ordering.

## 2 Types
In MSON, data structures are described by header-defined and/or list-defined _Types_ and/or combinations thereof built
from a limited set of _[Base Types][]_. A particular _Type_ is defined by its _[Type Declaration][]_ and combination of
optional _[Type Sections][]_.

_Type_ → _[Type Declaration][]_

_Type_ ⇒ _[Type Sections][]_ _[opt]_

More specifically:

_Type_ → _[Type Declaration][]_

_Type_ ⇒ _[Block Description][]_ _[opt]_

_Type_ ⇒ _[Member Type Group][]_ _[opt]_ |  _[Nested Member Types][]_ _[opt]_

_Type_ ⇒ _[Sample][]_ _[opt]_

_Type_ ⇒ _[Validations][]_ _[opt]_

- Header-defined _[Named Type][]_

    ```
    # Person (object)
    A person.

    ## Properties
    - `first_name`
    - `last_name`
    - address
        - city
        - street
    ```

- List-defined _[Member Type][]_

    ```
    - person (object) - A person
        - `first_name`
        - `last_name`
        - address
            - city
            - street
    ```

    or, equivalently:

    ```
    - person (Person) - A person
    ```

### 2.1 Base Types
MSON defines a number of distinct base, case-insensitive _[Primitive][]_ and _[Structure Types][]_ from which all data
structures are built (sub-typed).

#### 2.1.1 Primitive Types
Applies to basic data structure and type definitions. _[Types][]_ that are sub-typed from _Primitive Types_ MUST NOT
contain a _[Member Type Group][]_ or _[Nested Member Types][]_.

- `boolean`

    Specifies a type with allowed values of `true` or `false`.

- `string`

    Specifies any string.

- `number`

    Specifies any number.

#### 2.1.2 Structure Types
Applies to recursive, composite data structure and type definitions. _[Types][]_ that are sub-typed from
_Structure Types_ MAY contain a _[Member Type Group][]_ or _[Nested Member Types][]_.

Structure Types may directly or indirectly contain recursive references to
themselves.

- `array`

    Specifies a list of items for values.

- `enum`

    Specifies an exclusive list of possible _[Values][]_ or _[Types][]_ for values.

- `object`

    Specifies a structure that contains properties as members.

### 2.2 Named Types
Users may extend _[Base Types][]_ to define new header-defined _Named Types_ that MAY be referenced to build other
_[Types][]_.

_Named Type_ → _[Named Declaration][]_ | _[Generic Named Declaration][]_

_Named Type_ ⇒ _[Type Sections][]_ _[opt]_

```
# Person (object)
- first_name
- last_name
```

### 2.3 Member Types
Markdown lists specify MSON _Member Types_, which define the structure and types of individual
members of _[Types][]_ built from _[Structure Types][]_. A _[Member Type][]_ that contains _[Nested Member Types][]_
defines an inner, anonymous type that specifies the structure of values of that particular member.

#### 2.3.1 Property Member Types
Define individual members of `object` type structures.

_Property Member Type_ → _[Property Member Declaration][]_

_Property Member Type_ ⇒ _[Type Sections][]_ _[opt]_

Any list of _[Property Member Types][]_ collectively MAY define an implied parent `object` type structure.

```
- person (object) - A person
    - `first_name`
    - `last_name`
    - address
- company (string)
```

Defines a `person` _Property Member Type_ with a value that is an explicit `object` type structure with members
`first_name`, `last_name`, and `address` and, at the same time, is a member together with `company` of another
implied `object` structure.

#### 2.3.2 Value Member Types
Define individual members of `array` or `enum` type structures.

_Value Member Type_ → _[Value Member Declaration][]_

_Value Member Type_ ⇒ _[Type Sections][]_ _[opt]_

```
- (array)
    - red (string) - A sample value
    - green (string)
```

## 3 Type Declaration
Defines a particular MSON _[Type][]_.

_Type Declaration_ → _[Named Declaration][]_ | _[Generic Named Declaration][]_ | _[Property Member Declaration][]_  | _[Value Member Declaration][]_

### 3.1 Named Declaration
Defines a _[Named Type][]_.

_Named Declaration_ → `#` _[Type Name][]_` `_[Type Definition][]_ _[opt]_

```
# Person (object)
```

### 3.1.1 Generic Named Declaration
Defines a _[Named Type][]_ that allows an italicized _[Variable Type Name][]_ to represent a _[Type Name][]_
at any location in the _[Type Specification][]_ of a _[Variable Type Specification][]_.

_Generic Named Declaration_ →  `#` _[Type Name][]_` `_[Variable Type Specification][]_

```
# One or Many (enum[*T*])
```

### 3.2 Property Member Declaration
Defines a _[Property Member Type][]_.

_Property Member Declaration_ → `-` _[Property Name][]_ `:` _[opt]_ _[Value Definition][]_ _[opt]_ `-` _[opt]_ _[Description][]_ _[opt]_

```
- person (object) - A person
```

The optional `:` is only applicable in the case where a _[Value Definition][]_ is present and includes a _[Value][]_.

```
- company: Apiary (string)
```

#### 3.2.1 Property Name
Defines the name of a property in an `object` type structure.

_Property Name_ → _[Literal Value][]_ | _[Variable Property Name][]_

```
- customer (object)
```

Defines a _[Property Member Declaration][]_ with a _Property Name_ "customer".

#### 3.2.2 Variable Property Name
Defines a _[Property Name][]_ that is associated with a specific _[Value Definition][]_ in an `object` type structure
that MAY be any arbitrary name in an actual implementation. The _[Value][]_ of the _Variable Property Name_ serves as
a sample.

_Variable Property Name_ → `*`_[Value Definition][]_`*`

In the case of specifying a _[Variable Property Name][]_, the  _[Value Definition][]_ MAY reference a _[Named Type][]_
that MUST be sub-typed from a `string` _[Primitive Type][]_.

```
*rel (Custom String)* (object)
```

Where `rel` is a sample value for the arbitrary _[Property Name][]_ of a _[Property Member Declaration][]_.

### 3.3 Value Member Declaration
Defines a _[Value Member Type][]_. A _Value Member Declaration_ MUST only be used to define structures of `array`
or `enum` _[Member Types][]_.

_Value Member Declaration_ → `-` _[Value Definition][]_ `-` _[opt]_ _[Description][]_ _[opt]_

The optional `-` is only applicable in the case where a _[Description][]_ is provided.

```
- (array)
```

### 3.4 Value Definition
Every _[Member Type][]_ MAY specify type information associated with values in a structure or member in a structure.
This includes a _Value Definition_ that may include literal or sample _[Values][]_ and a _[Type Definition]_, including
a _[Type Specification][]_ and/or _[Type Attributes][]_, of associated types.

_Value Definition_ → _[Value][]_ _[opt]_ _[Type Definition][]_ _[opt]_

A _Value Definition_ MUST include at least a _[Value][]_ or a _[Type Definition][]_. A _Value Definition_ of an
`object` _[Member Type][]_ MUST NOT specify a _[Value][]_.

```
5, 6 (array)
```

Defines a _Value Definition_ for an `array` type structure with sample values "5" and "6".

#### 3.4.1 Value
Defines either sample or actual value(s) in _[Member Types][]_ based on the associated _[Type Definition][]_ in a
_[Value Definition][]_.

_Value_ → _[Literal Value][]_ | _[Variable Value][]_ | _Values List_

_Values List_ → _Value_ | _Value_`,` _Values List_

A _Values List_ MUST only be used with `array` or `enum` _[Structure Types][]_ or _[Named Types][]_ derived from them.

By default:

- A _Value Definition_ that incorporates a _Values List_ but has no _[Type Definition][]_ implies an
`array` type structure.

```
- list: 1, 2, 3
```

is equivalent to:

```
- list: 1, 2, 3 (array)
```

Where "1", "2", and "3" are sample values of the `array` structure.

- A _Value Definition_ that incorporates a _Values List_ defines fully-qualified values of an `enum` type structure.

```
- colors: red, green (enum)
```

Where "red" and "green" are fully-qualified values of the `colors` enumeration.

#### 3.4.2 Literal Value
Literal value of a type instance. Some limitations apply (see [Reserved Characters & Keywords][]).

```
5
```

#### 3.4.3 Variable Value
Defines a _[Value][]_ that is not concrete using Markdown *italics*.

_Variable Value_ → `*`_[Literal Value][]_`*`

A _Variable Value_ MAY be used to indicate a _[Variable Property Name][]_ in a _[Property Member Declaration][]_ or
more generally, a sample value in a _[Value Definition][]_.

```
*rel*
```

#### 3.5 Type Definition
Explicitly specifies the type of a value in an MSON instance.

_Type Definition_ → `(`_[Type Specification][]_ _[opt]_ `,` _[opt]_ _Type Attributes List_ _[opt]_`)`

_Type Attributes List_ → _[Type Attribute][]_ | _[Type Attribute][]_`,` _Type Attributes List_

A _Type Definition_ MUST separate multiple items with commas and is order-independent.

```
(enum, optional)
```

#### 3.5.1 Type Specification
Defines sub-typed _[Base Types][]_ or _[Types][]_ for a particular _[Type][]_.

_Type Specification_ → _[Type Name][]_ | _[Type Name][]_`[`_Nested Type Name List_`]`

_Nested Type Name List_ →  _[Type Name][]_ | _[Type Name][]_`,` _Nested Type Name List_

An `array` or `enum` _[Value Definition][]_ MAY specify the _Type Specifications_ of implied
_[Nested Member Types][]_ members in-line using `[]` as a _Nested Type Name List_.

```
array[number, string]
```

Indicates a `array` type structure MAY include distinct numbers or strings as values.

##### 3.5.1.1 Variable Type Specification
Defines a variable _[Type Specification][]_ to indicate generic _[Named Types][]_.

_Variable Type Specification_ → _[Type Specification][]_

A _Variable Type Specification_ MUST include at least one _[Variable Type Name][]_.

```
# One or Many (enum[*T*])
```

#### 3.5.2 Type Name
References the name of a type in _[Base Types][]_ or _[Named Types][]_. Some limitations apply (see
[Reserved Characters & Keywords][]).

_Type Name_ → _[Literal Value][]_ | _[Variable Type Name][]_ | _[Wildcard Type Name][]_ | _Referenced Type Name_

_Referenced Type Name_ → *A valid [Markdown-style link][], where the link name MUST be a _[Literal Value][]_*

A _[Variable Type Name][]_ MUST only be used in two situations:
- As a _Type Name_ in a _[Type Definition]_ for a _[Generic Named Type][]_.
- As an associated _Type Name_ in _[Nested Member Types][]_ of the _[Generic Named Type][]_.

A _Referenced Type Name_ MAY be used to link to a _Type Name_ defined in another location in an MSON document
solely as a navigation convenience.

##### 3.5.2.1 Variable Type Name
An *italicized* variable that MAY be used in place of a _[Type Name][]_ for a _[Type Definition][]_ in a
_[Generic Named Declaration][]_.

_Variable Type Name_ → `*`_[Literal Value][]_`*`

##### 3.5.2.2 Wildcard Type Name
Indicates any type MAY be possible.

_Wildcard Type Name_ → `*`

A _Wildcard Type Name_ MAY only be used in a _[Type Specification][]_ for _[Member Types][]_.

#### 3.5.3 Type Attribute
Defines extra attributes associated with the implementation of a type.

- `required` - instance of this type is required
- `optional` - instance of this type is optional (default)
- `fixed`    - instance of this type structure and values are fixed
- `nullable` - instance of this type *Value* MAY be unset (e.g. `null` or `nil`). `nullable` may only be used within properties of objects.
- `sample`   - Alternate way to indicate a _[Value][]_ is a sample. See _[Sample][]_.
- `default`  - Alternate way to indicate a _[Value][]_ is a default. See _[Default][]_.

A `sample` _Type Attribute_ is mutually exclusive with `default`.

### 3.6 Description
Describes a _[Member Type][]_ in-line.

_Description_ → `-` _Markdown-formatted text_

```
- name: Andrew (string) - A Description
```

## 4 Type Sections
_[Types][]_ MAY contain any _[Block Description][]_, _[Member Type Group][]_, _[Nested Member Types][]_,
_[Sample][]_, _[Default][]_ and/or _[Validations][]_ _Type Sections_. Apart from a _[Block Description][]_, multiple
_[Type Sections][]_ MAY be specified and MAY have any order.

```
# Person (object)
An additional
multi-line description

- here
- there

## Sample
...

## Properties
- `first_name`
- `last_name`

## Validations
...

## Sample
Another ...
```

In general, _Type Sections_ nested under:
- A _[Named Declaration][]_ MUST use header-defined (`##`) variations.
- A _[Property Member Declaration][]_ or _[Value Member Declaration][]_ MUST use list-defined (`-`) variations.

### 4.1 Block Description
Describes a _[Type][]_ using a nested (multi-line) Markdown text block.

_Block Description_ → _Markdown-formatted text_

A _Block Description_ MUST be located directly under a _[Type Declaration][]_ and MUST start with text. Markdown lists
MAY be included in a _Block Description_ after initial text content and are considered part of the block text.

```
- name: Andrew (string) - A Description

    An additional
    multi-line description.

    - here
    - there

    More text.
```

Note that `here` and `there` are NOT _[Nested Member Types][]_ but rather are part of a Markdown list in the
_Block Description_.

A _[Member Types][]_ _Block Description_ MUST escape _[Member Type Separator][]_ _[Keywords][]_ as a `code span`
when used in a Markdown list.

```
- person (object)
    This person does not have:

    - `Properties`
        - first_name
        - last_name

    - Properties
        - `given_name`
        - surname
```

### 4.2 Member Type Group
A _Member Type Group_ delineates _[Nested Member Types][]_ and MUST be used when other _[Type Sections][]_ are present.

_Member Type Group_ → `-` _[Member Type Separator][]_ | `##` _[Member Type Separator][]_

_Member Type Group_ ⇒ _Nested Member Types_

In order to specify _[Nested Member Types][]_ after a _Block Description_, _[Types][]_ MUST use an appropriate
_[Member Type Group][]_ to indicate the end of the _Block Description_ and the beginning of a
_[Nested Member Types][]_ list.

- Named Type

    A header-defined (`##`) _[Member Type Group][]_ SHOULD be nested one additional header level under the associated
    _[Named Type][]_, when required.

    ```
    # Person (object)
    An additional
    multi-line description

    - here
    - there

    ## Properties
    - `first_name`
    - `last_name`
    ```

- Member Type

    A list-defined (`-`) _[Member Type Group][]_ SHOULD be nested one indentation level under the associated
    _[Member Type][]_.

    ```
    - person (object)

        An additional
        multi-line description

        - here
        - there

        - Properties
            - `first_name`
            - `last_name`
    ```

A _Member Type Group_ of an appropriate type MUST be used to define _[Nested Member Types][]_ whenever any other
_[Type Section][]_ is specified at the same level.

- Without a _[Block Description][]_

    - _[Nested Member Types][]_ SHOULD be nested directly under a _[Type Declaration][]_ whenever possible.
    - _[Nested Member Types][]_ MAY be used without a _Member Type Group_ in a _[Named Type][]_, even if other
    _[Type Sections][]_ are present, if they are nested directly under the _[Type Declaration][]_.


    ```
    # Person (object)
    - `first_name`
    - `last_name`

    ## Sample
    ...
    ```
- With a _[Block Description][]_

    A _Member Type Group_ MUST be used to define _[Nested Member Types][]_.

    ```
    # Person (object)
    Just an ordinary person

    ## Properties
    - `first_name`
    - `last_name`
    ```

#### 4.2.1 Member Type Separator
Defines the names of separators to indicate the beginning of a section of _[Nested Member Types][]_.

_Member Type Separator_ →  `Items` | `Members` | `Properties`

- Array Structures - MUST use `Items` for a _Member Type Separator_

- Enum Structures - MUST use `Members` for a _Member Type Separator_

- Object Structures - MUST use `Properties` for a _Member Type Separator_


### 4.3 Nested Member Types
_[Types][]_ built from _[Structure Types][]_ MAY contain _Nested Member Types_, which are defined using nested Markdown
lists of allowed _[Member Types][]_.

A _[Member Type][]_ that contains _Nested Member Types_ defines an inner, anonymous type that specifies the structure
of values of that particular member.

_Nested Member Types_ → _[Member Types][]_

By Default:

- Un-nested Member Types

    A _[Member Type][]_ that does not contain _Nested Member Types_ and does not contain a _[Type Definition][]_
    implies a `string` _[Type Specification][]_.

    ```
    - count: 1
    ```

    Implies:

    ```
    - count: 1 (string)
    ```

- Object Structures

    A _[Property Member Type][]_ without a _[Type Definition][]_ that contains _[Nested Member Types][]_ implies
    an `object` type structure.

    ```
    - address
        - city
        - state
    ```

    Implies:

    ```
    - address (object)
        - city
        - state
    ```

- Array Structures

    A _[Member Type][]_ with an `array` _[Type Definition][]_ that contains _[Nested Member Types][]_
    specifies an `array` type structure that MAY contain items of the specified type and
    sample values per the particular _[Value Definition][]_.

    ```
    - colors (array)
        - red (string)
        - 5 (number)
    ```

    Implies an `array` structure whose individual items MAY be strings or numbers with sample values "red" and "5",
    respectively.

- Enum Structures

    A _[Member Type][]_ with an `enum` _[Type Definition][]_ that contains _[Nested Member Types][]_
    specifies an `enum` type structure that MUST only contain items of the specified type and
    values per the particular _[Value Definition][]_.

    ```
    - colors (enum)
        - red (string)
        - 5 (number)
    ```

    Implies a `colors` _[Property Member Type][]_ that MUST only have a value of the string "red" or the number 5.

    A _[Variable Value][]_ in a _[Nested Member Type][]_ under a `enum` type structure indicates an allowed type
    with an associated sample value.

    ```
    - colors (enum)
        - red (string)
        - *5* (number)
    ```

    Implies a `colors` _[Property Member Type][]_ that MUST have either the string "red" or any number as
    a value, where "5" is a sample of a `number` value.

- Named Types

    _[Nested Member Types][]_ MAY be nested directly under a _[Named Declaration][]_ when there are no other
    _[Type Sections][]_ present.

    ```
    # Person (object)
    - `first_name`
    - `last_name`
    ```

With a `fixed` _[Type Attribute][]_:

- If a _[Named Type][]_ or _[Member Type][]_ annotates its type as `fixed`, all _[Nested Member Types][]_ inherit
the `fixed` attribute as well.

    ```
    - person (object, fixed)
        - name
    ```

    Implies:

    ```
    - person (object, fixed)
        - name (fixed)
    ```

- An `array` based _[Named Type][]_ or _[Member Type][]_ MAY specify `fixed` to indicate the structure is a
"fixed ordered list" of only the specified values and/or types, if any, of its _[Nested Member Types][]_.

    ```
    - colors (array, fixed)
        - red
        - green
    ```

    Implies a fixed-list `array` structure that MUST only contain the two items "red" and "green" in that order.

    ```
    - components (array, fixed)
        - (object)
        - (string)
    ```

    Implies a fixed-list `array` structure that MUST only contain the two items of an arbitrary `object` and `string`
    in that order.

- An `object` based _[Named Type][]_ or _[Member Type][]_ MAY specify `fixed` to indicate a "value object" where all
the properties MUST be present and the values of the properties MUST be the values specified, if any, in its
_[Nested Member Types][]_. Further, such an `object` type structure MUST NOT contain any other properties.

    ```
    - person (object, fixed)
        - `first_name`: Andrew
        - `last_name`: Smith
    ```

    Implies a "value object" that MUST contain only the properties "first_name" and "last_name" with the values
    "Andrew" and "Smith", respectively.

    ```
    - person (object, fixed)
        - `first_name`
        - `last_name`
    ```

    Implies an `object` that MUST contain only the properties "first_name" and "last_name", respectively.

- Individual _[Nested Member Types][]_ MAY override inherited behavior from a `fixed` inherited type
by using an `optional` attribute and/or MAY indicate values are samples using a _[Variable Value][]_.

    ```
    - person (object, fixed)
        - `first_name`
        - `last_name` (optional)
    ```

    Implies a "value object" that MUST contain the property "first_name" and MAY contain the property
    "last_name".

    ```
    - colors (array, fixed)
        - red
        - *green*
    ```

    Implies an `array` type structure that MUST contain "red" as an item and MAY contain any other strings, where
    "green" is a sample value.

### 4.4 Sample
Defines alternate sample _[Values][]_ for _[Member Types][]_ as a nested Markdown list with (multi-line) text.

_Sample_ → `- Sample` | `- Sample :` _[Value][]_ | `## Sample`

_Sample_ ⇒ _Markdown-formatted text_ | _[Value Member Types][]_

A _[Type][]_ MAY have multiple _Sample_ lists.

- Named Types

    A header-defined (`##`) _[Sample][]_ MUST be nested one additional header level under the associated
    _[Named Type][]_ if a _[Block Description][]_ is used.

    ```
    # Colors (array)
    A list of colors

    ## Sample
    - red

    ## Items
    - (string)

    ## Sample
    - blue
    - green
    ```

    A `sample` _[Type Attribute][]_ MUST NOT be used in the _[Type Definition][]_ of a _[Named Declaration][]_.

- Member Types

    A list-defined (`-`) _[Sample][]_ SHOULD be nested one indentation level under the associated
    _[Member Type][]_.

    ```
    - colors (array)
      - Sample: red
      - Sample
          - blue
          - green
    ```

    A `sample` _[Type Attribute][]_ MAY be used to indicate a _[Value][]_ in a _[Value Member Type][]_ is a sample
    value.

    ```
    - list: 3, 4 (enum, sample)
    ```

    Is equivalent to:

    ```
    - list: *3, 4* (enum)
    ```

    Which, is equivalent to:

    ```
    - list (enum)
        - Sample
            - 3
            - 4
    ```

    A `default` _[Type Attribute][]_ MUST NOT be used in the _[Type Definition][]_ of a
    _[Property Member Declaration][]_.

### 4.5 Default
Indicates _[Values][]_ for _[Member Types][]_ as a nested Markdown list with (multi-line) text are defaults.

_Default_ → `- Default` | `- Default :` _[Value][]_ | `## Default`

_Default_ ⇒ _Markdown-formatted text_ | _[Value Member Types][]_

A _[Type][]_ MAY have one _Default_ _[Type Section][]_. A _Default_ for a _[Member Type][]_ MAY also indicate a
_[Sample][]_.

- Named Types

    A header-defined (`##`) _[Default][]_ MUST be nested one additional header level under the associated
    _[Named Type][]_ if a _[Block Description][]_ is used.

    ```
    # Colors (array)
    A list of colors

    ## Default
    - red

    ## Items
    - (string)
    ```

     A `default` _[Type Attribute][]_ MUST NOT be used in the _[Type Definition][]_ of a _[Named Declaration][]_.

- Member Types

    A list-defined (`-`) _[default][]_ SHOULD be nested one indentation level under the associated
    _[Member Type][]_.

    ```
    - colors (array)
      - Default: red
    ```

    A `default` _[Type Attribute][]_ MAY be used to indicate a _[Value][]_ in a _[Value Member Type][]_ is a default
    value.

    ```
    - list: 4 (enum, default)
        - 3
        - 4
    ```

    is equivalent to:

    ```
    - list: 3, 4 (enum)
        - Default: 4
    ```

     A `default` _[Type Attribute][]_ MUST NOT be used in the _[Type Definition][]_ of a
     _[Property Member Declaration][]_.

### 4.6 Validations
Reserved for future use.

## 5 Type Inheritance
A _[Member Type][]_ or _[Named Type][]_ that inherits from another _[Named Type][]_ also inherits any
_[Nested Member Types][]_ in the same order they are defined in the inherited _[Named Type][]_ and in order based
on the placement of the _[Mixin Type][]_.

```
# Person (object)
- `first_name`
- `last_name`
```

And:

```
- person (Person)
    - address
```

Implies the same structure as:

```
- person (object)
    - `first_name`
    - `last_name`
    - address
```

Where the inherited _[Member Types][]_ from `Person` _[Named Type][]_ are listed first.

An object may not inherit from itself, either directly or indirectly.

### 5.1 Mixin Type
MSON defines a _Mixin Type_ that supports multiple inheritance from another _[Named Type][]_. The _[Named Type][]_ being inherited MUST be a _[Structure Type][]_ or its sub-type.

_[Nested Member Types][]_ defined in and inherited from the mixed-in _[Named Type][]_ are added at the same indentation level of the _Mixin Type_.

_Mixin Type_ → `- Include` _[Type Name][]_ | `- Include` _[Type Definition][]_

_Example 1_

```
# Person (object)
- `first_name`
- `last_name`
```

And:

```
- `formal_person` (object)
    - prefix: Mr
    - Include Person
```

Implies the same structure as:

```
- `formal_person` (object)
    - prefix: Mr
    - `first_name`
    - `last_name`
```

_Example 2_

Alternately:

```
- `formal_person` (object)
    - Include Person
    - prefix: Mr.
```

Implies the same structure as:

```
- `formal_person` (object)
    - `first_name`
    - `last_name`
    - prefix: Mr.
```

A _[Mixin Type][]_ MUST use an appropriate _[Member Type Separator][]_ in a _[Member Type Group][]_ in order to specify
_[Nested Member Types][]_ after a _[Block Description][]_.

```
- address (object)
    An address

    - Properties
        - Include Address
```

### 5.2 One Of Type
MSON defines a _One Of Type_ that can be used to describe mutually exclusive sets of _[Nested Member Types][]_. A
_One of Type_ MUST only be used to define _[Property Member Types][]_ for a `object` type structure.

_One of Type_ → `- One Of`

_One of Type_ ⇒ _[Member Type Group][]_

_One of Type_ ⇒ _[Nested Member Types][]_

_One of Type_ ⇒ _[Mixin Type][]_

_One of Type_ ⇒ _One of Type_

```
- `first_name`
- One Of
    - `last_name`
    - One Of
        - `given_name`: Smith
        - `suffixed_name`: Smith, Sr.
```

Implies values with a structure of:

```
- `first_name`
- `last_name`
```

Or:

```
- `first_name`
- `given_name`: Smith
```

Or:

```
- `first_name`
- `suffixed_name`: Smith, Sr.
```

A _One Of Type_ MUST use a `Properties` _[Member Type Separator][]_ in a _[Member Type Group][]_:
- In order to specify _[Nested Member Types][]_ after a _[Block Description][]_.

    ```
    - address (object)
        An address

        - Properties
            - One Of
                - state
                - province
    ```

- When it contains a _[Member Type Group][]_.

    ```
    - person (object)
        - One Of
            - `full_name`
            - Properties
                - `first_name`
                - `last_name`
    ```

### 5.3 Generic Named Type
Defines a _[Named Type][]_ that allows an italicized _[Variable Type Name][]_ to represent a _[Type Name][]_
at any location in a _[Type Specification][]_.

_Generic Named Type_ → _[Named Type][]_

By default:
- A _[Named Type][]_ that contains at least one _[Variable Type Name][]_ is a _Generic Named Type_.
- A _Variable Type Name_ in a _[Type Specification][]_ MAY only be used in the _[Type Definition][]_ of explicitly
defined _[Nested Member Types][]_ in the _Generic Named Type_ and MUST NOT define any implied _[Nested Member Types][]_.

- Inherited type as a variable

    ```
    # Address Decorator (*T*)
        - address

    # Person (object)
        - `first_name`
        - `last_name`
    ```

    And:

    ```
    - `decorated_person` (Address Decorator(Person))
    ```

    Implies the same structure as:

    ```
    - decorated_person (object)
        - `first_name`
        - `last_name`
        - address
    ```

- Type passed into explicitly defined _[Member Types][]_

    ```
    # One or Many (*S*[*T*, string])
    - (*T*)
    - (array[*T*])
    ```

    And:

    ```
    - rel (One or Many(enum, object))
    ```

    Implies the same structure as:

    ```
    - rel (enum)
        - (string)
        - (object)
        - array[object]
    ```

### 5.4 Member Type Precedence
Implementers of tooling for MSON structures SHOULD use an inheritance precedence such that the last redundant
_[Member Type][]_ specified in a list at the same indentation level appends or overrides a previously defined
_[Member Type][]_.

```
# Person (object, fixed)
- `first_name`
- `last_name`
- address (object)
```

- Add/Override Attributes

    _Example 1_

    ```
    - person (Person)
        - `last_name` (optional)
    ```

    Is literally the same as:

    ```
    - person (object)
        - `first_name` (fixed)
        - `last_name` (fixed)
        - address (object, fixed)
        - `last_name` (optional)
    ```

    Which implies a structure the same as:

    ```
    - person (object)
        - `first_name` (fixed)
        - `last_name` (optional)
        - address (object, fixed)
    ```

    _Example 2_

    ```
    - person (object)
        - `first_name` (optional)
        - Include Person
    ```

    Is literally the same as:

    ```
    - person (object)
        - `first_name` (optional)
        - `first_name` (fixed)
        - `last_name` (fixed)
        - address (object, fixed)
    ```

    Which implies a structure the same as:

    ```
    - person (object)
        - `first_name` (fixed)
        - `last_name` (fixed)
        - address (object, fixed)
    ```

    _Example 3_

    ```
    - person (object)
        - Include Person
        - `first_name` (optional)

    ```

    Is literally the same as:

    ```
    - person (object)
        - `first_name` (fixed)
        - `last_name` (fixed)
        - address (object, fixed)
        - `first_name` (optional)
    ```

    Implies a structure the same as:

    ```
    - person (object)
        - `first_name` (optional)
        - `last_name` (fixed)
        - address (object, fixed)
    ```

- Add New Member Types

    ```
    - person (Person)
        - citizenship
    ```

    Implies a structure the same as:

    ```
    - person (object, fixed)
        - `first_name`
        - `last_name`
        - address (object)
        - citizenship
    ```

- Override Member Types

    ```
    - person (object)
        - Include Person
        - address (string)
    ```

    Is literally the same as:

    ```
    - person (object)
        - `first_name` (fixed)
        - `last_name` (fixed)
        - address (object, fixed)
        - address (string)
    ```

    Implies a structure the same as:

    ```
    - person (object)
        - `first_name` (fixed)
        - `last_name` (fixed)
        - address (string)
    ```

## 6 Reserved Characters & Keywords
When using following characters or keywords in a _[Property Name][]_, _[Literal Value][]_ or _[Type Name][]_ the name
or literal MUST be escaped in backticks `` ` ``. Otherwise, a `code span` MAY be used for any arbitrary formatting
and has no specific meaning in an MSON document.

### 6.1 Characters

`:`, `(`,`)`, `<`, `>`, `{`, `}`, `[`, `]`, `_`, `*`, `-`, `+`, `` ` ``

### 6.2 Keywords

`Property`, `Properties`, `Item`, `Items`, `Member`, `Members`, `Include`, `One of`, `Sample`

Note keywords are case-insensitive.

### 6.3 Additional Keywords
Following keywords are reserved for future use:

`Trait`, `Traits`, `Parameter`, `Parameters`, `Attribute`, `Attributes`, `Filter`, `Validation`, `Choice`, `Choices`,
`Enumeration`, `Enum`, `Object`, `Array`, `Element`, `Elements`, `Description`

[RFC2119]: https://www.ietf.org/rfc/rfc2119
[Markdown-style link]: http://daringfireball.net/projects/markdown/syntax#link
[How to Read the Grammar]: #1-how-to-read-the-grammar
[Markdown Syntax]: #11-markdown-syntax
[Notational Conventions]: #12-notational-conventions
[Promises]: #13-promises
[Type]: #2-types
[Types]: #2-types
[Base Type]: #21-base-types
[Base Types]: #21-base-types
[Primitive]: #211-primitive-types
[Primitive Type]: #211-primitive-types
[Primitive Types]: #211-primitive-types
[Structure Type]: #212-structure-types
[Structure Types]: #212-structure-types
[Named Type]: #22-named-types
[Named Types]: #22-named-types
[Member Type]: #23-member-types
[Member Types]: #23-member-types
[Property Member Type]: #231-property-member-types
[Property Member Types]: #231-property-member-types
[Value Member Type]: #232-value-member-types
[Value Member Types]: #232-value-member-types

[Type Declaration]: #3-type-declaration
[Named Declaration]: #31-named-declaration
[Generic Named Declaration]: #311-generic-named-declaration
[Property Member Declaration]: #32-property-member-declaration
[Property Name]: #321-property-name
[Variable Property Name]: #322-variable-property-name
[Value Member Declaration]: #33-value-member-declaration
[Value Definition]: #34-value-definition
[Value Definitions]: #34-value-definition
[Value]: #341-value
[Values]: #341-value
[Literal Value]: #342-literal-value
[Variable Value]: #343-variable-value
[Type Definition]: #35-type-definition
[Type Definitions]: #35-type-definition
[Type Specification]: #351-type-specification
[Type Specifications]: #351-type-specification
[Variable Type Specification]: #3511-variable-type-specification
[Type Name]: #352-type-name
[Type Names]: #352-type-name
[Variable Type Name]: #3521-variable-type-name
[Wildcard Type Name]: #3522-wildcard-type-name
[Type Attribute]: #353-type-attribute
[Type Attributes]: #353-type-attribute
[Description]: #36-description
[Descriptions]: #36-description

[Type Section]: #4-type-sections
[Type Sections]: #4-type-sections
[Block Description]: #41-block-description
[Block Descriptions]: #41-block-description
[Member Type Group]: #42-member-type-group
[Member Type Separator]: #421-member-type-separator
[Nested Member Type]: #43-nested-member-types
[Nested Member Types]: #43-nested-member-types
[Sample]: #44-sample
[Samples]: #44-sample
[Default]: #45-default
[Defaults]: #45-default
[Validation]: #46-validations
[Validations]: #46-validations


[Type Inheritance]: #5-type-inheritance
[Mixin Type]: #51-mixin-type
[One Of Type]: #52-one-of-type
[Generic Named Type]: #53-generic-named-type
[Member Type Precedence]: #54-member-type-precedence

[Reserved Characters & Keywords]: #6-reserved-characters--keywords
[Characters]: #61-characters
[Keywords]: #62-keywords
[Additional Keywords]: #63-additional-keywords
