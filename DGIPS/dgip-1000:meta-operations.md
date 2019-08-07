---
dgip: 1000
title: Meta Operations
author: bagaking <kinghand@foxmail.com>
status: Draft
type: Kernal
created: 2019-08-08
---

## Abstract

Meta operation, which represents atomic operations on a single NFT, It has the following features.

1. It **MUST** change only one nft at a time, thus it **MUST** has an `nft_id` filed to specify the nft id.
2. It **MUST** has both rollback and execution operations.
3. It **MAY** has a `validate` filed to constraining provided values.

Moreover, when a meta be called as a `basic-meta`, it **MUST** has the following features.

1. It **MUST** change only one field or one bag_entry of the nft at a time. (except create)
2. The reduce procedure of this meta **MUST NOT** have any side effects.

Otherwise, the meta are called `complex-meta`

## Meta operations

There are **4** clearly defined `basic-meta`:

1. Create:

    Create a new nft with given nft_id.

2. Delete:

    Delete a new nft of given nft_id.

3. Handover:

    For a given nft_id, change it's owner from on dgid to another dgid.

4. Update:

    Add or subtract a fixed ft value for a given nft_id.

    Has `validate` field.

There are **1** cleary defined `complex-meta`:

1. Factor:

    For a given nft_id, subtract a certain percentage of the value relative to the nft, or increase a certain percentage of the value relative to the total changed value.

    Has `validate` field.

    **side effects**  
    To support rollback, factor must have a auxiliary structure, to record how much ft are added or subtracted. Therefore, the factor meta before and after procedure executed are different.

## Implementation

1. Type enumeration

    ```typescript
    enum MetaType{
        Create = 1,
        Delete = 2 ,
        Handover = 3,
        Update = 4,
        Factor = 5
    }
    ```

2. Interface base

    Based on the definition of meta, all meta **SHOULD** have fields `type` and `nft_id`, and the field `dgid` **MAY** be provided to make the meta request more reliable.

      ```typescript
      interface IMetaBase {
          t: MetaType;
          dgid: DGID;
          nft_id: NftID;
      }
      ```

3. Interfaces of `basic-meta`.  
    1. create

        ```typescript
        interface IMetaCreate extends IMetaBase {
            t: MetaType.Create;
        }
        ```

    2. delete

        ```typescript
        interface IMetaDelete extends IMetaBase {
            t: MetaType.Delete;
        }
        ```

    3. handover

        ```typescript
        interface IMetaHandover extends IMetaBase {
            t: MetaType.Handover;
            to: DGID;
        }
        ```

    4. update

        There are 3 meaningful validate option of this meta: `NotZero`, `MustPositive`, `MustNegative`.

        ```typescript
        enum ValueValidateOption {
            Whatever = 0,
            NotZero = 1,
            MustPositive = 2,
            MustNegative = 3
        }

        interface IMetaUpdate extends IMetaBase {
            t: MetaType.Update;
            validate?: ValueValidateOption;
            sym: string;
            amount: number;
        }
        ```

4. interfaces of `complex-meta`

    1. factor

        When the `sym` is the wildcard `*`, all of this nft's ft will participate in the operation.

        There are 2 meaningful validate option of this meta: `MustPositive`, `MustNegative`.

        The field `rec_amount` **SHOULD** be generated and be persisted in the prepare procedure. Because the recovery of `rec_amount` is a necessary condition for commit and abort to be executed correctly.

        ```typescript
        export enum FactorValidateOption {
            MustPositive = 2,
            MustNegative = 3
        }

        export interface IMetaFactor extends IMetaBase {
            t: MetaType.Factor;
            sym: string | "*";
            factor: number;
            validate?: FactorValidateOption;
            rec_amount?: {
                [sym: string]: BigNumber
            };
        }
        ```
