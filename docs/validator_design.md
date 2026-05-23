# Validator Design

The validator consumes CircuitIR JSON and returns a validation report matching `schemas/validation_report.schema.json`.

## Validator goals

The validator must reject bad or unsupported circuit descriptions before generation.

## Required checks for v0

### Schema checks

- input matches `schemas/circuit_ir.schema.json`
- required fields exist
- no unexpected fields unless explicitly allowed

### Component checks

- every component has unique `ref`
- every component `part` exists in `knowledge/component_db.json`
- component status is generation-ready or explicitly experimental
- resistor components require a `value`

### Net checks

- every net name is unique
- every connection references a declared net
- net names use characters accepted by current terminal-label rules
- `VCC` should be kind `power`
- `GND` should be kind `ground`

### Connection checks

- every connection references an existing component ref
- every pin exists for supported components
- for v0 resistor support, allowed pins are `1` and `2`
- each resistor should have exactly two terminal connections unless intentionally marked partial

### Scope checks

For resistor-generator v0, reject unsupported parts even if present in requested future scope.

Supported v0 parts:

```text
RESISTOR
INPUT_TERMINAL
OUTPUT_TERMINAL
POWER_TERMINAL
GROUND_TERMINAL
```

The generator should usually create terminals automatically from nets, so CircuitIR does not need explicit terminal components for every net.

## Warning examples

- internal net has only one connection
- power net is declared but unused
- output net is not connected to anything
- component value uses an untested format

## Error examples

```json
{
  "code": "DUPLICATE_COMPONENT_REF",
  "message": "Component ref R1 appears more than once.",
  "path": "circuit.components"
}
```

```json
{
  "code": "UNSUPPORTED_COMPONENT",
  "message": "Part 74LS00 is known but not generation-ready yet.",
  "path": "circuit.components[0].part",
  "suggestion": "Run controlled component tests before enabling this part."
}
```

```json
{
  "code": "INVALID_PIN",
  "message": "RESISTOR only supports pins 1 and 2 in v0.",
  "path": "circuit.connections[3].pin"
}
```
