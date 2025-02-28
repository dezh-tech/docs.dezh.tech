---
sidebar_position: 5
---

# Database

The database used in immortal is mongo db since it supports json documents which is a proper way to store nostr events. Also, its easy to scale mongo db as well.

We only use mongo to keep events for long run. for querying events we use meilisearch which we synced all mongo db events to it.

## Queries

In immortal we send queries to meilisearch and we also support `search` field (NIP-50) as well.

## Deleting

To delete some document for a NIP-09 request or Expiration or operator order from manager, we set all fields `null` using and we only keep the `id` of event. this helps us to prevent an deleted event be rewritten or returned to user.
