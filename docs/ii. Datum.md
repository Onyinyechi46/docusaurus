[[module Plutus.V1.Ledger.Tx]]

```
-- | 'Datum' is a wrapper around 'Data' values which are used as data in transaction outputs.
newtype Datum = Datum { getDatum :: BuiltinData  }
  deriving stock (Generic, Haskell.Show)
  deriving newtype (Haskell.Eq, Haskell.Ord, Eq, ToData, FromData, UnsafeFromData, Pretty)
  deriving anyclass (NFData)
```

---

# 📦 Plutus Tutorial: Understanding `Datum` in Cardano Smart Contracts

---

## 📘 Introduction

In Cardano's Extended UTXO (EUTXO) model, every transaction output can carry **associated data**. This data, called a **Datum**, allows smart contracts to store state or information with each UTXO.

In Plutus, the `Datum` type is used to wrap this data in a consistent and structured way. In this tutorial, we’ll explore the `Datum` type, its structure, and how it plays a key role in Plutus smart contracts.

---

## 🧾 The Code Definition

```haskell
-- | 'Datum' is a wrapper around 'Data' values which are used as data in transaction outputs.
newtype Datum = Datum { getDatum :: BuiltinData  }
  deriving stock (Generic, Haskell.Show)
  deriving newtype (Haskell.Eq, Haskell.Ord, Eq, ToData, FromData, UnsafeFromData, Pretty)
  deriving anyclass (NFData)
```

---

## 🔍 Understanding the Components

|Element|Description|
|---|---|
|`Datum`|A wrapper for `BuiltinData`, used to store on-chain data in transaction outputs.|
|`getDatum`|A function to **extract** the wrapped `BuiltinData` from a `Datum`.|
|`newtype`|A Haskell construct that wraps a single value — in this case, `BuiltinData`.|
|`ToData`, `FromData`|Enable serialization and deserialization of the data.|
|`Eq`, `Ord`|Allow comparing two `Datum` values.|
|`NFData`|Ensures that `Datum` can be evaluated strictly — useful for performance.|
|`Pretty`|Allows nicely formatted output for debugging or display.|

---

## 📥 Inputs and Outputs

Since `Datum` is a `newtype`, it’s not a function in itself. But we can still explain how it works in terms of inputs and outputs:

|Operation|Input|Output|
|---|---|---|
|`Datum` constructor|`BuiltinData`|`Datum`|
|`getDatum` accessor|`Datum`|`BuiltinData`|

---

## 🧠 Simple Explanation

Think of a `Datum` as a **labelled box** that holds some information (`BuiltinData`) alongside a UTXO. When a validator checks if someone can spend that UTXO, it can also inspect the `Datum` inside the box.

---

## 🛠️ Plutus Code Example

Let’s look at how a `Datum` is created and accessed in Plutus.

### ✅ Creating a `Datum`

```haskell
import PlutusTx
import Plutus.V1.Ledger.Api (Datum(..))

-- Wrap an integer as Datum
myDatum :: Datum
myDatum = Datum (toBuiltinData (42 :: Integer))
```

### 🔍 Accessing the underlying BuiltinData

```haskell
getRawData :: BuiltinData
getRawData = getDatum myDatum
```

### 📤 Using a Datum in a validator (simplified)

```haskell
{-# INLINABLE myValidator #-}
myValidator :: Datum -> Redeemer -> ScriptContext -> Bool
myValidator datum _ _ = case fromBuiltinData (getDatum datum) of
    Just (n :: Integer) -> n == 42
    Nothing             -> False
```

---

## 🖼️ Cardano Diagram: Datum in a Transaction

```
┌────────────────────────────────────┐
│          Transaction Output        │
│------------------------------------│
│  Value:       10 ADA               │
│  Validator:   Smart Contract       │
│  Datum:       {"votes": 5}         │
└────────────────────────────────────┘
             ▲
             │
     ┌─────────────┐
     │  Datum Type │
     └─────────────┘
             ▲
             │
     ┌──────────────────────┐
     │  BuiltinData (Raw)   │
     └──────────────────────┘
```

In this flow:

- The UTXO is locked with a validator script.
    
- The `Datum` carries data that the script will check.
    

---

## 🔚 Conclusion

The `Datum` type is central to how **stateful data** is stored in Cardano smart contracts. It allows you to embed and retrieve on-chain information safely and efficiently.

By wrapping `BuiltinData`, `Datum` gives your smart contract access to values like integers, records, or any serializable Plutus data — all within Cardano's secure, deterministic execution model.

Understanding how to use and access `Datum` is fundamental for building interactive, state-aware dApps on Cardano.

---
