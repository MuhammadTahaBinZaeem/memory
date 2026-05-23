# Project 2 Level 1 Ambitious Trial Pack

Generated pack:

```text
P2_LEVEL1_AMBITIOUS_TRIALS_PACK.zip
```

## Project target

CEP Project 2 Level 1 requires taking a 4-digit password from the user as input and storing it in memory, with each digit as a 4-bit number.

## User request

The user asked to move from small component tests into hit-and-test whole-circuit generation, using the first 8.16 DLD test as the base shell, avoiding the pro2 project as a source, and taking documented leaps of faith.

## Trial strategy

The pack contains 10 generated trials.

The trials use user-created Proteus projects as DSN/CDB sources, version-normalized to Proteus 8.13:

- DLDLab13 first 8.16 test as shell/control
- LCA switch/LED project as input-button substitute
- SR_NAND as one-bit NAND memory primitive
- D flip-flop / JK / T flip-flop DLD projects as sequential memory/control primitives
- DIGITAL_CLOCK_MASTER as non-pro2 complex IC source for DFF/MUX/display-like blocks

The pro2/tahaa project was intentionally not used as a DSN/CDB source.

## Generated trials

1. `G01_BASE_first_816_test_v813_control`
2. `G02_INPUT_SWITCH_BANK_from_LCA_switches`
3. `G03_ONE_BIT_SR_MEMORY_from_SR_NAND`
4. `G04_D_LATCH_SLICE_from_Dflipflop`
5. `G05_LOAD_CONTROL_from_JK_project`
6. `G06_DIGIT_ADVANCE_from_T_flipflop`
7. `G07_MUX_DISPLAY_CORE_from_digital_clock_empty_cdb`
8. `G08_MUX_DISPLAY_CORE_from_digital_clock_full_cdb`
9. `G09_P2_LEVEL1_PRE_FINAL_clock_core_renamed`
10. `G10_FINAL_P2_LEVEL1_password_store_big_leap`

## Main leaps of faith

- A complete Project 2 Level 1 design can be assembled from a DFF/control/mux/display framework rather than copying the known pro2 implementation.
- Digital clock DFF/MUX/display topology can serve as a base for a password-store candidate because both require registers, digit selection, and display/control logic.
- Version-normalized DSN/CDB swaps remain openable in Proteus 8.13.
- Empty CDB is useful for diagnosing visible DSN viability, but full CDB is used where metadata preservation matters.

## Testing protocol

User should test G01 first.

If G01 passes, test G10 final first.

If G10 fails, test G02-G09 to identify the layer that fails:

- input switching
- memory primitive
- D latch primitive
- load/control primitive
- digit advance primitive
- MUX/display complex IC core
- full metadata preservation

## Important caveat

These are hit-and-test candidate circuits, not final guaranteed Project 2 Level 1 implementations. They are designed to reveal whether large DSN/CDB recombinations can produce openable and useful complex-circuit starting points.
