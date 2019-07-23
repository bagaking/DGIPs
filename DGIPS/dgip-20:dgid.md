---
dgip: 20
title: Decentralized Game's Internal iDentifier
author: bagaking <kinghand@foxmail.com>
status: Draft
type: Core
created: 2019-07-23
---

## Simple Summary

A standard for ids inner decentralized game.

## Abstract

The following standard provides a basic regulation of dgid, which is the core method to identify a user in a dgame center.

## DG-ID

|sign|payload|
|--|--|
|1~9|0,000,001~9,999,999|

**DG-ID algorithm:** An dgid consists of 1 decimal bits' sign, and a 7 decimal bits' payload. The exact definition of payload is determined by sign. Based on what the sign value is, the entire domain can be divided into 3 categories - `system domain`, `user domain` and `reserved domain`.

### System Domain

|sign|game_id|sequence|
|--|--|--|
|1|0,001~9,999|000~999|

- sign(1 decimal bit)  
The highest decimal bit is always 1.

- game_id (4 decimal bits)  
The next 4 decimal bits, represents the game id, maximum value will be 10 Thousand.

- sequence (3 decimal bits)
The last 3 decimal bits, sequence within the game, maximum value will be 1 Thousand. It can be defined by the corresponding game, for system account's usage.  
In particular, when the sequence is equal to 0, it represents the `default dgid` for this game.  
  > e.p. for game `game_id:00001`, its default dgig should be `dgid:10001000`

### User Domain

|sign|sequence|
|--|--|
|2|0,000,001~9,999,999|

- sign(1 decimal bit)  
The highest decimal bit is always 2.

- sequence (7 decimal bits)
The last 7 decimal bits, represents the users' sequence among the dgame_center, maximum value will 10 million - 1. It will be directly generated by the register module of game_center.  

### Reserved Domain

|sign|sequence|
|--|--|
|3~9|0,000,001~9,999,999|

The reserved domain is reserved for other id purposes, not yet assigned.