# Test Generation Methodology

This document captures the practical method used to generate the research test packs.

## Guiding principle

Use the smallest change that can answer one question.

Examples:

- change ROOT.CDB only to test metadata authority
- change ROOT.DSN only to test visible/topology authority
- keep PROJECT.XML and scripts constant unless testing version/container behavior
- include a control file beside each generated test

## Current test-pack workflow

1. Pick a known-good user-created `.pdsprj` control.
2. Extract internal files.
3. Decide the hypothesis.
4. Replace or patch only the internal files needed to test that hypothesis.
5. Normalize version metadata if targeting Proteus 8.13.
6. Repack as `.pdsprj`.
7. Include a personalized README/checklist.
8. User opens the project in Proteus and records observations.
9. Compare resaved file with generated file and control.
10. Promote stable findings into `knowledge/rules.json`.

## Examples of hypotheses already tested

### CDB value authority

Patch resistor value in ROOT.CDB only while leaving ROOT.DSN unchanged.

Outcome: Proteus displayed/property value followed ROOT.CDB.

### DSN topology authority

Swap series/parallel ROOT.DSN while keeping ROOT.CDB the same.

Outcome: visual topology followed ROOT.DSN.

### Empty CDB regeneration

Use full ROOT.DSN with minimal ROOT.CDB.

Outcome: Proteus can often rebuild metadata for visible components after open/save, especially single-sheet projects.

### Version warning source

Patch PROJECT.XML only, then patch PROJECT.XML plus ROOT.DSN header.

Outcome: ROOT.DSN header fields were needed to remove later-version warning.

## How personalized README files should be written

Each generated test folder should include:

```text
README_TEST_THIS_FILE.txt
OBSERVATIONS_TO_FILL.txt
CONTROL_original_source.pdsprj
TEST_generated_trial.pdsprj
```

The README should include:

- source project used
- exact internal file changes
- documented leap of faith
- what should appear visually
- what result means if it opens
- what result means if it fails
- yes/no checklist

## Current correction

Do not use uncertain later-version sheets as base shells. Use the user-provided `E001_empty_project` as the default blank base for future single-sheet generation trials.
