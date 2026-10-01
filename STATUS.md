# GITSon Status and Compatibility Boundary

**Author:** Nicholas Hartman / American Milestone Inc.

## Status

GITSon is **active**: the canon-bundle transport format accompanying
`.mg8` outputs handed off to a receiving LLM. The bundle carries the
NYCH + MG8 canon (full rules/spec text or pointers to the canonical
GitHub repositories) so the receiver can ingest the rules needed to
interpret the `.mg8` output without prior context.

### Status history

An earlier revision of this repository recorded GITSon as
auxiliary/legacy relative to the MG8 core family. That framing is
superseded by deliberate repurposing (2026-10): the format was reassigned
the canon-ingestion transport role described above. The history is kept
here rather than erased.

The MG8 core execution family is unchanged and GITSon does not replace
any member of it:

```text
.mg8pk → .mg8 → .ork / .gst / .g8son → .qson
```

## Platform-size limits

Retained principle from the earlier revision: platform-specific size
restrictions are external constraints. The splitter takes the current
limit as a configurable parameter; it is never hard-coded as a permanent
semantic property of the `.gitson` format. Oversized canon blocks are
demoted to repository references — disclosed, never truncated.

## Historical compatibility

Pre-repurposing `.gitson` artifacts remain valid historical development
material within their original implementation profile. They are not
evidence that current `.g8son` files require a `.gitson` wrapper, and
current bundles are identified by their `gitson_version` envelope field.

## Migration principle

When modernizing an older implementation:

1. determine what semantic role the historical `.gitson` artifact
   actually performs;
2. preserve it if required for reproduction;
3. map gate definitions to `.g8son`, state to `.gst`, orchestration to
   `.ork`, trace events to `.qson`, and bounded units to `.mg8` where
   appropriate;
4. do not rename files mechanically without validating their semantics.

## Provenance

This repository is retained so both the historical format and the
repurposing decision remain visible in the development record.
