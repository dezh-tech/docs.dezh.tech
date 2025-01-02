---
sidebar_position: 2
---

# Config

The Immortal relay has 2 type of configs:

1. Local file config: this config comes from a yaml file which is dedicated for each immortal instance. [Example](https://github.com/dezh-tech/immortal/blob/main/config/config.yml).
2. Global config (or parameters) : this comes from a [manager service](../managment/intro.md) and is same trough all immortal instances. [Example](https://github.com/dezh-tech/immortal/blob/main/config/parameters.go).


## Local file config

You can find an up to date version of this [file here](https://github.com/dezh-tech/immortal/blob/main/config/config.yml).

This file generally contains the connection information to the redis and mongo db instances such as database names, timeouts and more. the connection secrets are kept in the environment variables.

Also there is a section called `kraken` which is the information of a manager (default is [kraken](https://github.com/dezh-tech/kraken)) which the immortal instance will connects to it instantly after initializing the modules and receive the parameters from the manager.


## Parameters

The Parameters come from a manager and currently immortal don't support hot reload for this config as well as file config.

The Parameters are a set of limitations and information that relay needs to perform. You can find the example [here](https://github.com/dezh-tech/immortal/blob/887ea62f9edeec7e51e71eeac3f967761c31b576/client/proto/kraken.proto#L80).


### Notes

There is more data shared between relay and manager such as white/black listed pubkeys. they are shared using a cuckoo filter on the redis instance.

