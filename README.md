# `.gitson` — Canon Bundle Transport Format

`.gitson` is a structured object-notation format from Nicholas Hartman /
American Milestone Inc. Its current, active purpose: **carry the entire
NYCH + MG8 canon — the full suite explanation and rules, or pointers to
the canonical GitHub repositories — alongside an `.mg8` output, so that a
receiving LLM with no prior context can ingest the rules needed to
interpret that output.**

## Current status

`.gitson` is **active and repurposed**. The earlier framing of this
repository (auxiliary/legacy, excluded from the MG8 core family) is
superseded: when an `.mg8` output is handed off to another LLM, it travels
as `.mg8` **plus** `.gitson`.

The MG8 core execution family is unchanged:

```text
.mg8pk → .mg8 → .ork / .gst / .g8son → .qson
```

`.gitson` sits beside that family as the handoff/ingestion transport. It
is still not a container for `.g8son` gates and does not replace any core
format's semantics.

## Size-limited splitting

A `.gitson` bundle respects repository-platform file-size limits by
splitting into numbered parts: `1.1`, `1.2`, `1.3`, … (bundle sequence,
then part index). Reassembly is deterministic and loss-free.

The numeric size limit is **a configurable parameter of the splitter, not
a property of the format**: external platform limits (GitHub's, or any
other) can change independently of `.gitson` itself, so implementations
pass the current limit in rather than hard-coding it. This preserves the
principle recorded in this repository's earlier documentation.

A canon block too large to fit any single part under the configured limit
is **demoted to a reference** (a pointer to its canonical repository) —
disclosed in the part's metadata — never silently truncated.

## Format

See [`spec/gitson_v1.md`](spec/gitson_v1.md). In brief: each part is one
UTF-8 JSON file with a `gitson_version` / `bundle_id` / `part` /
`parts_total` envelope, a list of canon `blocks` (title + source +
content), and a list of `references` (canonical repo URLs for ingestion).

The reference implementation of the splitter/reassembler lives in
[mg8-engine](https://github.com/nhartman000/mg8-engine)
(`src/mg8_engine/gitson.py`), following the existing convention that spec
repos hold format definitions and mg8-engine holds runtime code.

## Historical compatibility

Pre-repurposing `.gitson` artifacts remain valid as historical development
material within their original implementation profile. New systems should
not silently reinterpret a historical `.gitson` file as a canonical
`.g8son` or `.mg8` file, and should check `gitson_version` before assuming
the current bundle shape.

## Naming

No expanded phrase for **GITSON** is asserted here unless/until an
authoritative expansion is established in the project source history.

## Canonical references

- NYCH: https://github.com/nhartman000/nych
- MG8: https://github.com/nhartman000/mg8
- MG8 reference runtime: https://github.com/nhartman000/mg8-engine
- GST: https://github.com/nhartman000/gst
- G8SON: https://github.com/nhartman000/g8son
- QSON: https://github.com/nhartman000/qson
- TCTA: https://github.com/nhartman000/TCTA
- T.O.T.E-loops: https://github.com/nhartman000/T.O.T.E-loops

See [`STATUS.md`](STATUS.md) for the status history and compatibility
boundary.
