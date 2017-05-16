---
layout: lesson
title: "Promises"
permalink: /promises/
---

---

## Promises

- ES6 introduces a new object for asynchronous computations.  A `Promise` represents a value which may be available now, in the future, or never.  See [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) for in depth description
- The constructor takes a function which takes two functions (`resolve` and `reject`) to be called asynchronously when the `Promise` completes or fails
- You can chain promises with the `Promise.prototype.then(onFulfilled, onRejected)` method or catch only failures with `Promise.prototype.catch(onRejected)`

```js
function msgAfterTimeout (msg, who, timeout) {
    return new Promise((resolve, reject) => {
        setTimeout(() => resolve(`${msg} Hello ${who}!`), timeout)
    })
}
msgAfterTimeout("", "Foo", 100).then((msg) =>
    msgAfterTimeout(msg, "Bar", 200)
).then((msg) => {
    console.log(`done after 300ms:${msg}`)
})
```

---

## Promises

- You can create a promise that resolves when a set of Promises all resolve (via the static `Promise.all(iterable)`) or when the first of a set of Promises resolve (via the static `Promise.race(iterable)`)

```js
function fetchAsync (url, timeout, onData, onError) { ... }
let fetchPromised = (url, timeout) => {
    return new Promise((resolve, reject) => { fetchAsync(url, timeout, resolve, reject) })
}
Promise.all([
    fetchPromised("http://backend/foo.txt", 500),
    fetchPromised("http://backend/bar.txt", 500),
    fetchPromised("http://backend/baz.txt", 500)
]).then((data) => {
    let [ foo, bar, baz ] = data
    console.log(`success: foo=${foo} bar=${bar} baz=${baz}`)
}, (err) => {
    console.log(`error: ${err}`)
})
Promise.race([
    fetchPromised("http://backend/foo.txt", 500),
    fetchPromised("http://backend/bar.txt", 500),
    fetchPromised("http://backend/baz.txt", 500)
]).then((data) => {
    console.log(`fastest is: ${data}`)
}, (err) => {
    console.log(`error: ${err}`)
})
```
