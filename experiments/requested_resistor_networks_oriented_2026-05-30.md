# Requested Resistor Networks Oriented, 2026-05-30

## Purpose

The user correctly objected that the first 15 requested resistor-network batch still emitted horizontal resistor symbols, so bridge/star/ladder layouts were not visually representative.

This follow-up batch verifies a generator update that writes real Proteus resistor rotation fields and generates vertical endpoint stubs.

## Evidence Used

Public Proteus sample projects contain resistor visual records where the four bytes after final model placement `x/y` encode rotation:

```text
00 00 00 00 = horizontal
7c fc 00 00 = -900 tenths of a degree, vertical down
```

Wire endpoint records in the same samples confirm that a vertical-down resistor uses first pin `(x, y)` and second pin `(x, y - 1270000)`.

## Main Generator Output

Generated in:

```text
D:/Coding/protuesgen/experiments/requested_resistor_networks_oriented_2026_05_30
```

Every case has:

```text
input.json
<case>.pdsprj
<case>.ROOT.CDB.bin
<case>.ROOT.DSN.bin
manifest.json
README_TEST_FIRST.txt
generator_version.txt
```

## Static Results

```text
15/15 generated
0 static validation issues
pytest: 26 passed, 40 subtests passed
```

Angle scan over generated outputs:

```text
01_SIMPLE_LOOP                         [-900]
02_SERIES_CIRCUIT                      [0, 0, 0, 0]
03_PARALLEL_CIRCUIT                    [-900, -900, -900, -900]
04_SERIES_PARALLEL_COMBO               [0, -900, -900, -900]
05_BASIC_VOLTAGE_DIVIDER               [-900, -900]
06_MULTI_STEP_VOLTAGE_DIVIDER          [-900, -900, -900, -900, -900]
07_CURRENT_DIVIDER                     [-900, -900, -900, -900, -900]
08_DELTA_NETWORK                       [0, -900, 0]
09_STAR_Y_NETWORK                      [-900, -900, 0]
10_DELTA_TO_STAR_SETUP                 [0, -900, 0, -900, -900, 0]
11_WHEATSTONE_BRIDGE                   [-900, -900, -900, -900]
12_BALANCED_WHEATSTONE_BRIDGE          [-900, -900, -900, -900, 0]
13_UNBALANCED_WHEATSTONE_BRIDGE        [-900, -900, -900, -900, 0]
14_H_BRIDGE_RESISTOR_VERSION           [-900, -900, -900, -900, 0]
15_R_2R_LADDER_NETWORK                 [0, 0, 0, 0, -900, -900, -900, -900, -900]
```

## Remaining Limitation

This fixes horizontal-only resistor objects for 90-degree vertical layouts. Arbitrary diagonal resistor symbols and fully routed visible bus/junction geometry are still experimental and must not be treated as locked until a separate donor/evidence batch passes Proteus GUI open/save testing.
