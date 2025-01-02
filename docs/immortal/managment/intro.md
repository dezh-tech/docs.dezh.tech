---
sidebar_position: 1
---

# Introduction

One of the benefits of immortal relay for big services, clients and operators is that it can have multiple instances running at the same time with a load balancer in different servers, regions, providers and more. Which helps immortal to be scalable.

For this purpose these instances would require a service which manages them, checks their health status, updates the global states and more. 

This document will talk about hwo this manager works and uses the [Kraken](https://github.com/dezh-tech/kraken) as an example of manager implementation.