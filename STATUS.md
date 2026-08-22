# GITSon Status and Compatibility Boundary

**Author:** Nicholas Hartman / American Milestone Inc.

## Status

GITSon is preserved as an **auxiliary / legacy structured transport format**.

It is not part of the current canonical MG8 core family:

```text
.mg8pk → .mg8 → .ork / .gst / .g8son → .qson
```

## Historical compatibility

Earlier repositories and artifacts may use `.gitson` for gate graphs, Git-oriented structured packaging, compact interchange, or other experimental transport functions. Those artifacts remain valid as historical development material within their original implementation profile.

They should not be used as evidence that current `.g8son` files require a `.gitson` wrapper.

## Platform-size limits

Platform-specific size restrictions are external constraints. They must be represented by adapters or compatibility profiles, not hard-coded as permanent semantic properties of the `.gitson` format.

## Migration principle

When modernizing an older implementation:

1. determine what semantic role the historical `.gitson` artifact actually performs;
2. preserve it if required for reproduction;
3. map gate definitions to `.g8son`, state to `.gst`, orchestration to `.ork`, trace events to `.qson`, and bounded units to `.mg8` where appropriate;
4. do not rename files mechanically without validating their semantics.

## Provenance

The separate `.gitson` repository is retained so the historical format remains visible rather than being erased from the development record.
