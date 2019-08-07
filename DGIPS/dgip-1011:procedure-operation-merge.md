---
dgip: 1011
title: Procedure Operation - Merge
author: bagaking <kinghand@foxmail.com>
status: Draft
type: Kernal
created: 2019-08-08
---

## Definition

The merge operation of procedure, which aimed at marge the raw meta options and generated data in reducer-context, will be executed before the prepare operation.

In the merge operation:

1. All value changes of same field or bag-entry of the nft **MUST** be reduced, it means that the number of instructions will be greatly reduced.

2. The legality of all static data **MUST** be verified.

3. All relevant necessary auxiliary data structures of the reducer-context **MUST** be filled.

## Static value verification

Some of the following verifications may appear in the static parameter validation summary:

1. format of dgid (see dgip-20)
2. format of nft_id (see dgip-200)
3. format and values of quantity (see dgip-101)
4. meta mutual exclusion (see dgip-1001)
5. meta's validate condition (see dgip-1001)
