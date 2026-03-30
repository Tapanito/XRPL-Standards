<pre>
Title:        <b>XLS Specification Template</b>
Revision:     <b>2</b> (2025-05-13) StandardVersion: <b>0.1 (Draft)</b> <hr>Authors:
  <a href="mailto:vtumas@ripple.com">Vytautas Vito Tumas</a>
  
Affiliation: <a href="https://ripple.com">Ripple</a>
</pre>

# XLS Specification Template

## _Abstract_

## Index

- [**1. Introduction**](#1-introduction)
  - [**1.1. Motivation and Overview**](#11-motivation-and-overview)
  - [**1.2. Terminology**](#12-terminology)
  - [**1.3. System Diagram**](#13-system-diagram)
- [**2. Ledger Entries**](#2-ledger-entries)
  - [**2.1. `Example` Ledger Entry**](#21-example-ledger-entry)
    - [**2.1.1. Object Identifier**](#211-object-identifier)
    - [**2.1.2. Fields**](#212-fields)
    - [**2.1.3. Ownership**](#213-ownership)
    - [**2.1.4. Reserves**](#214-reserves)
    - [**2.1.5. Deletion**](#215-deletion)
    - [**2.1.6. `pseudo-account`**](#216-pseudo-account)
- [**3. Transactions**](#3-transactions)
  - [**3.1. `Example` Transactions**](#31-example-transactions)
    - [**3.1.1. `ExampleSet` Transaction**](#311-exampleset-transaction)
    - [**3.1.2. `ExampleDelete` Transaction**](#312-exampledelete-transaction)
- [**4. API**](#4-api)
- [**Appendix**](#appendix)
  - [**A-1. F.A.Q**](#a-1-faq)

## 1. Introduction

An XRP Ledger Standard (XLS) describes a proposed change or addition to the XRP Ledger protocol or its ecosystem. The specification focuses on the _why_ (motivation) and the _what_ (technical details) of a protocol, whereas the implementation focuses on the _how_.

**TODO: Provide a brief overview of what _this specific hypothetical standard_ (that this template would be used for) aims to achieve. It should introduce the core concept, like "This specification proposes a new mechanism for decentralised lending..." This involves defining a semantically coherent set of new or modified ledger objects and transactions.**

### 1.1. Motivation and Overview

### 1.2. Terminology

The following terms are used in this document:

- **Example Term:** A definition for a term specific to this specification.

### 1.3. System Diagram

A system diagram provides a bird's-eye view of the relationship between the objects and actors introduced or affected by this specific protocol.

Below is an example of a Lending Protocol system diagram.

[**Return to Index**](#index)

## TODO. Serialisable Fields (Optional Section)

This optional section lists any new serialisable fields introduced by this XRP Ledger specification. Serialisable Fields should not be introduced lightly, as they require changes to the core protocol and impact all `rippled` servers. Existing fields, that can be found in [`rippled`'s `sfields.h` file](https://github.com/XRPLF/rippled/blob/develop/src/ripple/protocol/SField.h) (see `SField.cpp` for full list & `sfields.macro`), should cover the majority of use-cases.

**Important:** Introducing new serialisable fields is a significant change that requires strong justification, thorough review by the XRP Ledger core developers and careful consideration.

However, if additional fields must be introduced, this must be done in this section. For each new field, provide:

- `Field Name`: The proposed name (e.g., `ExampleSpecificField`).
- `Field Code`: The proposed unique numerical code.
- `Type`: The data type (e.g., `STI_UINT32`, `STI_AMOUNT`).
- `Flags`: Serialisation flags (e.g., `SField::sfDefault`, `SField::sfOptional`).
- `Justification`: Why this field is necessary and cannot be achieved with existing fields or combinations thereof.

## 2. Ledger Entries

Ledger Entries documents one or more new ledger entry objects introduced or modified by the specification. Each ledger entry must be in its own numbered section.

### 2.1. `Example` Ledger Entry

This is an example of a single ledger entry section. This section should provide the purpose of the new ledger entry or changes made to an existing entry.

The `Example` ledger entry is an on-chain object capturing the number of times an `ExampleSet` transaction (in update mode) was successfully called for that object if it is not locked.

#### 2.1.1 Object Identifier

Each object on the XRP Ledger must have a unique object identifier (ID or key). The ID for a given object within the ledger is calculated using "tagged hashing". This involves hashing some object-specific parameters along with a type-specific prefix called the `key space` (a 16-bit value) to ensure that different objects have different IDs, even if they use the same set of parameters. A list of existing `key spaces` can be found in the `rippled` [Github Repository (Indexes.cpp)](https://github.com/XRPLF/rippled/blob/develop/src/libxrpl/protocol/Indexes.cpp). The key space is given as a `hex` representation in the specification.

The key of the `Example` object is the result of [`SHA512-Half`](https://xrpl.org/docs/references/protocol/data-types/basic-data-types/#hashes) of the following values concatenated in order:

- The `Example` space key `0x0045` (representing ASCII for 'E')
- The `AccountID`(<https://xrpl.org/docs/references/protocol/binary-format/#accountid-fields>) of the account that created the `Example` object.
- The `Sequence` number of the account that created the `Example` object.

#### 2.1.2 Fields

This subsection lists the object fields, indicating whether a field is constant or optional, its JSON type (for reference, actual storage is binary), Internal Type, and Default Value.

- `Field Name`: The column indicates the field's name. Fields follow the `PascalCase` naming convention. For existing field names (and their associated types), please refer to [`sfields.marco` in `rippled`](https://github.com/XRPLF/rippled/blob/develop/include/xrpl/protocol/detail/sfields.macro). A rule of thumb is to reuse already existing fields whenever possible and sensible.

- `Constant`: Indicates whether the Ledger Entry field is mutable after creation.
  - `Yes` - the field is constant.
  - `No` - the field is not constant.

- `Required`: Indicates whether the field is required for the object to be valid.
  - `Yes` - The field is required.
  - `No` - The field is optional.
  - `Conditional` - The field is required under certain circumstances, described in the subsection following the fields table.

- `Internal Type`: The internal data type of the field. Refer to `rippled` for all internal types (e.g., `UINT16`, `ACCOUNT`, `HASH256`). Internal types must be in all capital letters. Please refer to [`sfields.marco` in `rippled`](https://github.com/XRPLF/rippled/blob/develop/include/xrpl/protocol/detail/sfields.macro) for a list of existing internal types.
- `Default Value`: The field's default value when the ledger entry is created.
  - `N/A` - The field does not have a default value or is always required.
- `Description`: A brief description of the field. Further details can be written in subsections.

The `Example` object has the following fields:

| Field Name          | Constant? | Required? | Internal Type | Default Value | Description                                                                           |
| ------------------- | :-------: | :-------: | :-----------: | :-----------: | :------------------------------------------------------------------------------------ |
| `LedgerEntryType` |    `Yes` |   `Yes` |   `UINT16` | 55 | Unique Ledger object type identifier. Must be a new, unused value.                 |
| `Flags` |    `No` |   `Yes` |   `UINT32` |       0       | Ledger object flags.                                                                  |
| `PreviousTxnID` |    `No` |   `Yes` |   `HASH256` |     `N/A` | The ID of the transaction that last modified this object.                             |
| `PreviousTxnLgrSeq` |    `No` |   `Yes` |   `UINT32` |     `N/A` | The ledger sequence containing the transaction that last modified this object. |
| `OwnerNode` |    `Yes` |   `Yes` |   `UINT64` |     `N/A` | Identifies the page where this item is referenced in the owner's directory.           |
| `Account` |    `Yes` |   `Yes` |   `ACCOUNT` |     `N/A` | The address of the Account that owns this `Example` object.                           |
| `ExampleCounter` |    `No` |   `No` |   `UINT32` |       0       | An example counter field, incremented by `ExampleSet` if not locked.                  |

The field table MUST include all standard ledger entry fields (like `LedgerEntryType`, `Flags`, `PreviousTxnID`, `PreviousTxnLgrSeq`, `OwnerNode`) as well as fields unique to the ledger entry. The `Account` field is typical for objects owned by a single account.

Following a table of fields is an optional list of fields that require further details that are too long for the `Description` column.

##### 2.1.2.1 `LedgerEntryType`

This is a mandatory, non-modifiable field that identifies the type of the ledger entry. A list of existing ledger entries and their type IDs can be found in [`ledger_entries.macro` in `rippled`](https://github.com/XRPLF/rippled/blob/develop/include/xrpl/protocol/detail/ledger_entries.macro). A new, unique `LedgerEntryType` value (e.g., 53) must be chosen for a new object type.

##### 2.1.2.2 `Flags`

This subsection lists the flags associated with the ledger entry. All flags must be named as `lsf<LedgerEntryName><Adjective>`, where:

- `lsf` is a mandatory prefix that identifies this as a ledger entry flag.
- `<LedgerEntryName>` identifies the ledger entry associated with the flag (e.g., `Example`).
- `<Adjective>` uniquely identifies the purpose of the flag (e.g., `IsLocked`).

Ledger Entry flags MUST be presented in a table with the following columns:

| Flag Name            | Flag Value   | Description                                                                                |
| -------------------- | ------------ | ------------------------------------------------------------------------------------------ |
| `lsfExampleIsLocked` | `0x00010000` | If set, indicates that the `Example` object counter is locked and will not be incremented. |

This section should also indicate if any flags are mutually exclusive.

##### 2.1.2.3 `ExampleCounter`

A unique field to the `Example` ledger entry. This field stores a count modified under specific conditions by the `ExampleSet` transaction.

#### 2.1.3 Ownership

All XRP Ledger objects must have an owner. The owner is an `AccountRoot` object, and the ownership relationship is typically established by adding the object's ID to an `OwnerDirectory` ledger entry associated with the owner's account. This subsection captures which `AccountRoot` object's `OwnerDirectory` the ledger entry is registered. A single ledger entry may be linked from one or more unique `DirectoryNode` pages, usually under one `OwnerDirectory`.

The `Example` ledger entry is tracked in the `OwnerDirectory` of the `Example.Account` `AccountRoot` object.

#### 2.1.4 Reserves

Creating ledger entries typically requires an increase in the owner's XRP reserve to discourage ledger bloat and account for the cost of storage. Each new ledger entry directly owned by an account typically increments the owner reserve by one unit (currently 2 XRP, as of last check, but subject to change by Fee Voting). This section should confirm whether this standard behaviour applies or specify any deviations.

#### 2.1.5 Deletion

This subsection captures the conditions under which the ledger entry can be deleted from the ledger. It should specify:

- What transaction(s) can delete the object.
- Any prerequisite conditions for deletion (e.g., object state, zero balances, no linked objects).
- Is the ledger entry a "blocker" for deleting its owner `AccountRoot` (i.e., must it be deleted before the account can be deleted?).

#### 2.1.6 `pseudo-account`

A' pseudo-account' might be associated if the newly introduced ledger entry needs to hold assets (XRP, IOUs or MPTs) or issue tokens (e.g., MPTs). A pseudo-account is a programmatically derived `Account` that cannot submit transactions, send or receive funds directly via standard payments, or have a key pair. Its sole purpose is to act as an issuer or holder of assets on behalf of the ledger object. For further details about pseudo-accounts, refer to `XLS-64d` (or the relevant accepted standard). This section should specify if a pseudo-account is used, how its `AccountID` is derived, and its purpose.

[**Return to Index**](#index)

## 3. Transactions

This section details new or modified transactions introduced by the specification.

### 3.1. `Example` Transactions

This section specifies the transactions associated with the `Example` ledger entry. Transaction names should be descriptive, often `<LedgerEntryName><Verb>`, where:

- `<LedgerEntryName>` is the name of the ledger entry the transaction primarily interacts with (e.g., `Example`).
- `<Verb>` identifies the action of this transaction (e.g., `Set`, `Delete`, `Invoke`).

For example, `ExampleSet` and `ExampleDelete`.

Most specifications introducing new objects will have at least:

- `<Object>Set` (or `<Object>Create` and `<Object>Update` if distinct): A transaction to create or update an object.
- `<Object>Delete`: A transaction to delete the object.

#### 3.1.1. `ExampleSet` Transaction

This section outlines transaction fields, provides details about them, defines special logic, failure conditions, state changes, and invariants.

The `ExampleSet` transaction creates a new `Example` object or updates an existing one. If updating an existing `Example` object is not locked, the transaction increments `ExampleCounter` by `1`.

##### 3.1.1.1. Fields

Transaction field outline table:

- `Field Name`: The column indicates the field's name. Fields follow the `PascalCase` naming convention. For existing field names (and their associated types), please refer to [`sfields.marco` in `rippled`](https://github.com/XRPLF/rippled/blob/develop/include/xrpl/protocol/detail/sfields.macro). A rule of thumb is to reuse already existing fields whenever possible and sensible.
- `Required?`: `Yes`, `No`, or `Conditional` (with conditions explained).
- `JSON Type`: For JSON submission (e.g., `string`, `number`, `object`, `array`).
- `Internal Type`: The internal data type of the field. Refer to `rippled` for all internal types (e.g., `UINT16`, `ACCOUNT`, `HASH256`). Internal types must be in all capital letters. Please refer to [`sfields.marco` in `rippled`](https://github.com/XRPLF/rippled/blob/develop/include/xrpl/protocol/detail/sfields.macro) for a list of existing internal types.
- `Default Value`: If any. `N/A` if none or always required.
- `Description`: Purpose, conditions, etc.

This table should list fields specific to this transaction. [Common transaction fields](https://xrpl.org/docs/references/protocol/transactions/common-fields) (e.g., `Account`, `TransactionType`, `Fee`, `Sequence`, `Flags` (common transaction flags), `SigningPubKey`, `TxnSignature`) are assumed unless their usage has special implications for this transaction type.

| Field Name        | Required?   | JSON Type | Internal Type | Default Value | Description                                                                |
| ----------------- | :---------: | :-------: | :-----------: | :-----------: | :------------------------------------------------------------------------- |
| `TransactionType` |    `Yes` | `string` |   `UINT16` | 100 | The transaction type identifier. Must be a new, unused value.         |
| `Flags` |    `No` | `number` |   `UINT32` |       0       | Transaction-specific flags (distinct from common transaction flags).       |
| `ExampleID` | `Conditional`| `string` |   `HASH256` |     `N/A` | The ID of an existing `Example` object to update. If omitted, it creates a new `Example` object. |

###### 3.1.1.1.1. `TransactionType`

Each new transaction type must have a unique numerical identifier. The new `TransactionType` value must be chosen carefully, typically one greater than the current highest value for custom transactions or as assigned by a coordinated process. Refer to [`transactions.macro` in `rippled`](https://github.com/XRPLF/rippled/blob/develop/include/xrpl/protocol/detail/transactions.macro). For this example, `ttEXAMPLE_SET = 100`.

###### 3.1.1.1.2. `Flags` (Transaction-Specific)

This subsection lists the transaction-specific flags (distinct from common transaction flags). These flags modify the transaction's behaviour. All transaction-specific flags must be named as `tf<TransactionName><Verb/Adjective>`, where:

- `tf` is a mandatory prefix.
- `<TransactionName>` identifies the transaction (e.g., `ExampleSet`).
- `<Verb/Adjective>` uniquely identifies the action/property (e.g., `Lock`).

Transaction flags MUST be presented in a table:

| Flag Name             | Flag Value   | Description                                                                                                                               |
| --------------------- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `tfExampleSetLock` | `0x00010000` | If set when updating an existing object, sets the `lsfExampleIsLocked` flag on the `Example` object. It fails if an object that is already locked is created. |
| `tfExampleSetUnlock` | `0x00020000` | If set when updating an existing, locked object, clears the `lsfExampleIsLocked` flag. Fails if creating an object already unlocked.    |

`tfExampleSetLock` and `tfExampleSetUnlock` are mutually exclusive.

##### 3.1.1.2. Failure Conditions

This section describes the conditions under which the transaction will fail. This must be an exhaustive, descriptive list. Each condition should ideally map to a specific error code. The list should be indexed for easy reference.

When using the same transaction to create and update an object, the expected behaviour is identified by the presence or absence of the object identifier (e.g., `tx.ExampleID`).

In case of a transaction failure, an XRP Ledger server returns an error code indicating the outcome. These codes are crucial for clients to understand why a transaction was not successful. The primary categories are:

- `tec` (Transaction Engine - Claimed Fee): The transaction was successfully submitted and included in a validated ledger, but its execution failed due to a specific rule or condition defined by the protocol (e.g., insufficient balance for an operation, no permission, or a logic failure particular to the transaction type). The transaction fee is claimed, and the account's sequence number is consumed. This result is final once the ledger is validated.
- `tef` (Failure): The server determined that the transaction cannot be successfully applied. This can be due to various reasons, such as the transaction already being applied (tefALREADY), the account sequence number being too old (tefPAST_SEQ), or other conditions making it permanently invalid from that server's perspective. Generally, the transaction is not relayed further, no fee is claimed from the account, and the sequence number is not consumed for this attempt.
`tel` (Local Error): The rippled server encountered an error due to its local conditions, such as high load or temporary unavailability of resources. The transaction was not processed. Submitting the transaction to a different server or retrying later may result in a different outcome. No fee is claimed, and the sequence number is not consumed.
- `tem` (Malformed Transaction): The transaction was rejected because it was improperly formed according to the protocol rules. This could be due to incorrect syntax, conflicting options, a bad signature, missing required fields, or other structural issues. The transaction is not relayed, no fee is claimed, and the sequence number is not consumed.
- `ter` (Retry): This provisional code indicates that the transaction could not be applied to the current open ledger, but it might succeed if applied to a future ledger. Clients may choose to retry. These codes are typically seen before a transaction is finalised in a validated ledger.

A comprehensive list of existing result codes can be found in the [`TER.h` file in the rippled repository](https://github.com/XRPLF/rippled/blob/develop/include/xrpl/protocol/TER.h).

When defining failure conditions for a new transaction type in an XLS:

 Reuse existing codes whenever an existing code accurately describes the failure condition. This helps maintain consistency and avoids unnecessary proliferation of codes.
 If the new transaction logic introduces novel failure reasons not adequately covered by existing generic codes, a new tec code SHOULD be proposed. This new code must be clearly defined and justified and would eventually be added to TER.h if the XLS is adopted. XLS authors will primarily define tec codes for their specific transaction logic failures.

1. If `tx.ExampleID` IS provided (updating an existing object):
    1. The `Example` ledger entry identified by `tx.ExampleID` does not exist (`tecNO_ENTRY`).
    2. The transaction submitter (`tx.Account`) is not the owner of the `Example` object (`Example(tx.ExampleID).Account`) (`tecNO_PERMISSION`).
    3. The `tfExampleSetLock` flag is set, but the `Example(tx.ExampleID)` object already has `lsfExampleIsLocked` set (`temINVALID_FLAG`).
    4. The `tfExampleSetUnlock` flag is set, but the `Example(tx.ExampleID)` object does not have `lsfExampleIsLocked` set (`temINVALID_FLAG`).
    5. The transaction attempts to modify any constant fields of the `Example` object (e.g., `Account`) (`temINVALID` or a more specific error).
    6. Both `tfExampleSetLock` and `tfExampleSetUnlock` flags are set (`temINVALID`).
2. If `tx.ExampleID` IS NOT provided (creating a new ledger object):
    1. The `tfExampleSetLock` or `tfExampleSetUnlock` flag is set (these flags are only valid for updates) (`temINVALID_FLAG`).
    2. The submitting account does not have sufficient XRP to meet the increased owner reserve (`tecINSUFFICIENT_RESERVE`).

##### 3.1.1.3. State Changes

This section describes the changes made to the ledger state if the transaction executes successfully. It should omit default state changes common to all transactions (e.g., fee processing, sequence number increment, setting `PreviousTxnID`/`PreviousTxnLgrSeq` on modified objects). Indexed for clarity.

A successfully applied transaction must return a `tesSUCCESS` code.

1. If `tx.ExampleID` IS provided (updating an existing object):
    1. Let `example_obj` be the `Example` ledger entry identified by `tx.ExampleID`.
    2. If `tx.Flags` includes `tfExampleSetLock`:
        1. Set `example_obj.Flags = example_obj.Flags | lsfExampleIsLocked`.
    3. Else if `tx.Flags` includes `tfExampleSetUnlock`:
        1. Set `example_obj.Flags = example_obj.Flags & ~lsfExampleIsLocked`.
    4. If `example_obj.Flags` does NOT include `lsfExampleIsLocked` (i.e., it's unlocked after any flag changes):
        1. Increment `example_obj.ExampleCounter` by `1`.
2. If `tx.ExampleID` IS NOT provided (creating a new ledger object):
    1. Create a new `Example` ledger entry (`new_example_obj`).
    2. Add `new_example_obj.ID` to the `OwnerDirectory` of `tx.Account`.
    3. Update `tx.Account.OwnerCount` and check reserves.

##### 3.1.1.4. Invariants

Invariants are logical statements that MUST be true before and after the execution of a transaction (if successful). They help ensure the transaction does not lead to an invalid or inconsistent ledger state. Use `<object>` for the state before and `<object>'` for the state after.

1. If an `Example` object existed (`Example`) and was updated to `Example'`:
    1. `Example.Account == Example'.Account` (Owner does not change).
    2. If `Example.Flags` had `lsfExampleIsLocked` set AND `Example'.Flags` has `lsfExampleIsLocked` set (i.e., it remained locked, possibly due to `tfExampleSetLock` on an already locked object which would fail, or no unlock flag), then `Example.ExampleCounter == Example'.ExampleCounter`.
    3. If `Example.Flags` did NOT have `lsfExampleIsLocked` set AND `Example'.Flags` has `lsfExampleIsLocked` set (i.e., it became locked via `tfExampleSetLock`), then `Example'.ExampleCounter == Example.ExampleCounter + 1`.
    4. If `Example.Flags` had `lsfExampleIsLocked` set AND `Example'.Flags` does NOT have `lsfExampleIsLocked` set (i.e., it became unlocked via `tfExampleSetUnlock`), then `Example'.ExampleCounter == Example.ExampleCounter + 1`.
    5. If `Example.Flags` did NOT have `lsfExampleIsLocked` set AND `Example'.Flags` does NOT have `lsfExampleIsLocked` set (i.e., it remained unlocked), then `Example'.ExampleCounter == Example.ExampleCounter + 1`.
2. If a new `Example'` object was created:
    1. `Example'.Account == tx.Account`.
    2. `Example'.Flags` does not include `lsfExampleIsLocked`.
    3. `Example'.ExampleCounter` is initialised (e.g., to 0).

##### 3.1.1.5. Example JSON

Provide JSON examples for transaction submission.

###### 3.1.1.5.1. Create Example Object

```json
{
    "TransactionType": "ExampleSet",
    "Account": "r...", // Submitter's account
    // No ExampleID indicates creation
    // Other common fields: Fee, Sequence, SigningPubKey, TxnSignature
}
````

###### 3.1.1.5.2. Update Example Object (Locking it)

```json
{
    "TransactionType": "ExampleSet",
    "Account": "r...", // Owner's account
    "ExampleID": "...", // HASH256 ID of the Example object to update
    "Flags": 65536, // tfExampleSetLock (0x00010000 is 65536 in decimal)"
    // Other common fields
}
```

#### 3.1.2. `ExampleDelete` Transaction

This transaction deletes an existing `Example` ledger entry.

[**Return to Index**](#index)

## 4. API

This section describes any new or modified API endpoints for `rippled` (JSON-RPC or WebSocket) required to support the features introduced in this specification.

## Appendix

### A-1. F.A.Q

An optional section for Frequently Asked Questions regarding this specification.

#### A.1.1 Q1

**Q:** What happens if...?
**A:** The behaviour is...

```
```
