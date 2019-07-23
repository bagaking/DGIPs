---
dgip: 210
title: Fungible Token Bags
author: bagaking <kinghand@foxmail.com>
status: Draft
type: Standards Track
created: 2019-07-23
---

## Simple Summary

A standard for bag_id.

## Abstract

The bag_ids is a special nft_id who represents the ft bag of a user. It occupies part of the reserved nft_id domain.

## Bag_ID

|sign|game_id|sign|blank|dgid|
|--|--|--|--|--|
|1|00,00|9|,000,0|00,000,001~99,999,999|

If a reserve nft_id's game_id is 0,000, and the next decimal bit is 9, this reserve nft_id represents the ft-bag of a dgid.  
Meanwhile, The corresponding 8-decimal-bit dgid will be filled at the low-endian of this nftid, and the whole nft-id can be called bag_id.
Therefore, a bag_id can be defined as a nft_id in the format as follows:

|bag-id-signature|dgid|
|--|--|
|100,009,000,0|00,000,001~99,999,999|
