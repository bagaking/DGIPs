---
dgip: 1002
title: Reducer
author: bagaking <kinghand@foxmail.com>
status: Draft
type: Kernal
created: 2019-08-08
---

## Abstract

In reducer, all procedure options of all metas are reduced to four reducer method:
`reduceMerge`, `reducePrepare`, `reduceCommit`, `reduceAbort`.

And the only way for tconv to interact with nft is through these 4 methods.

## Reducer

Reduce merged-metas and generate reducer.

### reducer merge

1. execute all merge operation of procedures, generate merged-metas.

### reducer prepare

1. checking all lock status
2. execute all prepare operation in merged-metas.
3. persist the reducer.  

### reducer commit

1. update reducer's state and record conservation_records
2. execute all commit operation in merged-metas.
3. remove nft updating lock

### reducer abort

1. update reducer's state
2. execute all abort operation in merged-metas.
3. remove nft updating lock

## Lock conditions

### lock level

There are 2 different lock level

1. pending lock

    To mark a meta procedure should check pending locks, it should be record into the reducer-context's checkPendingLocks structure.

    ```typescript
    nft.pending_locker === ""
    && (nft.occupier === 0
        || nft.occupier === ctx.game_id);
    ```

2. updating lock

    To mark a meta procedure should check updating locks, it should be record into the reducer-context's checkUpdatingLocks structure.

pending-lock **SHOULD** be added or removed by create procedure or delete procedure.

updating-lock **SHOUD** be add in reducer's prepare method, and be delete in reducer's commit or remove method

### operation locks

1. these meta operation should be marked with `checkPendingLocks`
   1. handover
   2. update
   3. factor

2. these meta operation should be marked with `checkUpdatingLocks`
   1. delete

3. these meta operation should add `update_locks`
   1. handover
   2. update
   3. factor

### lock checking

All lock status will be checked in the method reducePrepare, and all `updating-lock` will be added right after all status are checked.
