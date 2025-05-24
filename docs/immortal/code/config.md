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

Also there is a section called `manager` which is the information of a manager, which the immortal instance will connects to it instantly after initializing the modules and receive the parameters from the manager.

### Notes

There is more data shared between relay and manager such as white/black listed pubkeys. they are shared using a cuckoo filter on the redis instance.
