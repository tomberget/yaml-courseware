# YAML Courseware

## Introduction

YAML stands for "YAML Ain't Markup Language" (a recursive acronym) or sometimes "Yet Another Markup Language". It's a human-readable data serialization format, often used for configuration files and data interchange between languages with different data structures. YAML is to JSON what markdown is to HTML: a simpler way to write structured data that gets converted into a more complex format.

Here are the main characteristics and features of the YAML format:

### Human-readable

One of the goals of YAML is to be easily readable by humans, which means it's often favored for configuration files where manual edits might be necessary.

### Indentation

The structure of a YAML document is determined by indentation using spaces (not tabs!). This is similar to how Python determines block structure.

## YAML structure

### Key-Value Pairs

Much of the structure of a YAML document is made up of key-value pairs, similar to JSON.

```yaml
key: value
```

### Lists/Arrays

Lists are created using hyphens:

```yaml
fruits:
  - "Apple"
  - "Banana"
  - "Cherry"
```

### Maps/Dictionaries

Use key-value pairs within indents:

```yaml
person:
  name: "John"
  age: 30
```

### Comments

Anything after a `# ` is considered a comment.

```yaml
# This is a comment
key: value  # This is also a comment
```

Please note that *yamllint* insist that you should use a double space between the end of a value, and it's comment, when the comment is placed at the end of a line.

### Multi-line Strings

YAML provides ways to denote multi-line strings, either with the | (keep newlines) or > (fold newlines) characters. It is important that once a multi-line has been denoted, you are not to wrap the string using `"`-s or `'`-s. Everything that belongs to the `|` or `>` are evaluated as text.

#### Be Cautious with Multi-Line Strings

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

### Scalar Data Types

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

### Support for Anchors & Aliases

This allows for creating references in the YAML document, so you don't have to duplicate data. For example:

```yaml
homeAddress: &homeAddr
  street: "Henrik Ibsens gate 1"
  zipcode: "0255"
  city: "Oslo"
  country: "Norway"
invoiceAddress:
  <<: *homeAddr
  street: "Karl Johans gate 22"
  zipcode: "0026"
deliveryAddress: *homeAddr
```

In the above example, both `invoiceAddress` and `deliveryAddress` reuse the `homeAddress` structure and values.

For the `invoiceAddress`, we use *merge reference* to the anchor `&homeAddr` in order to set new `street` and `zipcode` values. `city` and `country`, which are not overwritten, will still reference the anchor values.

#### Verifying that anchors and references work

It is not possible to see directly in the YAML file that the anchors and references work like intended. To easily validate, you can make use of YAML Query (yq), which is available [here](https://mikefarah.gitbook.io/yq/).

Once installed, you can convert the file from YAML to JSON, and then back again to YAML - in order to get the human readable output. The difference can be seen by yourself like this:

> **The raw YAML output**
>
> This is no different than viewing the file in a text editor.
>
> ```sh
> yq -e '.' Examples/Pizza/pizzas-address.yaml
> ```

When converting it to JSON and back:

> **Processed YAML output**
>
> ```sh
> yq -o=json Examples/Pizza/pizzas-address.yaml | yq -P -
> ```

### No explicit ending delimiter

Unlike some formats, you don't need a special character or set of characters to indicate the end of a YAML document.

### Support for multiple documents

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

### Strict Syntax

YAML can be error-prone due to its reliance on indentation. A misplaced space can change the meaning of a YAML file, so tools like YAML linters are often used to catch mistakes.

Here's a brief example of a YAML file:

```yaml
name: "Tom Berget"
age: 30
is_student: false
addresses:
  - city: Oslo
    zip: "0123"
  - city: Oslo
    zip: "0124"
```

In the computing world, YAML is often used in configuration files, Kubernetes definitions, Ansible playbooks, and more. It's essential to handle YAML with care due to its whitespace sensitivity, but its readability makes it a popular choice for many applications.

## Supporting systems

### Visual Studio Code

[VS Code](https://code.visualstudio.com) is not the be all/end all of working with YAML files. However, it has many benefits when regarding extensions that can help you write YAML more correctly and more accurately.

Though the other supporting systems mentioned may have extension/plugin support for other IDEs (like Notepad++), it is not highlighted here.

### EditorConfig

[EditorConfig](https://editorconfig.org/) helps to maintain consistent coding styles cross developers. Github has native support for it, and it can be added as an extension for *VS Code*.

The EditorConfig relies on a `.editorconfig` file, that tells the IDE which settings should be used for files with a certain extension. Some settings can be set for all files `[*]`, and some can be changed for certain file extensions. In this repository, you will find a [.editorconfig](/.editorconfig) file that defines coding styles for all files, as well as for `.yml`, `.yaml`, `.md` and `.sh` file extensions specifically.

A link to the *VS Code* extension: [EditorConfig for VS Code](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig).

### YAML

The [YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml) linter from Red Hat is a great default tool to hold in your arsenal of extensions. It also has build-in Kubernetes syntax support.

### yamllint

[yamllint](https://github.com/adrienverge/yamllint) is a linter based on *Python* that lints your YAML files. It does not only check for syntax validity, but also:

> *for weirdnesses like key repetition and cosmetic problems such as lines length, trailing spaces, indentation, etc*

A comprehensive documentation regarding *yamllint* can be found at [yamllint](https://yamllint.readthedocs.io/en/stable/).

The *VS Code* extension for yamllint is an extension that support linting for multiple languages in one package: [Linter](https://marketplace.visualstudio.com/items?itemName=fnando.linter).

There is a default ruleset that *yamllint* will use in order to validate YAML files. By adding a `.yamllint` file to the root of your repository, you can globally change how, and for what, the linter should report errors or warnings. A more relaxed example file can be found at the root in this repo at [.yamllint](/.yamllint).

### yq

[yq](https://mikefarah.gitbook.io/yq/) is a lightweight and portable command-line YAML processor. Using this tool, you can easily query YAML files for content, or convert YAML -> JSON -> YAML in order to see what values are actually used. When dealing with anchors and references, this is an easy way to validate the YAML values.

#### Convert YAML -> JSON -> YAML

Converting from YAML to JSON to YAML is a way to check that the YAML structure, keys and values, are as you expect. In this example, we convert the [pizzas-address.yaml](/Examples/Pizza/pizzas-address.yaml) file to check that Anchor/Reference work as expected.

```sh
yq -o=json Examples/Pizza/pizzas-address.yaml | yq -P -
```

#### Querying a YAML file

It is possible to query a YAML file, to check that the path is correct, and that the data at that path is correct. It is important to know that a query string *always* start with a single `.`, to reference the root of the YAML document.

As an example, let us retrieve the `city` value from the `invoiceAddress` node from the same [pizzas-address.yaml](/Examples/Pizza/pizzas-address.yaml) as used above:

```sh
yq -e '.invoiceAddress.city' Examples/Pizza/pizzas-address.yaml
```

### The YAML specification

The current YAML specification can be found at [yaml.org](https://yaml.org/spec/1.2.2/). This can be helpful if you are looking into deep diving into YAML, or just need to understand it (or sections of it) better.
