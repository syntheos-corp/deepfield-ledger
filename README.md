# Deepfield ledger heads

This repository is written by a machine. Once a day (00:05 UTC) Deepfield publishes the head of every stream in its forecast
ledger here, as one file: `heads/<timestamp>.json`.

Deepfield commits each forecast to an append-only ledger before the forecast can be checked against what happened. Each
ledger stream is a hash chain: every event's hash is `sha256(previous hash ‖ canonical bytes of the event)`, so changing,
removing or back-dating any event changes every later hash, including the head. A head published here, in a commit whose time
GitHub records, fixes the chain up to that point. A later export of the ledger that does not reach these heads has been
altered.

## File format

```json
{
  "schemaVersion": 1,
  "publishedAt": "2026-10-16T00:05:00Z",
  "streams": [{ "streamId": "…", "headSeq": 12, "headHash": "<sha256 hex>" }]
}
```

## Checking an export

With a ledger export from Deepfield and any file from `heads/`:

```
pnpm tsx scripts/ledger-check-published.ts <export.json> heads/<timestamp>.json
```

(from the Deepfield repository). It verifies every chain in the export and that each published head appears in it at the
published sequence number.

Files here are never edited or deleted. This repository holds hashes only: no questions, forecasts or evidence.
