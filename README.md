# zyntor-commitments

Public, timestamped commitments for the ZYNTOR research project.

This repository holds **hashes only**. It contains no market data, no code, no strategy rules and no results. Each commitment proves that a specific set of private files existed, byte for byte, no later than the time it was published and timestamped.

## What is in here

Files in `manifests/`, added in time order and never edited or deleted:

| File | What it is |
|---|---|
| `<UTC>_C-n.txt` | A manifest: the SHA-256 of each committed file at that moment |
| `<UTC>_C-n.txt.sha256` | The commitment: the SHA-256 of the manifest itself |
| `<UTC>_C-n.txt.ots` | An OpenTimestamps proof for the manifest, anchored in Bitcoin |

`C-n` is the kind of event: C-1 approvals and locks, C-2 daily digest, C-3 holdout start and end, C-4 before unseal, C-5 halts, C-6 owner decisions.

## Manifest format (ZYNTOR-C v1)

UTF-8, LF line endings, one final newline:

```
ZYNTOR-C v1
type=C-1 utc=2026-11-24T10:00:00Z
prev=<SHA-256 of the previous manifest, or 64 zeros for the first>
<sha256>  <path>
<sha256>  <path>
```

Paths are sorted by their UTF-8 bytes. The `prev` line links each manifest to the one before it, so removing or changing an earlier manifest breaks every later link.

## How to check it yourself

1. **Commitment:** the SHA-256 of each `.txt` must equal the value in its `.sha256` file.
2. **Chain:** each `prev=` must equal the SHA-256 of the previous manifest; the first is 64 zeros.
3. **Time:** `ots verify <file>.txt.ots` (needs `pip install opentimestamps-client` and a Bitcoin block source) shows the earliest block the manifest is anchored in.
4. **Files:** anyone later given a committed file can hash it and compare it to the manifest line.

The Git commit dates in this repository are not evidence on their own, because they can be set freely. The OpenTimestamps proofs are the time evidence.

## What this does not prove

A commitment shows that files existed unchanged by a certain time. It does not show that their contents are correct, that no other work was done privately, or that any strategy is profitable.
