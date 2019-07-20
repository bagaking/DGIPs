---
dgip: 100
title: Fungible Tokens Standards
author: bagaking <kinghand@foxmail.com>
status: Draft
type: Standards Track
created: 2019-07-21
requires: dgip-2
---

## Simple Summary

A standard interface for fungible tokens.

## Abstract

The following standard allows for the implementation of a standard API for fungible tokens within an dgame center.
This standard provides basic functionality to transfer tokens.

## Methods

**NOTES**:

- The following specifications use syntax from typescript `3.3.0` (or above)
- Callers MUST handle `error code` from `returns`.  Callers MUST NOT assume that `false` is never returned!

### info

Returns the global status of the fungible token with symbol `_symbol`.

```typescript
info(_symbol: string): Promise<{
  sym: string;
  decimals: number; // UInt8
  maxSupply: string; // BigInt
  supply: string; // BigInt
}>
```

### balanceOf

Returns the account balance of another account with dg_id `_owner`.

```typescript
balanceOf(_owner: number): Promise<{
  sym: string;
  amount: string; // BigInt
}>
```

### create

create a new fungible tokens with symbol `_symbol`, decimal info `_decimal`, maxSupply `_maxSupply`, and MUST fire the `Create` event.  

*Note* maxSupply of -1 values means that the fungible token of `_symbol` is an extenal token which has no supply limits, thus it MUST be treated as normal create and fire the `Create` event.

```typescript
create(_symbol: string, _decimal: number, _maxSupply: string): Promise<{
  status: StatusCode;
  error?: {
    code: ErrorCode;
    msg: string;
  }
}>
```

### issue

Issue `_value` amount of fungible tokens with symbol `_symbol` to address `_to`, and MUST fire the `Issue` event.  
The function SHOULD `throw` if the message caller's authority is not a reliable source.  
The function SHOULD `throw` if the supply is about to exceeds the maxSupply of the fungible token.  
Issue of 0 values SHOULD `throw`.  

```typescript
issue(_symbol: string, _to: number, _value: string): Promise<{
  status: StatusCode;
  error?: {
    code: ErrorCode;
    msg: string;
  }
}>
```

### transfer

Transfers `_value` amount of tokens to address `_to`, and MUST fire the `Transfer` event.  
The function SHOULD `throw` if the message caller's account balance does not have enough tokens to spend.
Transfers of 0 values SHOULD `throw`.

```typescript
transfer(_to: number, _value: string): Promise<{
  status: StatusCode;
  error?: {
    code: ErrorCode;
    msg: string;
  }
}>
```

### burn

Burn `_value` amount of fungible tokens with symbol `_symbol` to address `_from`, and MUST fire the `Burn` event.  
The function SHOULD `throw` if the message caller's account balance does not have enough tokens to burn.
Issue of 0 values SHOULD `throw`.  

```typescript
burn(_symbol: string, _from: number, _value: string): Promise<{
  status: StatusCode;
  error?: {
    code: ErrorCode;
    msg: string;
  }
}>
```
