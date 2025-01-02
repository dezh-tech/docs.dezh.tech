---
sidebar_position: 5
---

# Database

The database used in immortal is mongo db since it supports json documents which is a proper way to store nostr events. Also, its easy to scale mongo db as well.

Each nostr event kind has a different collection on mongo db database. This helps to keep events more organized, query based on kinds and have less event in each collection instead of keeping whole events in one place such as all short notes and reactions in the same collection which is inefficient.

## Queries

When immortal wants to query something from mongo db, its basically a filter. The immortal turns the normal filter and makes `n` mongo queries. `n` is the number of kind numbers in filter. it would attach the whole conditions of filter to each query and execute the queries. Currently queries are sent separately which is inefficient, we are working on it to support batch queries or increase the performance.

> The special case is when someone requests a filter without kinds field, in this case immortal makes a request to some papular collections like reactions, short text notes, zaps and some more.

## Deleting

To delete some document for a NIP-09 request or Expiration or operator order from manager, we make ALL fields removed (not empty!) and we only keep the id of event. this helps us to prevent an deleted event be rewritten.
