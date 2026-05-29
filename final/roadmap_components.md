# Component Roadmap After Locked Resistor Baseline

The locked baseline is resistor-only.

Do not merge new component support into the main generator until each item has its own controlled experiment, notes, JSON extension, generated artifact, validation manifest, and user-tested result.

## Current locked baseline

```text
RESISTOR: two-terminal, CDB-backed, V9 linked terminal/resistor/wire group
```

## Planned order

```text
1. power terminal
2. ground terminal
3. capacitor
4. inductor
5. DC power source
6. AC power source
7. v1 actual generator release
```

## Milestone 1: power terminal

Required work:

```text
identify stable Proteus power terminal object family
confirm label/value constraints
confirm CDB interaction if any
confirm connection behavior with resistor network
update JSON spec with terminal component/node role
create simple test circuit
record artifact and manifest
```

Possible JSON extension later:

```json
{
  "ref": "P1",
  "type": "POWER_TERMINAL",
  "node": "V0",
  "label": "VCC"
}
```

Not locked yet.

## Milestone 2: ground terminal

Required work:

```text
identify ground terminal object family
confirm whether GND is visual label, net label, or special object type
confirm netlisting/simulation behavior
update JSON spec
create resistor-to-ground test
record artifact and manifest
```

Possible JSON extension later:

```json
{
  "ref": "G1",
  "type": "GROUND_TERMINAL",
  "node": "G0",
  "label": "GND"
}
```

Not locked yet.

## Milestone 3: capacitor

Required work:

```text
identify capacitor CDB component records
identify capacitor DSN visual object records
confirm value field handling
confirm linked terminal/wire grouping
test capacitor alone and resistor-capacitor network
update JSON spec
```

Possible JSON extension later:

```json
{
  "ref": "C1",
  "type": "CAPACITOR",
  "value": "1u",
  "nodes": ["N1", "N2"]
}
```

Not locked yet.

## Milestone 4: inductor

Required work:

```text
identify inductor CDB records
identify inductor DSN visual records
confirm value field handling
confirm linked grouping
test resistor-inductor network
update JSON spec
```

Possible JSON extension later:

```json
{
  "ref": "L1",
  "type": "INDUCTOR",
  "value": "10m",
  "nodes": ["N1", "N2"]
}
```

Not locked yet.

## Milestone 5: DC power source

Required work:

```text
identify DC source primitive/model records
identify visual object record
confirm polarity handling
confirm value handling
confirm CDB simulation model behavior
create resistor + DC source + ground test
update JSON spec
```

Possible JSON extension later:

```json
{
  "ref": "V1",
  "type": "DC_VOLTAGE_SOURCE",
  "value": "5V",
  "nodes": ["N1", "G0"],
  "polarity": {"positive": "N1", "negative": "G0"}
}
```

Not locked yet.

## Milestone 6: AC power source

Required work:

```text
identify AC source primitive/model records
identify visual object record
confirm amplitude/frequency/phase fields
confirm CDB simulation model behavior
create AC source + resistor test
update JSON spec
```

Possible JSON extension later:

```json
{
  "ref": "V1",
  "type": "AC_VOLTAGE_SOURCE",
  "amplitude": "1V",
  "frequency": "1k",
  "phase": "0",
  "nodes": ["N1", "G0"],
  "polarity": {"positive": "N1", "negative": "G0"}
}
```

Not locked yet.

## v1 actual generator release criteria

v1 can be called only after the following are locked:

```text
JSON parser and validator
resistor generation
power terminal
ground terminal
capacitor
inductor
DC source
AC source
manifest generation
artifact recording workflow
static validation suite
at least one tested example for every supported component type
```
