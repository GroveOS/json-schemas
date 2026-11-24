# JSON Schemas

Small, reusable, implementation-agnostic JSON Schema definitions for exchanging information across systems.

## Principles

- Describe information, not databases, interfaces, workflows, or application classes.
- Prefer plain, human-readable text unless numeric, boolean, or temporal semantics are *essential*.
- Preserve source information before normalizing or interpreting it.
- Keep schemas focused and context-independent; compose larger models with `$ref`.
- Require only what is necessary for an instance to be meaningful.
- Use open-ended descriptive strings when a fixed vocabulary is not intrinsic to the concept.
- State meaningful constraints, units, and formats clearly without imposing unnecessary rules.

## Conventions

- JSON Schema Draft-07
- A descriptive `title` and `description` on every standalone schema
- Arrays for collections and objects for related properties
- Explicit `additionalProperties` policy
- Concise, domain-independent property names where practical

## Schemas

### `contacts.json`

A collection of contacts associated with any primary entity. Each contact has a name, a relationship to that entity, optional priority, contact methods, and notes. It can be composed into people, organizations, projects, buildings, systems, and other schemas.

### `schema-builder.json`

A compact exchange format for requesting schema-design help from a person or AI system.

Every message has only `type` and `value`:

```json
{
  "type": "draft",
  "value": "We need a schema for lesson inquiries. So far we collect date, student, age, contacts, and interests."
}
```

- A `draft` response returns concise prose questions and recommendations in `value`.
- A `final` request supplies prose in `value`.
- A `final` response returns the completed Draft-07 JSON Schema object in `value`.

Good schemas make useful information portable while making as few unnecessary assumptions as possible.
