---
sidebar_position: 5
---

# Database

The database used in immortal is MongoDB since it supports JSON documents which is a proper way to store the Nostr events. Also, its easy to scale the MongoDB as well.

We only use MongoDB to keep events for long run. for querying events we use the Meilisearch which we synced all MongoDB events to it.

## Queries

In immortal we send queries to the Meilisearch and we also support `search` field (NIP-50) as well.

## Deleting

To delete some document for a NIP-09 request or Expiration or operator order from manager, we set all fields `null` using and we only keep the `id` of event. this helps us to prevent an deleted event be rewritten or returned to user.
