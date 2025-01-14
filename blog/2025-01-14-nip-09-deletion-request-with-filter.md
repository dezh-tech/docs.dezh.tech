---
slug: nip-09-deletion_request-with-filter
title: "NIP-09: Deletion Request with Filter"
authors: k
tags: ["nip09","delete","nostr"]
image: "/img/2025-01-14-nip-09-deletion-request-with-filter-1.jpg"
---

![NIP-09: Deletion Request with Filter](/img/2025-01-14-nip-09-deletion-request-with-filter-1.jpg)

# NIP-09: Deletion Request with Filter

## Abstract

In this article, we explain how deletion requests on Nostr work and propose improvements using filters.

## NIP-09: Deletion Request

As you know, we can request the deletion of an event that we own using a kind 5 event, as shown below based on NIP-09:

```json
{
  "kind": 5,
  "pubkey": "<32-bytes hex-encoded public key of the event creator>",
  "tags": [
    ["e", "dcd59..464a2"],
    ["e", "968c5..ad7a4"],
    ["a", "<kind>:<pubkey>:<d-identifier>"],
    ["k", "1"],
    ["k", "30023"]
  ],
  "content": "these posts were published by accident"
  // other fields...
}
```

There are two important limitations to this model:

1. We can't request the deletion of a large number of events with complex conditions in one request. Such a request would result in an event with many tags, which some relays might reject due to the number of tags. For example, deleting all your reactions to posts from one of your followings could cause issues.
2. There is no guarantee that a kind 5 request will be executed by all relays and clients since they can keep a copy of events indefinitely.

## Deletion Using Filters

Based on the limitations of the old model in NIP-09, we explored alternative solutions. Deletion using filters is a new model that addresses the first issue. The second issue is still under research.

Nostr uses a query model called `filter` to query events. Filters can match events and return one of two results: matched or not matched. Using filters, we can define a structure that matches multiple events.

We introduce a new tag called `filter`, which contains a stringified Nostr filter. Using this tag, we can construct a kind 5 event to request the deletion of all matching events. Here's an example:

```json
{
  "kind": 5,
  "pubkey": "aca682c51c44c9046461de0cb34bcc6338d5562cdf9062aee9c3ca5a4ca0ab3c",
  "tags": [
    ["filter", "{\n  \"authors\": [\"aca682c51c44c9046461de0cb34bcc6338d5562cdf9062aee9c3ca5a4ca0ab3c\"],\n  \"kinds\": [7],\n  \"#p\": \"aca682c51c44c9046461de0cb34bcc6338d5562cdf9062aee9c3ca5a4ca0ab3c\",\n  \"since\": 0,\n  \"until\": 2147483647\n}"]
  ],
  "content": "I don't want to be selfish anymore."
  // other fields...
}
```

In this example, [`aca682c51c44c9046461de0cb34bcc6338d5562cdf9062aee9c3ca5a4ca0ab3c`](https://njump.me/example@dezh.tech) requests the deletion of all their reactions on their events.

Or, using the example below, you can request account deletion:

```json
{
  "kind": 5,
  "pubkey": "aca682c51c44c9046461de0cb34bcc6338d5562cdf9062aee9c3ca5a4ca0ab3c",
  "tags": [
    ["filter", "{\n  \"authors\": [\"aca682c51c44c9046461de0cb34bcc6338d5562cdf9062aee9c3ca5a4ca0ab3c\"]\n}"]
  ],
  "content": "Use my new pubkey: cc6...a0a."
  // other fields...
}
```

### Filter Limitations and Details

Here are some limitations and considerations for the `filter` tag on kind 5 events:

1. `authors`: This field must implicitly contain one pubkey, the deletion author's pubkey. Some relays **may** accept multiple pubkeys for moderator events, but it's **recommended** to use the NIP-86 management API, [gRPC API](https://docs.dezh.tech/docs/immortal/managment/apis), or other relay management tools.
2. `limit`: This field **must** be ignored, as limit selection may vary within relay/client systems.
3. `search`: A relay **may** support NIP-50 `search` for this purpose.
4. `until/since`: These fields are used instead of the event's `created_at` for time. Relays **may** follow these behaviors:
   - Only allow pubkeys to request deletion of old events, considering the `since` field.
   - Maintain a list of deleted filters in a hashmap (using `pubkey` as the key) to check incoming events against the filter. Filters can expire once the `until` field is reached. This approach may be resource-intensive, so relays **may** charge a fee or require proof of work to accept kind 5 events in this case.

Thanks to [mike@mikedilger.com](https://njump.me/mike@mikedilger.com) for sharing ideas on [this PR](https://github.com/nostr-protocol/nips/pull/1509#issuecomment-2587967823).

### Use Cases

Here are some use cases for this model:

1. **Account Deletion Request**: A pubkey can request the deletion of their account.
2. **Bulk Removal**: Chat clients can support selecting multiple messages and requesting deletion.
3. **Force Limit**: Relays that store filters using the `until` field can block specific events matching the filter for a defined time.

## Final Words

This standard is currently in beta and has not yet been merged into the [NIPs](https://github.com/nostr-protocol/nips) repository. It is implemented and supported in the [Immortal](https://github.com/dezh-tech/immortal) relay.

1. [Open PR on NIPs](https://github.com/nostr-protocol/nips/pull/1509).
2. [Example implementation in Golang](https://github.com/dezh-tech/immortal/pull/110).
