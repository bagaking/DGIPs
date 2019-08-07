---
dgip: 12
title: DGame
author: bagaking <kinghand@foxmail.com>
status: Draft
type: Role
created: 2019-08-07
---

## Abstract

DGame is the basic logical entity of dgcenter, each game has a separate id, validation hash, and control permissions

### Game-ID

|game id|
|--|
|0,000~9,999|

- there are 9999 games, from id `0001` to `9999`, can be created
- the game_id `0000` means the dgcenter itself.

### Create Game

A dgame instance can only be created by the dgcenter `(game_id: 0000)`  
To create a new game, `game_id` and `name` must be provided.  
When game created, a unique and random `hash` will be also generated for the game.  
This `hash` is used to signature massages sent from the game.  
The algorithm are provided at [Signature and Validate](#signature-and-validate) segment.  

e.p.  

```typescript
type DGame = {game_id: number, name: string, hash: string, paused: string}
const game: DGame = await createGame(game_id: number, name: string);
```

### Game Status

#### paused

The status entry `paused` is a string that presents the reason why game paused.  
If the entry `paused` are empty string, means the game is running.  
When game are paused, all external tconv methods **MUST** be unavailable.  
The defualt paused status of a new game **SHOULD** be empty.

### Signature and Validate

#### Signature

1. Sort all keys except "sign" in the message blob from small to large.
2. Build string with format `key=value` for each entry, and combined then with `&`.
3. If the value is an object, stringify it as a JsonObject before building the string for this entry.
4. Attach the game `hash` at the end of the combined string, can then convert the whole string to lower case.
5. Calculate the md5 value *(hex)* of the string generated in step 4.

*e.p.*

```typescript  
const md5 = crypto.createHash("md5");
const keys: any[] = Object.keys(data).filter(s => s !== "sign").sort();
const signStr = keys.reduce((prev, k) =>
          prev + (data[k] !== undefined ? `&${k}=${
                      typeof data[k] === "object"
                      ? JSON.stringify(data[k])
                      : data[k]
                  }` : "")
          , "").substr(1) + hash;
const dist = (signStr).toLowerCase();
const sign = md5.update(dist).digest("hex");
```

#### Validate

1. Using the same algorithm provided in [Signature](#signature) to calculate the sign.
2. Verify the authority based on whether the signs are equal.

### DGame message

dgame message **MUST** have field `game_id` and `sign`.

e.p.

```typescript
interface IDGameMsg {
  game_id: number;
  sign: string;
}
```
