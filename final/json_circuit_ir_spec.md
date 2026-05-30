# Final JSON Circuit IR Specification

This file defines the JSON format that the Proteus generator must accept.

The goal is that any AI or human can describe a small circuit in this JSON format, and the generator can create a Proteus `.pdsprj` project using the locked V9 method.

## Top-level object

The JSON must be a single object with these fields:

```json
{
  "schema_version": "proteus-circuit-ir/v0.1",
  "generator_target": "proteus-8.13-v9-resistor-terminal",
  "project": {},
  "nodes": [],
  "components": [],
  "layout": {},
  "metadata": {}
}
```

## Required fields

```text
schema_version      required string
generator_target    required string
project             required object
nodes               required array
components          required array
layout              required object
metadata            optional object
```

## schema_version

For the locked baseline, use:

```json
"schema_version": "proteus-circuit-ir/v0.1"
```

## generator_target

For the current locked method, use:

```json
"generator_target": "proteus-8.13-v9-resistor-terminal"
```

This tells the generator to use:

```text
E001 empty base
V9 linked terminal/resistor/wire groups
resistor-only component support
two-character node labels
```

## project object

Required fields:

```json
"project": {
  "name": "HANDDRAWN_6R_CORRECTED_N0_N1_N2_N3_N4",
  "output_basename": "HANDDRAWN_6R_CORRECTED_N0_N1_N2_N3_N4",
  "base": "E001_EMPTY_BASE",
  "units": "proteus_internal"
}
```

Rules:

```text
name: human-readable project name
output_basename: used for output file names
base: must be E001_EMPTY_BASE for this baseline
units: must be proteus_internal unless a later coordinate converter is added
```

## nodes array

Each node must have a two-character label.

Example:

```json
"nodes": [
  {"id": "V0", "kind": "power"},
  {"id": "G0", "kind": "ground"},
  {"id": "N0", "role": "left_common"},
  {"id": "N1", "role": "top_right"},
  {"id": "N2", "role": "middle_right"},
  {"id": "N3", "role": "lower_middle_right"},
  {"id": "N4", "role": "bottom_right"}
]
```

Required fields per node:

```text
id      required string, exactly two ASCII characters for this baseline
role    optional string
kind    optional string: internal, power, ground
```

Node label rules:

```text
Must be exactly two ASCII characters.
Recommended pattern: capital letter + digit, such as N0, N1, A1, B2.
Use V0 for current power endpoint tests and G0 for current ground endpoint tests.
Avoid GND, VCC, OUT, IN1 until variable-length labels are validated.
Do not define duplicate node ids.
Every component endpoint must reference an existing node id.
```

## current power/ground endpoint support

The main generator currently supports the locked short-wire endpoint methods:

```text
V0 with kind=power  -> $TERPOWER when used as component.nodes[0]
G0 with kind=ground -> $TERGROUND when used as component.nodes[1]
```

Constraints:

```text
$TERPOWER is only locked for left endpoints because it replaces $TERINPUT.
$TERGROUND is only locked for right endpoints because it replaces $TEROUTPUT.
Standalone POWER_TERMINAL_BRIDGE and GROUND_TERMINAL_BRIDGE JSON components are not part of this v0.1 input yet.
```

## components array

Each component describes one schematic device.

Current locked component type:

```text
RESISTOR
```

Resistor object format:

```json
{
  "ref": "R1",
  "type": "RESISTOR",
  "value": "1k",
  "nodes": ["N0", "N1"],
  "visual": {
    "orientation_hint": "horizontal",
    "role": "top_horizontal"
  }
}
```

Required fields per resistor:

```text
ref       required string, exactly two characters for this baseline
          examples: R1, R2, R9, RA, RB

type      required string, must be RESISTOR

value     required string, resistor value such as 1k, 220R, 10k
          for the current stable visible field, compact values are safest

nodes     required array of exactly two existing node ids
          nodes[0] is left/input terminal for the V9 group
          nodes[1] is right/output terminal for the V9 group

visual    optional object
```

Reference rules:

