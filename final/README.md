# Final Locked Baseline

This folder is the locked baseline for the Proteus native project generator work.

It combines the recovered V9 resistor-terminal method, the corrected hand-drawn 6-resistor case, the JSON circuit description format, and the Codex implementation requirements for the next generator.

## Current target

```text
Input:  JSON circuit description
Output: Proteus 8.13 .pdsprj project generated from E001 empty base
Method: V9 linked terminal/resistor/wire group method
Current component support: RESISTOR only
```

## Files

```text
final/README.md
final/locked_v9_method.md
final/json_circuit_ir_spec.md
final/ai_json_authoring_guide.md
final/codex_generator_requirements.md
final/locked_handdrawn_6r_case.md
final/examples/handdrawn_6r_corrected.json
final/templates/generation_manifest_template.json
final/roadmap_components.md
```

## Locked success case

Corrected hand-drawn 6R topology:

```text
R1: N0 - N1
R2: N1 - N2
R3: N0 - N2
R4: N2 - N3
R5: N3 - N4
R6: N0 - N4
```

Generated file recorded as correct:

```text
HANDDRAWN_6R_CORRECTED_N0_N1_N2_N3_N4.pdsprj
```

Recorded hashes:

```text
project_sha256: 2f5eb8226c825736ca5ff15dd48177c3c5b9c52364f9eeea9f2bf9ba62018476
zip_sha256: e54b7c0be48798635a6942b08b4237096e96408b57de87c15682451ac9784f7a
```

## Mandatory rule

Every future generation must be recorded in this repository with source, interpretation, JSON input, script/generator version, manifest, generated output hashes, validation result, user feedback, limitations, and next action.
