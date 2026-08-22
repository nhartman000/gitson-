# `.gitson` — Auxiliary Git-Oriented Structured Transport Format

`.gitson` is an experimental structured object-notation format from Nicholas Hartman / American Milestone Inc. for packaging structured machine-readable content for version-controlled / Git-oriented workflows.

## Current status

`.gitson` is **auxiliary / legacy relative to the current canonical MG8 core file family**.

The current MG8 core is:

```text
.mg8pk
.mg8
.ork
.gst
.g8son
.qson
```

`.gitson` is not required for a conforming MG8 unit and should not be described as the container for all G8SON gates in current canonical documentation.

## Original design motivation

Earlier `.gitson` work explored structured transport optimized for repository tooling and bounded file handling. One early README tied the format to a specific GitHub Copilot file-size limit.

That numeric platform limit is intentionally **not** part of the file-format definition here. External service limits can change independently of the format. Implementations that target GitHub, Copilot, or another platform should apply a versioned adapter/profile for the service's current limits rather than redefining `.gitson` itself.

## Relationship to current work

Potential roles for `.gitson` remain auxiliary, for example:

- Git/repository transport;
- chunked structured interchange;
- repository-oriented manifests or sidecars;
- compatibility with earlier MGate/MG8 experiments that already reference `.gitson`.

Those roles should remain separate from the current meanings of:

- `.g8son` — bounded conditional gate/operator definitions;
- `.gst` — structured state/context;
- `.qson` — auditable execution traces;
- `.ork` — orchestration;
- `.mg8` — bounded execution unit/container.

## Compatibility policy

Existing historical `.gitson` artifacts should be preserved for provenance and reproducibility. New systems should not silently reinterpret a `.gitson` file as a canonical `.g8son` or `.mg8` file.

## Naming

No expanded phrase for **GITSON** is asserted here unless/until an authoritative expansion is established in the project source history. The file extension and project name are preserved without inventing a backronym.

## Canonical references

- MG8: https://github.com/nhartman000/mg8
- GST: https://github.com/nhartman000/gst
- G8SON: https://github.com/nhartman000/g8son
- QSON: https://github.com/nhartman000/qson-
- TCTA: https://github.com/nhartman000/TCTA-

See [`STATUS.md`](STATUS.md) for the compatibility/provenance boundary.
