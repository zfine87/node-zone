> minimal Zone implementation for Node

This implementation is based on the [experimental AsyncWrap API](https://github.com/nodejs/diagnostics/blob/master/tracing/AsyncWrap/README.md), but it seems to work fine.

Work with any asynchronous tasks (IO, timers, promises, external modules, …).

## Install

Installation of the [npm package](https://npmjs.org/package/node-zone):

```
> npm install --save node-zone
```

## Usage

```js
import { current } from 'node-zone'

// a zone has a name, a parent, and data
console.log(
  current.name,   // "<root>"
  current.parent, // null
  current.data    // { __proto__: null }
)

// create a new child zone
const myZone = current.fork('my zone')

console.log(
  myZone.name,   // "my zone"
  myZone.parent, // current
  myZone.data    // { __proto__: current.data }
)

// run some code in it
myZone.run(() => {
  console.log(current.name) // "my zone"

  // zone is preserved in async functions
  process.nextTick(() => {
    console.log(current.name) // "my zone"
  })
})

console.log(current.name) // "<root>"
```
