# Decision Log

This file records the reasoning behind major test directions in a concise, shareable form.

It is not private chain-of-thought. It is the project-level rationale that Codex or another contributor needs.

## D001: Use terminal-based topology first

Decision: avoid long auto-routed wires in early generator versions.

Reason:

- Proteus terminals with identical names act as virtual connections.
- User specifically prioritized POWER/GROUND/INPUT/OUTPUT terminals.
- Terminal labels were confirmed to live in ROOT.DSN.
- This reduces the first generator from arbitrary routing to repeated branch placement and net naming.

## D002: Treat ROOT.DSN as visual/topology authority

Decision: generator must create/update ROOT.DSN for visible objects and topology.

Evidence:

- CDB-only extra resistor entries did not create visible components.
- DSN with additional visible resistors opened even when CDB was incomplete.
- Series/parallel transformations followed ROOT.DSN.

## D003: Treat ROOT.CDB as component metadata authority

Decision: generator should generate ROOT.CDB for polished output, especially resistor refs/values.

Evidence:

- CDB-only resistor value/ref edits were authoritative.
- DSN-only resistor value/ref edits were normalized back from CDB.
- Missing CDB entries can sometimes be rebuilt, but refs may be regenerated.

## D004: Use minimal/empty CDB as trial mode, not final mode

Decision: empty CDB is useful for fast tests, but final generator should not rely on it for clean output.

Evidence:

- Big trials with empty CDB opened for many single-sheet circuits.
- Proteus rebuilt metadata after open/save.
- Refs were often regenerated/reordered.
- Multi-sheet projects collapsed to fewer visible sheets.

## D005: Avoid multi-sheet generation in v0

Decision: first generator target should be single-sheet circuits.

Evidence:

- Empty CDB caused a 6-sheet clock project to show only first sheet.
- Foreign CDB exposed wrong sheet count.
- Multi-sheet support requires CDB sheet metadata handling.

## D006: Patch both PROJECT.XML and ROOT.DSN version fields

Decision: generated projects targeting Proteus 8.13 must patch both places.

Evidence:

- PROJECT.XML-only patch did not remove ROOT.DSN later-version warning.
- ROOT.DSN header patch at offsets 167/169 removed warning in the repack control.

## D007: Use E001 empty project as default base

Decision: use the user-provided E001 empty project as the default base shell for future single-sheet trials.

Reason:

- It is known-good Proteus 8.13.
- It has clean PROJECT.XML and ROOT.DSN version metadata.
- It has minimal ROOT.CDB.
- A previous later-version DLDLab13 base caused every derived test to fail before testing the generated content.

## D008: Move from tiny tests to staged hit-and-test trials

Decision: after authority rules were confirmed, generate larger staged trial packs.

Reason:

- Repeating CDB/DSN authority tests became redundant.
- The user can test generated packs faster than manually building many circuits.
- Big trials reveal whether whole-circuit DSN-heavy generation is viable.

## D009: Keep natural-language planning separate from generator

Decision: generator input is CircuitIR JSON, not free-form English.

Reason:

- Any LLM can later generate CircuitIR.
- Validator can enforce correctness before generation.
- Deterministic generator stays testable and independent of a specific model.
