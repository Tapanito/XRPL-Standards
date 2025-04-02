<pre>    
Title:        <b>XLS Specification Template</b>
Revision:     <b>1</b> (2024-10-18)
Version:      <b></b>

<hr>Authors:    
  <a href="mailto:vtumas@ripple.com">Vytautas Vito Tumas</a>
  
Affiliation:
  <a href="https://ripple.com">Ripple</a>
</pre>

# XLS Specification Template

## _Abstract_

## Index

- [**1. Introduction**](#1-introduction)
  - [**1.1. Overview**](#11-overview)
  - [**1.6. Terminology**](#16-terminology)
  - [**1.7. System Diagram**](#17-system-diagram)
- [**2. Ledger Entries**](#2-ledger-entries)
  - [**2.1. Example Ledger Entry**](#21-loanbroker-ledger-entry)
    - [**2.1.1. Object Identifier**](#211-object-identifier)
    - [**2.1.2. Fields**](#212-fields)
    - [**2.1.4. Ownership**](#214-ownership)
    - [**2.1.5. Reserves**](#215-reserves)
- [**3. Transactions**](#3-transactions)
  - [**3.1. Example Transactions**](#31-loanbroker-transactions)
    - [**3.1.1. Set Transaction**](#311-loanbrokerset)
    - [**3.1.1. Delete Transaction**](#311-loanbrokerset)
- [**4. API **](#4-api)
- [**Appendix**](#appendix)

## 1. Introduction

An XRP Ledger Specification (XLS) provides a detailed description of an XRP Ledger protocol. The specification focuses on the why and the what of a protocol, whereas the implementation focuses on the how.

**TODO: add an overview of what a specification is (a semantically coherent set of objects and transactions)**

### 1.1 Overview

### 1.2 System Diagram

A system diagram provides a birds eye view of the relationship between the objects introduced by the specific protocol.

Below is an example of the Lending Protocol system diagram.

```
+-----------------+                          +-----------------+                       +-----------------+
|    Depositor    |                          |    LoanBroker   |                       |     Borrower    |
|   AccountRoot   |                          |   AccountRoot   |                       |   AccountRoot   |
|-----------------|                          |-----------------|                       |-----------------|
| Owner Directory |                          | Owner Directory | <-----OwnerNode       | Owner Directory | <---------
+-----------------+                          +-----------------+           |           +-----------------+          |
      ^         |                                     |                    |                    |                   |
      |       Reserve                  ____________Reserve____________     |                  Reserve               |
   Account      |                     |                               |    |                    |                   |
      |         V                     V                               V    |                    V                   |
+-----------------+          +-----------------+          +-----------------+          +-----------------+          |
|                 |          |                 |1        N|                 |1        N|                 |          |
|     MPToken     |          |      Vault      |--------->|   LoanBroker    |--------->|       Loan      |-OwnerNode-
|                 |          |                 |          |                 |          |                 |
+-----------------+          +-----------------+          +-----------------+          +-----------------+
         |                            ^                        |    ^                            |
      Issuance                        |                        |    |                            |
         |                         Account                     | Account                         |
         V                            |              -VaultNode-    |                            |
+-----------------+          +-----------------+     |    +-----------------+                    |
|      Share      |          |  Pseudo-Account |     |    |  Pseudo-Account |                    |
| MPTokenIssuance |<--Issuer-|   AccountRoot   |     |    |   AccountRoot   |                    |
|                 |          |-----------------|     |    |-----------------|                    |
+-----------------+          | Owner Directory | <----    | Owner Directory | <- LoanBrokerNode---
                             +-----------------+          +-----------------+
```

[**Return to Index**](#index)

## 2. Ledger Entries

Ledger Entries documents one or more new ledger entry objects introduced or modified by the specification. Each ledger entry must be in it's own numbered section.

### 2.1. Example Ledger Entry

This is an example of a single ledger entry section. This section should provide a purpose of the new ledger entry or changes made to an existing entry.

#### 2.1.1 Object Identifier

Each object on the XRP Ledger must have a unique object identifier. The identifier for a given object within the ledger is calculated based on some object-specific parameters. To ensure that different types of objects have different indices, even if they happen to use the same set of parameters, we use "tagged hashing" by adding a type-specific prefix called the `key space`. A list of existing `key spaces` can be foundon the `rippled` [Github Repository](https://github.com/XRPLF/rippled/blob/develop/src/libxrpl/protocol/Indexes.cpp). In the specification, the key space is given as a `hex` representation of the 8-bit ASCII code of the character.

The key of the `Example` object is the result of [`SHA512-Half`](https://xrpl.org/docs/references/protocol/data-types/basic-data-types/#hashes) of the following values concatenated in order:

- The `Example` space key `0x0045` (Upper-case `E`)
- The `AccountID`(https://xrpl.org/docs/references/protocol/binary-format/#accountid-fields) of the account submitting the `ExampleSet` transaction.

#### 2.1.2 Fields

This subsection lists the object fields, indicating whether a field is modifiable, optional, it's JSON type, Internal Type and the Default Value.

- `Modifiable`: The column takes one of the three values: `Yes`, `No` and `N/A`

  - `Yes` - the field is modifiable
  - `No` - the field is not modifiable, but this condition may change in the future
  - `N/A` - this field is not directly modifiable by the user. This value is reserved to fields that are only modified due to some protocol condition

- `Required`: The column takes one of the three values: `Yes`, `No`, `Conditional`

  - `Yes` - The field is required
  - `No` - The field is optional
  - `Conditional` - The field is required under certain circumstances, these circumstances must be described in Fields subsection

- `Internal Type`: The internal type of the field. Refer to [rippled](https://github.com/XRPLF/rippled/blob/develop/include/xrpl/protocol/detail/sfields.macro) for all internal types. Internal types must be in all capital letters

- `Default Value`: The default value of the field when the ledger entry is created

  - `N/A` - The field does not have a default value

- `Description` is a brief description of the field. In case further details are needed, these must be written in the subsection bellow.
  The `Example` object has the following fields:

| Field Name          | Modifiable? | Required? | Internal Type | Default Value | Description                                                                           |
| ------------------- | :---------: | :-------: | :-----------: | :-----------: | :------------------------------------------------------------------------------------ |
| `LedgerEntryType`   |    `N/A`    |   `Yes`   |   `UINT16`    |      53       | Unique Ledger object type.                                                            |
| `LedgerIndex`       |    `N/A`    |   `Yes`   |   `UINT16`    |     `N/A`     | Ledger object identifier.                                                             |
| `Flags`             |    `Yes`    |   `Yes`   |   `UINT32`    |       0       | Ledger object flags.                                                                  |
| `PreviousTxnID`     |    `N/A`    |   `Yes`   |   `HASH256`   |     `N/A`     | The ID of the transaction that last modified this object.                             |
| `PreviousTxnLgrSeq` |    `N/A`    |   `Yes`   |   `UINT32`    |     `N/A`     | The sequence of the ledger containing the transaction that last modified this object. |
| `Sequence`          |    `N/A`    |   `Yes`   |   `UINT32`    |     `N/A`     | The transaction sequence number that created the object.                              |
| `OwnerNode`         |    `N/A`    |   `Yes`   |   `UINT64`    |     `N/A`     | Identifies the page where this item is referenced in the owner's directory.           |
| `Account`           |    `No`     |   `Yes`   |   `ACCOUNT`   |     `N/A`     | The address of the Account that owns the object.                                      |
| `ExampleField`      |             |           |               |               |                                                                                       |

The field table MUST include default fields, as well as fields unique to the ledger entry.

Following a table of fields is an optional list of fields that require further details, that are too long for the `Description` column.

##### 2.1.2.1 `LedgerEntryType`

This is a mandatory, non-modifiable field that identifies the type of the ledger entry. A list of existing ledger entries and their type IDs can be found in `include/xrpl/protocol/ledger_entries.macro`.

##### 2.1.2.2 `Flags`

This subsection lists the flags associated with the ledger entry. All flags must be named as `lsf<LedgerEntry>Adjective`, where

- `lsf` is a mandatory prefix identifying that this is a ledger entry flag
- `<object>` identifies the ledger entry with which the flag is associated, e.g. `Example`.
- `Adjective` uniquely identifies the purpose of the flag. The name of the flag must be as short as necessary to describe the property the flag controls. For example `IsPrivate`.

Ledger Entry flags MUST be presented in a table with the following columns:

- `Flag Name` - The name of the flag constructed the aformentioned rules. For example, `lsfExampleIsPublic`
- `Flag Value` - The hexadecimal value of the flag, each flag must be a multiple of two.
- `Description` - The description of the intended behaviour of the flag.

**TODO: comment how flag values must be multiples of two**

| Flag Name               | Flag Value   | Description                                                                          |
| ----------------------- | ------------ | ------------------------------------------------------------------------------------ |
| `lsfExampleIsPublic`    | `0x00010000` | If set, indicates that the `Example` object can be used by anyone.                   |
| `lsfExampleCanLock`     | `0x00020000` | If set, indicates that the `Example` object can be locked by the owner.              |
| `lsfExampleCanTransfer` | `0x00040000` | If set, indicates that the `Example` ownership can be transfered to another account. |

##### 2.1.2.3 `ExampleField`

A unique field to the `Example` ledger entry. A rule of thumb is to re-use as much as possible already existing fields.

#### 2.1.3 Ownership

All XRP Ledger objects must have an owner. The owner is an `AccountRoot` object, and the ownership relationship is captured by the `OnwerDirectory` ledger entry. This subsection captures in which `AccountRoot` objects the ledger entry is registered. A single ledger entry may be captured in one or more unique `OwnerDirectory` objects.

#### 2.1.4 Reserves

To discourage users from creating limitless number of objects, eacher ledger entry must reserve at least one owner reserve fee.

#### 2.1.5 Deletion

This subsection captures the constraints for deleting the ledger entry, and whether the ledger entry is a blocker for the deletion of the Owner `AccountRoot`.

#### 2.1.6 `pseudo-account`

In case the newly introduced ledger entry has to hold other assets (XRP, IOU or MPT) there must be a `pseudo-account` associated with the ledger entry. A pseudo-account is a unique account created programatically that cannot submit transaction, send or receive funds directly or have a key pair associated with it. The sole purpose of the `pseudo-account` is to create tokens on behalf of the object or hold assets on behalf of an object. For further details about pseudo-accounts, refer to `XLS-64` specification.

[**Return to Index**](#index)

## 3. Transactions

### 3.1. `Example` Transactions

In this section we specify the transactions associated with the `Example` ledger entry. Transaction names must be named as `<LedgerEntry>Verb`, where:

- `<LedgerEntry>` is the name of the ledger entry the transaction is associated with, e.g. `Example`.
- `Verb` that identifies the functionality of this transaction.

For example, `ExampleSet`, `ExampleDelete`, `ExampleTransfer`.

Most specification will have at least the following two transactions:

- `<Object>Set` - a dual purpose transaction to create or update an already existing object. In case the logic of creating or updating the transaction is complex, it may be split into `<Object>Create` and `<Object>Set` to create and update the object.
- `<Object>Delete` - a transaction to delete the object.

Transactions is where the business

#### 3.1.1 `ExampleCreate`

The transaction creates a new `Example` object or updates an existing one.

| Field Name        |     Required?      | JSON Type | Internal Type | Default Value | Description                                   |
| ----------------- | :----------------: | :-------: | :-----------: | :-----------: | :-------------------------------------------- |
| `TransactionType` | :heavy_check_mark: | `string`  |   `UINT16`    |   **TODO**    | The transaction type.                         |
| `Flags`           |                    | `string`  |   `UINT32`    |       0       | Specifies the flags for the Lending Protocol. |
| inclusive.        |

##### 3.1.1.1 Failure Conditions

##### 3.1.1.2 State Changes

##### 3.1.1.3 Invariants

#### 3.1.2 `ExampleSet`

#### 3.1.2 `ExampleDelete`

[**Return to Index**](#index)

## 4. API

# Appendix

## A-1 F.A.Q

### A.1.1 Q1:
