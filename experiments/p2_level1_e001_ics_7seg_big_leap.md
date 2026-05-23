# Project 2 Level 1 - E001 IC + 7SEG Big-Leap Pack

Generated pack:

```text
P2_LEVEL1_E001_ICS_7SEG_BIG_LEAP_PACK.zip
```

## User correction applied

The previous final candidate incorrectly used too much of an existing clock circuit. This new pack uses the user-provided known-good `E001_empty_project` as the base shell.

## Project target

Project 2 Level 1: take a 4-digit password as user input and store it in memory, with each digit represented as 4 bits.

## Generation direction

The pack uses an E001 base and inserts trial circuits built from known-good primitive sheets. It avoids the pro2/tahaa project.

The pack uses actual IC/display references from other user-provided projects:

- 7400/7410 NAND/control primitives
- D flip-flop storage primitive
- T flip-flop/digit-advance primitive
- 7404/7408/7432/7483/7486 logic IC bank
- 74LS47 + 7SEG-COM-ANODE display block
- LOGICSTATE / LOGICPROBE / CLOCK style test inputs/outputs

## Connection strategy

The intended long-term strategy is terminal/net based:

- input/output terminals or logic labels with the same name create virtual connections
- avoid long manual wire routing
- refine successful trials into clean terminal-labeled blocks

## Generated trials

1. `G01_E001_REPACK_CONTROL`
2. `G02_INPUT_CLOCK_LOGICSTATE_PROBE_CANVAS`
3. `G03_SR_NAND_MEMORY_PRIMITIVE`
4. `G04_D_FLIPFLOP_STORAGE_SLICE`
5. `G05_7400_7410_JK_LOAD_CONTROL_CORE`
6. `G06_7410_TFLIPFLOP_DIGIT_ADVANCE_CORE`
7. `G07_ACTUAL_LOGIC_IC_BANK_7404_7408_7432_7483_7486`
8. `G08_7SEG_74LS47_DISPLAY_OUTPUT_BLOCK`
9. `G09_TWO_SHEET_CONTROL_AND_7SEG_COMBO`
10. `G10_FINAL_ONE_SHEET_P2_LEVEL1_TERMINAL_STYLE_BIG_LEAP`

## Important technical leap

The generator composes visible `ROOT.DSN` circuit blocks into the E001 shell and uses minimal CDB, expecting Proteus to rebuild metadata where possible.

This is hit-and-test, not guaranteed final generation.

## Testing order

1. Test G01 first.
2. If G01 passes, test G10.
3. If G10 fails, test G02-G09 to identify which primitive class failed.

## What to record

For each trial:

- opens yes/no
- warning/error text
- actual ICs visible yes/no
- seven-segment visible yes/no when expected
- logic inputs/probes visible yes/no
- Save As works yes/no
- attach resaved file if possible

## Chain-of-thought note

Private hidden reasoning is not stored in this repository. The repository stores shareable engineering rationale, reproducible methodology, decision logs, and the utility code used to generate/patch trials.
