# YAML Courseware

## Introduction

YAML stands for "YAML Ain't Markup Language" (a recursive acronym) or sometimes "Yet Another Markup Language". It's a human-readable data serialization format, often used for configuration files and data interchange between languages with different data structures. YAML is to JSON what markdown is to HTML: a simpler way to write structured data that gets converted into a more complex format.

Here are the main characteristics and features of the YAML format:

### Human-readable

One of the goals of YAML is to be easily readable by humans, which means it's often favored for configuration files where manual edits might be necessary.

### Indentation

The structure of a YAML document is determined by indentation using spaces (not tabs!). This is similar to how Python determines block structure.

## Key-Value Pairs

Much of the structure of a YAML document is made up of key-value pairs, similar to JSON.

```yaml
key: value
```

## Lists/Arrays

Lists are created using hyphens:

```yaml
fruits:
  - Apple
  - Banana
  - Cherry
```

## Maps/Dictionaries

Use key-value pairs within indents:

```yaml
person:
  name: John
  age: 30
```

## Comments

Anything after a `# ` is considered a comment.

```yaml
# This is a comment
key: value # This is also a comment
```

## Multi-line Strings

YAML provides ways to denote multi-line strings, either with the | (keep newlines) or > (fold newlines) characters.

### Be Cautious with Multi-Line Strings

Since YAML provides multiple ways to handle multi-line strings it is important to understand their differences:

`|` keeps newlines.
`|-` trims the trailing newline.
`>` folds newlines to spaces.
`>-` folds newlines to spaces and trims the trailing newline.

```yaml
description: |
  This is a long description
  that spans multiple lines.
  But it has a trailing newline.
```

> *Result*
>
> ```text
> This is a long description
> that spans multiple lines.
> But it has a trailing newline.
>
> ```

```yaml
description: |-
  This is a long description
  that spans multiple lines.
  With no trailing newline.
```

> *Result*
>
> ```text
> This is a long description
> that spans multiple lines.
> With no trailing newline.
> ```

```yaml
description: >
  While this is a long description
  that will fold into one line.

  But this line will be on its own.
  With a trailing newline.
```

> *Result*
>
> ```Text
> While this is a long description that will fold into one line.
> But this line will be on its own. With a trailing newline.
>
> ```

```yaml
description: >-
  While this is a long description
  that will fold into one line.

  But this line will be on its own.
  With no trailing newline.
```

> *Result*
>
> ```text
> While this is a long description that will fold into one line.
> But this line will be on its own. With no trailing newline.
> ```

## Scalar Data Types

YAML supports various scalar data types, including strings, integers, floats, booleans, and nulls.

```yaml
# Boolean:
boolExample1: true    # Also True, TRUE
boolExample2: false   # Also False, FALSE
boolExample3: yes     # Also Yes, YES, y, Y
boolExample4: no      # Also No, NO, n, N
boolExample5: on      # Also On, ON
boolExample6: off     # Also Off, OFF

# Date:
dateExample: 2015-04-05

# Float
floatExample1: 123.0
floatExample2: 1.23e+2

# Integer
intExample1: 123
intExample2: !!int 123

# Strings
strExample1: A string value supporting don't
strExample2: "A string value supporting don't"
strExample3: 'A string value supporting don''t'
strExample4: !!str A string value supporting don't
```

## Support for Anchors & Aliases

This allows for creating references in the YAML document, so you don't have to duplicate data. For example:

```yaml
default: &DEFAULT
  name: Default Name
  age: 20

person1:
  <<: *DEFAULT
  name: Alice

person2:
  <<: *DEFAULT
```

In the above example, both person1 and person2 will have age: 20 from the DEFAULT anchor, but person1 has a custom name.

## No explicit ending delimiter

Unlike some formats, you don't need a special character or set of characters to indicate the end of a YAML document.

## Support for multiple documents

You can have multiple YAML documents in a single file, separated by ---.

It is also best practice to start a YAML document using the document separator, like this:

```yaml
---
and:
  here:
    we:
      - "go"
      - "go"
      - "go"
```

## Strict Syntax

YAML can be error-prone due to its reliance on indentation. A misplaced space can change the meaning of a YAML file, so tools like YAML linters are often used to catch mistakes.

Here's a brief example of a YAML file:

```yaml
name: John Doe
age: 30
is_student: false
addresses:
  - city: New York
    zip: "10001"
  - city: Los Angeles
    zip: "90001"
```

In the computing world, YAML is often used in configuration files, Kubernetes definitions, Ansible playbooks, and more. It's essential to handle YAML with care due to its whitespace sensitivity, but its readability makes it a popular choice for many applications.
