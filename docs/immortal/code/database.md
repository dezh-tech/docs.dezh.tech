---
sidebar_position: 5
---

# Database

The database used in immortal is mongo db since it supports json documents which is a proper way to store nostr events. Also, its easy to scale mongo db as well.

Each nostr event kind has a different collection on mongo db database. This helps to keep events more organized, query based on kinds and have less event in each collection instead of keeping whole events in one place such as all short notes and reactions in the same collection which is inefficient.

## Queries

When immortal wants to query something from mongo db, its basically a filter. The immortal turns the normal filter and makes `n` mongo queries. Then we use the mongo db aggregation pipeline and `$unionWith` stage to query all requested collections. If you kinds was provided it will query all tables.

## Deleting

To delete some document for a NIP-09 request or Expiration or operator order from manager, we set all fields `null` using `$unset` mongo command and we only keep the `id` of event. this helps us to prevent an deleted event be rewritten or returned to user.
