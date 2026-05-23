# Proteus Native Project Generator Memory

This repository is the permanent project memory for building a lawful Proteus project-file generation workflow based on user-created test projects.

Current target:

- Proteus 8.13 first
- Native `.pdsprj` container creation/editing
- No modification of Proteus executables
- No license circumvention
- Generator input language: CircuitIR JSON
- Initial supported generation domain: terminal-based resistor networks

The repo is designed so Codex or another coding agent can read the knowledge files, schemas, and docs to build the validator/generator.

## Current architecture

```text
User prompt
  -> planner prompt / external AI
  -> CircuitIR JSON
  -> validator
  -> Proteus native generator
  -> .pdsprj
  -> Proteus test
  -> feedback JSON
  -> knowledge update
```

## Important directories

```text
knowledge/   confirmed rules, component database, open questions, test results
docs/        architecture, file model, validator/generator design
schemas/     JSON schemas for CircuitIR and feedback
prompts/     prompts for converting natural language into CircuitIR
experiments/ experiment protocols and phase notes
src/         future Python package
```
