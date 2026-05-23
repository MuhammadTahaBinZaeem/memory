# Generator Design

The generator is deterministic Python code. It consumes validated CircuitIR and emits a Proteus `.pdsprj` file.

## Current target

Version 0 target:

- Proteus 8.13
- terminal-based resistor networks
- VCC/GND/INTERNAL/OUTPUT nets represented using terminals
- resistor values and refs stored consistently

## High-level flow

```text
CircuitIR
  -> validate
  -> load Proteus 8.13 template
  -> build visual schematic data from templates
  -> build component metadata data
  -> copy stable internal project files
  -> repack .pdsprj
```

## Authority model used by generator

For Proteus 8.13, based on current tests:

- visual object existence: ROOT.DSN
- terminals: ROOT.DSN
- topology: ROOT.DSN
- wires/stubs: ROOT.DSN
- resistor values: ROOT.CDB
- resistor reference names: ROOT.CDB
- PWRRAILS.DAT: copy unchanged for v0

## Layout strategy for v0

Use terminal-based branches.

Each resistor branch should be represented as:

```text
terminal NET_A -- short connection -- resistor REF VALUE -- short connection -- terminal NET_B
```

The exact reusable branch template is still pending the 60-test results.

## Component expansion strategy

Do not enable a new component just because it appears in a public/user corpus.

Enable a part only after it has:

- a controlled single-component or small-circuit test
- known Proteus device name
- known pin mapping
- known visual template or generation method
- validator rules
- at least one successful generated/resaved project test

## Future supported component groups

Priority after resistor generator v0:

1. DC voltage source
2. AC voltage source
3. capacitor
4. inductor
5. LED
6. switch / DIP switch
7. clock
8. logic probe
9. 74xx basic gates
10. 7-segment and decoder circuits

## Non-goals for v0

- arbitrary auto-routing
- all Proteus components
- microcontrollers with firmware
- PCB layout generation
- simulation result verification
