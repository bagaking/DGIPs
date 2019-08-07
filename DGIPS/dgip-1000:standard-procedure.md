---
dgip: 1010
title: Standard Procedure
author: bagaking <kinghand@foxmail.com>
status: Draft
type: Kernal
created: 2019-08-08
---

## Abstract

The SP *(Standard Procedure)* is the atomic operation of processing metadata and transforming it into real NFT data and state changes.

Corresponding to the different state changes of the NFT, each SP has four such atomic `procedure operations`, namely `merge`, `prepare`, `commit`, `abort`.

## Procedure operations

There are four procedure operations.

- [1011] procedure operations merge
- [1012] procedure operations prepare
- [1013] procedure operations commit
- [1014] procedure operations abort

## Standard Procedure

Based on the five metas mentioned in the [dgip-1001:meta-operations](./dgip-1000:meta-operations.md), there are five corresponding SPs. Specific definition and descriptions of the five SP are provided in DGIPs listed below.

- [1021] create procedure
- [1022] delete procedure
- [1023] update procedure
- [1024] factor procedure
- [1025] handover procedure
