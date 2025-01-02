---
sidebar_position: 3
---

# Spam Protection

Since the Nostr is an open system and there is no need to provide any information for users, there is a high chance to get spammed. 

Immortal performs different actions and validations to prevent this kind of spam attacks.

## Bloom Filter

Immortal writes the ID of any successfully received events into a shared Redis bloom filter. When an immortal instance receives new event it checks the event ID existence on the shared bloom filter to prevent double processing of one event. 

This bloom filter is persist.
  
> If we don't do this check, the whole validation process will be done and event will be passed to databases layer and then we find out this is duplicated. using this check we make sure this event is new on the system.

