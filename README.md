# Actor Classes

![Compatibility](https://img.shields.io/badge/compatibility-0.7.0-blue)
[![Build Status](https://github.com/dfinity/examples/workflows/motoko-classes-example/badge.svg)](https://github.com/dfinity/examples/actions?query=workflow%3Amotoko-classes-example)

## Overview

This example demonstrates a simple use of actor classes, which allow a program to dynamically install new actors (that is, canisters). It also demonstrates a multi-canister project, and actors using inter-actor communication through shared functions.

The example defines two Motoko actors, `Map`.

Map is a simple, distributed key-value store, mapping `Nat` to `Text` values, with entries stored in a small number of separate Bucket actors, installed on demand.

`Map.mo` imports a Motoko actor class `Bucket(i, n`) from library `Buckets.mo`. It also imports the `ExperimentalCycles` base library, naming it `Cycles` for short, in order to share its cycles amongst the bucket it creates.

Each call to `Buckets.Bucket(n, i)` within Map instantiates a new Bucket instance (the i-th of n) dedicated to those entries of the Map whose key hashes to i (by taking the remainder of the key modulo division by n).

Each asynchronous instantiation of the Bucket actor class corresponds to the dynamic, programmatic installation of a new Bucket canister.

Each new Bucket must be provisioned with enough cycles to pay for its installation and running costs. Map achieves this by adding an equal share of Map's initial cycle balance to each asynchronous call to `Bucket(n, i)`, using a call to `Cycles.add(cycleShare`).

This is a Motoko example that does not currently have a Rust variant. 


## Prerequisites
This example requires an installation of:

- [x] Install the [IC SDK](../developer-docs/setup/install/index.mdx).

Begin by opening a terminal window.

### Step 1: Navigate into the folder containing the project's files and start a local instance of the Internet Computer with the command:

```
cd examples/motoko/classes
dfx start --background
```

### Step 2: Deploy the canisters `Map`:

```
dfx deploy
```

## Security considerations and security best practices

If you base your application on this example, we recommend you familiarize yourself with and adhere to the [security best practices](https://internetcomputer.org/docs/current/references/security/) for developing on the Internet Computer. This example may not implement all the best practices.
