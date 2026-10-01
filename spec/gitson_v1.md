# `.gitson` v1 — Canon Bundle Format

One `.gitson` **bundle** is a sequence of one or more **part files**. Each
part file is a single UTF-8 JSON document.

## Part envelope

```json
{
  "gitson_version": "1.0",
  "bundle_id": "canon-2026-10-01",
  "part": "1.2",
  "parts_total": 3,
  "created": "2026-10-01T00:00:00+00:00",
  "purpose": "canon-ingestion",
  "blocks": [ ... ],
  "references": [ ... ],
  "demoted": [ ... ]
}
```

| Field | Meaning |
| --- | --- |
| `gitson_version` | Format version; `"1.0"` for this spec. Receivers must check it before assuming this shape. |
| `bundle_id` | Identifier shared by all parts of one bundle. |
| `part` | `"<bundle_seq>.<part_index>"`, both 1-based: `1.1`, `1.2`, … Part index orders parts within the bundle. |
| `parts_total` | Total part count of the bundle; identical in every part. |
| `created` | ISO-8601 creation timestamp (same in every part). |
| `purpose` | Free string; `"canon-ingestion"` for NYCH/MG8 canon bundles. |
| `blocks` | Canon content blocks carried by **this** part (see below). |
| `references` | Canonical repository URLs the receiver should treat as the authoritative rule sources. |
| `demoted` | Block ids that were too large for the configured size limit and were replaced by a reference instead of truncated. May be empty. |

## Canon block

```json
{
  "block_id": "nych.readme",
  "title": "NYCH package README",
  "source": "https://github.com/nhartman000/nych",
  "content": "..."
}
```

`content` is the verbatim rule/spec text. `source` is the canonical origin
(normally a GitHub repository URL) so a receiver can fetch the live
version. A block appears in exactly one part of a bundle.

## Splitting rules

1. The size limit is **caller-supplied** (bytes per serialized part
   file). Platform limits (GitHub's or any other) are external
   constraints passed in at build time, never hard-coded in the format.
2. Blocks are packed into parts in input order; a part is closed when
   adding the next block would exceed the limit.
3. A single block that cannot fit alone in a part under the limit is
   **demoted**: its `content` is dropped, its `source` is appended to
   `references`, and its `block_id` is recorded in that part's `demoted`
   list. Content is never truncated.
4. Part names on disk follow `<stem>.<part>.gitson`, e.g.
   `canon.1.1.gitson`, `canon.1.2.gitson`.

## Reassembly rules

1. Collect all parts with the same `bundle_id`.
2. Verify every `parts_total` agrees and all part indices
   `1..parts_total` are present, each exactly once.
3. Concatenate `blocks` in part order; union `references` and `demoted`
   preserving first-seen order.
4. Any missing/duplicate part or mismatched `bundle_id`/`parts_total` is
   an error — a partial bundle must not be silently presented as complete.

## Reference implementation

`src/mg8_engine/gitson.py` in
[mg8-engine](https://github.com/nhartman000/mg8-engine):
`build_canon_bundle`, `split_canon_bundle`, `write_parts`, `load_bundle`.
