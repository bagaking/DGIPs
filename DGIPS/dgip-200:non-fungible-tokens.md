---
dgip: 200
title: Non-Fungible Tokens Standards
author: bagaking <kinghand@foxmail.com>
status: Draft
type: Standards Track
created: 2019-07-22
requires: dgip-12
---

## Simple Summary

A standard interface for non-fungible tokens.

## Abstract

The following standard allows for the implementation of a standard API for non-fungible tokens(nfts) within a dgame center. This standard provides basic functionality to issue/transfer/burn/occupy/release nfts.

## GAME NFT-ID

|sign|game_id|sequence|
|--|--|--|
|1|00,01~99,99|0,000,000,000,000~9,999,999,999,999

**NFT-ID algorithm:** An unique id consists of game id and sequence within the game. Usually, it is a 18 decimal bits number(long), and the default bits of that two fields are as follows:

- sign(1 decimal bit)  
The highest decimal bit is always 1.

- game_id (4 decimal bits)  
The next 4 decimal bits, represents the game id, and it's domain size will be 10 Thousand - 1.

- sequence (13 decimal bits)
The last 13 decimal bits represents the sequence within the game, of which domain size will be 10 Trillion. It should be provided by the corresponding game, with the issue method.

## Reserved NFT-ID

Base on the standard of game_id, the minimum game_id should be 0,001, and the id 0,000 should be reserved for expansion.  
Specific extensions can be defined by other dgips.  
