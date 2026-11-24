# JSON Schemas

A collection of reusable, implementation-agnostic JSON schemas designed for composability and interoperability across systems, languages, and platforms.

## Philosophy

Inspired by the Unix philosophy, this repository treats information as something that should be easy to read, exchange, understand, and compose, independently of the systems that produce or consume it.

Schemas should describe the intrinsic structure of information without unnecessarily prescribing how that information is stored, interpreted, presented, or used.

The goal is not to model the entirety of reality, but to provide useful, portable representations of it. Structure should serve information, rather than information being forced to serve structure.

## Design principles

### 1. Prefer plain text

Prefer human-readable strings over more restrictive primitive types wherever practical. Plain text is portable, inspectable, editable, and accessible without specialized tooling.

Numbers, booleans, timestamps, and identifiers are not inherently undesirable, but their representation should be chosen deliberately. A value that merely looks numeric may not actually represent a quantity. A telephone number, postal code, or account identifier, for example, is better represented as a string.

Use native numbers or booleans when their arithmetic or logical semantics are essential to the meaning of the data. When a value is represented as text, avoid ambiguity where it would undermine interoperability; dates, for example, can use a documented ISO 8601 representation.

### 2. Structure only what needs structure

Do not decompose information simply because it can be decomposed.

Prefer:

```json
{
  "name": "María García López"
}
```

Over:

```json
{
  "first-name": "María",
  "middle-name": "",
  "last-name": "García López"
}
```

Personal naming conventions vary across cultures, languages, families, and individuals. Chinese names, Spanish-language surnames, mononyms, and many other naming traditions do not necessarily fit a universal first-name/last-name model.

A single `name` preserves information without imposing an unnecessary interpretation of its internal structure. More specialized systems can introduce additional structure when their particular needs justify it.

### 3. Remain independent of context

A schema should describe what its information is, not every context in which it might exist.

A collection of contacts may belong to a person, organization, project, building, process, or system. The contacts schema does not need to distinguish between these possibilities. Its surrounding context establishes the primary entity.

Avoid embedding domain-specific assumptions into otherwise general-purpose definitions.

### 4. Design for composition

Prefer small, focused schemas that describe a coherent concept and can be incorporated into larger structures.

A contacts schema can be used independently or composed into schemas for people, organizations, projects, and other entities through JSON Schema references (`$ref`). A collection should be an array when an array is sufficient; it does not need an additional object wrapper merely to give it a name.

Each schema should do one thing well. More complex structures should emerge through composition rather than duplication.

### 5. Require as little as necessary

Required properties should represent the minimum information necessary for an instance to meaningfully express the concept being described.

Optional information should remain optional. Do not require consumers to invent values, supply empty strings, or populate irrelevant fields simply to satisfy validation.

Distinguish between omitted information, explicitly unknown information, and an empty collection where that distinction matters. Use `null` deliberately, and document its meaning when supported.

### 6. Prefer meaningful strings over rigid classifications

Use descriptive strings when the range of possible values is open-ended or dependent on context.

For example:

```json
{
  "name": "Alex Rivera",
  "relation": "technical coordinator"
}
```

A contact's relationship to a primary entity need not be restricted to a predetermined list of roles.

String-valued enumerations are appropriate when a finite vocabulary is genuinely part of the concept being represented. Prefer readable values over numeric codes or opaque identifiers. Avoid enumerations that attempt to exhaustively classify an inherently open-ended domain.

### 7. Make meaning explicit

Use concise, descriptive property names and plain-language descriptions. Prefer names that communicate meaning without depending on an application, programming language, or implementation convention.

Document constraints that matter to interoperability. Do not rely on a consumer's implicit understanding of units, formats, or domain-specific terminology.

Human readability and machine validation should complement one another.

### 8. Preserve information before interpreting it

Do not discard meaningful distinctions merely to make data fit a convenient model. Preserve spelling, punctuation, Unicode characters, and other significant aspects of source information.

Normalization, categorization, formatting, and other transformations may be useful, but should not be imposed by a general-purpose schema unless they are intrinsic to the concept itself.

### 9. Avoid unnecessary implementation assumptions

Schemas should not prescribe database layouts, application classes, storage engines, frameworks, user interfaces, or business workflows.

They define a shared representation of information. Consumers remain free to interpret and transform that representation according to their own needs.

### 10. Favor simplicity over completeness

A schema need not anticipate every possible use case. It should provide a useful common foundation without becoming burdened by speculative properties or constraints.

Generalization is not the absence of structure. It is the removal of unnecessary assumptions while retaining meaningful structure.

## Conventions

- Use JSON Schema Draft-07 for broad tooling compatibility.
- Give each standalone schema a descriptive `title` and `description`.
- Prefer plain-text strings, including string-valued enumerations where appropriate.
- Use arrays for collections and objects for related properties.
- Keep property names concise and domain-independent wherever practical.
- Require only the properties essential to the concept.
- Explicitly declare whether additional properties are accepted; closed objects can provide predictable contracts, while extensions should be defined deliberately.
- Prefer composition through `$ref` over duplicating definitions.

## Example

The following schema describes contacts associated with a primary entity without defining what that entity is:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "contacts",
  "type": "array",
  "description": "A collection of contacts associated with a primary entity. Use an empty array if no contacts are known.",
  "items": {
    "type": "object",
    "properties": {
      "name": {
        "type": "string",
        "description": "Name of the contact."
      },
      "relation": {
        "type": "string",
        "description": "The nature of the contact's relationship to the primary entity. Use 'unknown' if unspecified."
      },
      "phone": {
        "type": ["string", "null"],
        "description": "Contact's phone number."
      },
      "email": {
        "type": ["string", "null"],
        "description": "Contact's email address."
      }
    },
    "additionalProperties": false,
    "required": ["name", "relation"]
  }
}
```

The same definition can be incorporated into systems that have otherwise little in common. The meaning of the primary entity is supplied by the context in which the schema is used.

## Guiding idea

Define information in a way that allows it to exist independently of any particular system.

A good schema is not necessarily the one that describes the most about a thing, but the one that makes the fewest unnecessary assumptions about it.
