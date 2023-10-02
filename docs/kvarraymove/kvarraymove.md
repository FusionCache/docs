---
layout: default
title: Move Array Item
nav_order: 37
parent: KV API
has_children: false
---

# KV_ARRAY_MOVE
Moves the position of an item in an array to a different position *in the same array*.

If a key has an array value type, this command can be used to move items around.

Multiple keys are permitted.

<br/>

## Structure
The command has two forms:

  1. two indexes: the item index to move and the insertion index
  2. one index: the item to move only, it is inserted at the end of the array

Position indexes are zero-based so the first item is `0`.

<br/>

## With Insert Index
A `KV_ARRAY_MOVE` object with the key name, as a two element array:

```json
{
  "KV_ARRAY_MOVE":
  {
    "<mykeyname>":[indexOfItemToMove, insertIndex]
  }
}
```

This moves the item in position `indexOfItemToMove` and inserts it at position `insertIndex`:

<br/>

### Examples
Given an array "myarray" with value `["a", "b", "c"]`:

We want to take "c" and insert it at the start:

```json
{
  "KV_ARRAY_MOVE":
  {
    "myarray":[2, 0]
  }
}
```

The `[2,0]` is:
- `2` is the third item ("c")
- `0` is the insert index


This will now be `["c", "b", "a"]`.

If you want to move an item to the end using this syntax, you need to set the `insertIndex` to after the last item:


```json
{
  "KV_ARRAY_MOVE":
  {
    "myarray":[0, 3]
  }
}
```

This will now be `["a", "b", "c"]`.

<br/>

## Without Insert Index (moves to end)
This always moves an item to the end of the array:

```json
{
  "KV_ARRAY_MOVE":
  {
    "<mykeyname>":[indexOfItemToMove]
  }
}
```
This moves the item in position `indexOfItemToMove` and inserts at the end.


### Example
Given an array "myarray" with value `["a", "b", "c", "d"]`:

Move "b" to the end:

```json
{
  "KV_ARRAY_MOVE":
  {
    "myarray":[1]
  }
}
```

The array will now be: `["a", "c", "d", "b"]`

<br/>

## Response
`KV_ARRAY_MOVE_RSP` object containing the status (`st`) and key (`k`).

These are status names, their integer values are listed [here](../kvstatuslist.md):

- Ok
- KeyNotExist
- KeyLengthInvalid
- ValueTypeInvalid
- ValueSize
- OutOfBounds


<br/>

{: .important}
> You will receive a response for each key in `KV_APPEND`.
>
> The order of the responses is not gauranteed to be the same as in the `KV_APPEND` query.

<br/>

### Error Examples

*ValueTypeInvalid*

If you the array contains a non-integer:
```json
{
  "KV_ARRAY_MOVE":
  {
    "users:groups:":[0, "1"]
  }
}
```
This should have have been `[0,1]`.

<br/>

*ValueSize*

No indexes supplied:
```json
{
  "KV_ARRAY_MOVE":
  {
    "users:groups:":[]
  }
}
```

Too many indexes supplied:
```json
{
  "KV_ARRAY_MOVE":
  {
    "users:groups:":[1,2,3]
  }
}
```

<br/>

*OutOfBounds*

An index is out of range of the data.

If `users:groups` has 3 items and we:

```json
{
  "KV_ARRAY_MOVE":
  {
    "users:groups:":[4,1]
  }
}
```

`4` is the 5th item but we only have 3.

The same applies if the `users:groups` array is empty and we do:

```json
{
  "KV_ARRAY_MOVE":
  {
    "users:groups:":[0]
  }
}
```

We can't move the first item because there are no items.

<br/>


## Examples
In the examples above all the arrays have string elements but that's for clarity, you can move array items of any supported JSON type.

<br/>

### Array with Object Items

Store an array of objects:

```json
{
  "KV_SET":
  {
    "user:1234:favourites":
    [
      {"name":"Homework", "url":"school.com/pain"},
      {"name":"Videos", "url":"youtube.com/mychannel"},
      {"name":"Pubs", "url":"pubs.com/blah"},
      {"name":"Football", "url":"footyfootball.com"}     
    ]
  }
}
```

We must deprioritise "Homework" to be last:

```json
{
  "KV_ARRAY_MOVE":
  {
    "user:1234:favourites":[0]
  }
}
```

We use the single index version which moves an item to the end.

If we get the key now we have:

```json
{
  "KV_GET_RSP":
  {
    "user:1234:favourites":
    [
      {"name": "Videos","url": "youtube.com/mychannel"},
      {"name": "Pubs","url": "pubs.com/blah"},
      {"name": "Football","url": "footyfootball.com"},
      {"name": "Homework","url": "school.com/pain"}
    ],
    "st": 1
  }
}
```
