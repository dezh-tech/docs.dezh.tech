---
sidebar_position: 2
---

# Manager

This section contains the information about the manager rules and responsibilities. To findout how relays interact with manager and vice versa you can check the [APIs](./apis.md) section.

## White/Black Listing

The immortal implementation supports the white list and black list model to have the ability to be a paid relay or ban users based on the operator decision.

The list won't be passed to relays using APIs; instead the manager writes this into a cuckoo filter on redis and relays read it there and they check events `pubkey` on these filters.

## NIP-11

Since immortal supports multi-instance model, we can't provide the NIP-11 on document on each relay. Instead of this the web server routes NIP-11 requests to manager and manager will return the document:

Example nginx config:

> TODO.

## Reports

The report events will be stored on different mongo db collection (see: [database](../code/database.md)). This database is shared between manager and relays. manager will read reports from this collection and list them for operator  to take actions on them.

