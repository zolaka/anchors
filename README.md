# Zolaka anchors

Once a day, Zolaka publishes one Merkle root here. The root covers the head of every Zolaka workspace's event log at that moment. Each workspace receives an inclusion proof for its own head. Anyone holding a workspace's export can then show that the log was in that state by that date, without relying on Zolaka.

## Files

- `anchors/YYYY-MM-DD.json`: one file per day (UTC), holding the Merkle root and the number of leaves.

## How a root is built

- **Leaves:** one per workspace, the RFC 8785 canonical JSON of `{ "head_hash", "head_seq", "tenant" }`.
- **Hashing:** as RFC 6962 (Certificate Transparency) specifies.
  - A leaf hashes as `SHA-256(0x00 || leaf)`.
  - An interior node hashes as `SHA-256(0x01 || left || right)`.
  - The tree has RFC 6962's `MTH` shape, with leaves sorted by leaf hash.
- **What's public:** only the root and the leaf count. Nothing here identifies a workspace or reveals its contents.
- **Bitcoin timestamp:** each root is also timestamped in Bitcoin through [OpenTimestamps](https://opentimestamps.org).

## Rules

- Only the `zolaka-anchors` bot commits here.
- History is never rewritten: a day's file never changes once published.
