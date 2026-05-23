# P2 Level 1 - No-DLD E001 Source Trials

Generated pack:

```text
P2_LEVEL1_NO_DLD_E001_SOURCE_TRIALS_PACK.zip
```

## Reason for this pack

The user reported the same ISIS issue in the previous E001 IC/7SEG pack and suspected anything derived from `DLDLab13` or the DLD lab folder was poisoning the generated projects.

This pack avoids:

- `DLDLab13`
- all files inside the `DLD-Proteus` folder
- `pro2/tahaa`

It uses the known-good E001 project as the base/control and uses only non-DLD sources as reference trials.

## Purpose

Before attempting another full Project 2 Level 1 build, find which non-DLD source families open safely in Proteus 8.13 after version normalization.

## Trials

1. `G01_E001_REPACK_CONTROL` — known-good E001 control.
2. `G02_CLOCK_FULL_CDB_CONTROL` — Digital clock full DSN+CDB, version normalized.
3. `G03_CLOCK_EMPTY_CDB_REBUILD` — Digital clock DSN with E001 minimal CDB.
4. `G04_LV1_FULL_CDB_7SEG_CONTROL` — LV1 Level 1 Implementation full DSN+CDB.
5. `G05_LV1_EMPTY_CDB_REBUILD` — LV1 DSN with E001 minimal CDB.
6. `G06_ASSIGNMENT_1RA_PRUEBA_FULL_CDB` — assignment4 `1ra Prueba` full DSN+CDB.
7. `G07_ASSIGNMENT_1RA_PRUEBA_EMPTY_CDB` — assignment4 `1ra Prueba` DSN with E001 minimal CDB.
8. `G08_LCA_INPUT_PROBE_FULL_CDB` — LCA input/probe source full CDB.
9. `G09_HOME_THEATER_ATMEGA_LCD_FULL_CDB` — HomeTheater ATMEGA/LCD source full CDB.
10. `G10_HOME_THEATER_EMPTY_CDB` — HomeTheater DSN with E001 minimal CDB.
11. `G11_ARDUINO_FULL_CDB` — Arduino 328 source full CDB.
12. `G12_P2_CANDIDATE_NON_DLD_7SEG_CLOCK_CORE` — safe placeholder using non-DLD clock IC+7SEG source.

## Interpretation

If G01 fails, E001/repack method is broken.

If G02 passes, the digital clock source is safe as a non-DLD IC+7SEG donor.

If G03 passes, empty CDB rebuild works for that source.

If G04/G06 pass, those are candidate 7-segment/control donors.

If full-CDB versions pass but empty-CDB versions fail, future trial generation should preserve matching CDB rather than rely on regeneration.

If all non-DLD full-CDB controls pass, the ISIS problem was likely tied to the DLDLab13/DLD-Proteus source family rather than E001 or version patching.

## Next step if these pass

Use the safest passing non-DLD IC+7SEG source as the donor template for a Project 2 Level 1 candidate instead of the DLD lab folder.
