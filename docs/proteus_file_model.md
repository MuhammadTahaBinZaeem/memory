# Proteus File Model

## Resistor V9 Generator Scope

The current main generator implementation in `D:/Coding/protuesgen` follows the locked V9 resistor-terminal method:

- Use blank E001 as the `.pdsprj` base.
- Copy E001 `PROJECT.XML`.
- Copy E001 `SCRIPTS/PWRRAILS.DAT`.
- Generate `ROOT.CDB`.
- Generate `ROOT.DSN`.
- Use the user-confirmed R21 V9 artifact only as a byte-record donor.

## V9 Object Stream Rule

Each resistor has a conceptual linked endpoint group:

```text
left endpoint terminal
right endpoint terminal
resistor visual record
left short wire
right short wire
```

The generated records must preserve:

- terminal/resistor link suffix consistency
- one generated CDB record per resistor
- no premature final terminators
- final terminator only at the last object
- section pointers patched after final object-stream length is known

## Resistor Orientation Field

Public Proteus sample projects show that the resistor visual record stores rotation in the four bytes immediately after the final model placement `x/y` pair:

```text
00 00 00 00 = horizontal
7c fc 00 00 = -900 tenths of a degree, vertical down
```

The current main generator patches this field for locked 90-degree orientations. For `visual.orientation_hint = "vertical"`, the first resistor pin remains at the component position and the second pin is generated at `y - 1270000`; the terminal and short-wire endpoint records are generated along the same vertical axis.

## Standalone Visual Wires

The current generator also supports optional `layout.visual_wires` entries. These create standalone visible `WIRE` records after the resistor endpoint groups while preserving one final object-stream terminator.

Use these records for drawn buses and comparison links such as parallel rails, bridge cross-links, delta closure, and R-2R ladder bus segments. They are drawing geometry only; the component `nodes` arrays remain the electrical/topological authority.

## Power And Ground Endpoint Rule

Current supported labels:

```text
V0 = power
G0 = ground
```

Current safe terminal substitutions:

```text
$TERINPUT  -> $TERPOWER   when power is component.nodes[0]
$TEROUTPUT -> $TERGROUND  when ground is component.nodes[1]
```

Long labels such as `VCC` and `GND` remain outside the locked v0.1 resistor generator until variable-length terminal labels are validated.
