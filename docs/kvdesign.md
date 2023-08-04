---
layout: default
title: Design
nav_order: 2
---

# Key Value Design
Key value is much simpler than Object Mode:

1. There are no relationships to manage - keys are completely independant
2. KV queries require less processing to parse


This allows Fusion to optimise how it handles keys.

<br/>

## Network I/O
The Websocket API has dedicated thread(s) and is decoupled from the query processing. This means an I/O thread is used only when performing an I/O task or when initially parsing the query before execution.

<br/>

## Concurrency
Unlike Object Mode, KV Mode is concurrent with both read and write operations. This allows very high query throughput for the given CPU.

<br/>

## Constraints
The only constraints are:

- keys must be a string
- keys have a minimal length of 6 characters (this may be configurable in a future release)
