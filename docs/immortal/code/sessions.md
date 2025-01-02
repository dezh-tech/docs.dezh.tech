---
sidebar_position: 4
---

# Sessions

The Immortal keeps a set of information with each websocket connection called client session.


## NIP-42 Information

The first information is `pubkey` which is a string of user pubkey in hex format and `isKnown` which is a boolean showing wether this session is passed the NIP-42 process. If `isKnown` was false, it means pubkey is empty.