```text
For 1 to 9 resistors, use R1..R9.
For 10 and above in the current fixed-width baseline, use RA, RB, RC, etc.
Do not use R10 until longer reference labels are validated in the visual object family.
Each ref must be unique.
```

Value rules:

```text
CDB should store the real value.
Visible value may need compact representation if visual value field is fixed-width.
For resistor-only v0.1, values like 1k..9k are safest.
If values above 9k are needed, the generator may use compact visible labels while storing real CDB values.
```

## layout object

The layout object gives generator placement hints. It is not the electrical authority. The component `nodes` list is the electrical/topological authority.

Baseline layout format:

```json
"layout": {
  "mode": "manual_component_positions",
  "coordinate_units": "proteus_internal",
  "component_positions": {
    "R1": {"x": -6350000, "y": 5080000},
    "R2": {"x": -2540000, "y": 4318000},
    "R3": {"x": -6350000, "y": 3556000},
    "R4": {"x": -2540000, "y": 2032000},
    "R5": {"x": -2540000, "y": 508000},
    "R6": {"x": -6350000, "y": -1016000}
  }
}
```

Allowed layout modes for v0.1:

```text
manual_component_positions
branch_grid
auto_grid
```

For the first actual generator, `manual_component_positions` is safest.

Rules:

```text
Every component should have a position.
If a component is missing a position, set `layout.auto_place` to true so the generator may auto-place it and record that in manifest.json.
Coordinates are for body center of the resistor visual object.
Terminal and wire positions are generated relative to the resistor position using the locked V9 offsets.
```

## metadata object

The metadata object records source context.

Example:

```json
"metadata": {
  "source": "hand-drawn image",
  "source_image_sha256": "8f8f1327b8efc71473b541b1f0198e4871e47b30e75059260ad5e8781ff0a0a4",
  "human_description": "corrected 6-resistor two-loop circuit",
  "notes": [
    "Vertical resistors in source are represented topologically by node labels; physical rotation not yet locked."
  ]
}
```

## Electrical authority rule

The component node list is the source of truth.

Example:

```json
{"ref": "R2", "type": "RESISTOR", "value": "2k", "nodes": ["N1", "N2"]}
```

means:

```text
R2 connects node N1 to node N2.
```

It does not matter whether the original drawing shows that resistor vertical, horizontal, angled, or curved. Until rotation support is locked, visual orientation is only a hint.

## Validation rules

A valid v0.1 JSON must satisfy:

```text
schema_version is proteus-circuit-ir/v0.1
generator_target is proteus-8.13-v9-resistor-terminal
project.base is E001_EMPTY_BASE
nodes is nonempty
all node ids are unique
all node ids are exactly two ASCII characters
components is nonempty
all component refs are unique
all component refs are exactly two ASCII characters
all component types are RESISTOR
all resistor nodes arrays have exactly two entries
all resistor node references exist in nodes array
layout exists
component positions are provided or layout.auto_place is explicitly true
power nodes, when used as generated power terminals, are on component.nodes[0]
ground nodes, when used as generated ground terminals, are on component.nodes[1]
```

## Invalid examples

Invalid because node labels are too long for current baseline:

```json
{"id": "GND"}
```

Invalid because ref is too long for current baseline:

```json
{"ref": "R10", "type": "RESISTOR", "value": "10k", "nodes": ["N0", "N1"]}
```

Use this instead for the current fixed-width baseline:

```json
{"ref": "RA", "type": "RESISTOR", "value": "10k", "nodes": ["N0", "N1"]}
```

Invalid because endpoint node is not declared:

```json
{"ref": "R1", "type": "RESISTOR", "value": "1k", "nodes": ["N0", "NX"]}
```

## Generator output requirements

Given a valid JSON file, the generator must produce:

```text
<output_basename>.pdsprj
<output_basename>.ROOT.CDB.bin
<output_basename>.ROOT.DSN.bin
manifest.json
generation_code_used.py or generator version reference
README_TEST_FIRST.txt
```

The manifest must include:

```text
input_json_sha256
output_project_sha256
base project identity
component count requested
component count emitted in CDB
component count emitted in DSN
terminal count
wire count
object group count
terminator validation
link suffix validation
section pointer values
static_validation_issues
known limitations
```
