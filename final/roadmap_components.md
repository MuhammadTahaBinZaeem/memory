# Component Roadmap After Locked Resistor Baseline

This roadmap tracks component support after the locked resistor + endpoint-terminal baseline.

## Current locked support

```text
RESISTOR: two-terminal, CDB-backed, V9 linked terminal/resistor/wire group
POWER_TERMINAL: working with two locked methods
```

## Power terminal status

Power terminal is now working and locked with two methods:

```text
1. Short-wire endpoint method
   final/power_terminal_short_wire_method.md

2. Donor-derived output-bridge method
   final/power_terminal_output_bridge_method.md
```

The failed hand-built bridge method is recorded and must not be used:

```text
experiments/power_terminal_output_bridge_2026-05-29/test_result_vgdvc_failure.md
```

## Remaining planned order

```text
1. ground terminal
2. capacitor
3. inductor
4. DC power source
5. AC power source
6. v1 actual generator release
```

## Completed Milestone: power terminal

Confirmed working methods:

```text
$TERPOWER(node) -> short wire -> resistor pin

donor-derived bridge cluster containing $TERPOWER and $TEROUTPUT(node), then same-name node connection to circuit
```

Requirements satisfied:

```text
identified marker: $TERPOWER
confirmed ROOT.DSN visual authority
confirmed no CDB record required in current observed method
confirmed connection behavior with resistor network by user testing
recorded artifacts and manifests
locked final method docs
```

Power terminal JSON extension target:

```json
{
  "ref": "P1",
  "type": "POWER_TERMINAL",
  "node": "N1",
  "label": "N1",
  "connection_style": "donor_output_bridge"
}
```

Alternative locked style:

```json
{
  "ref": "P1",
  "type": "POWER_TERMINAL",
  "node": "V0",
  "label": "V0",
  "connection_style": "short_wire_endpoint"
}
```

## Next Milestone: ground terminal

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

## Milestone: capacitor

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

## Milestone: inductor

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

## Milestone: DC power source

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

## Milestone: AC power source

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
