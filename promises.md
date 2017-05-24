---
layout: lesson
title: "Promises"
permalink: /promises/
---

## Callbacks

FIXME: 

- Start with callbacks before going into promises. Explain what the 'Pyramid of Doom' is when working with callbacks.
- Explain how promises solve this. 

---

## Promises

- ES6 introduces a new object for asynchronous computations.  A `Promise` represents a value which may be available now, in the future, or never.  See [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) for in depth description
- The constructor takes a function which takes two functions (`resolve` and `reject`) to be called asynchronously when the `Promise` completes or fails
- You can chain promises with the `Promise.prototype.then(onFulfilled, onRejected)` method or catch only failures with `Promise.prototype.catch(onRejected)`

FIXME This looks confusing. I think we should suggest to just use `Promise.resolve(...)then`. Also the below example should be simplified. Probably best to start with 
```js 
var promise = $http.get('http://localhost:3000/api/products');
promise.then((response) => {
    //...
}); 
```

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
 
## Chaining Promises 1/2

FIXME:

- Show how we can chain promises using `then`
- Explain why this is bad

```js
  .then(function(x){     
     //...
  })
  .then(function (y) {
     //...
  })
```
---

## Chaining Promises 2/2

FIXME Expand previous section and show how this can be rewritten by creating different function

---

## Error Handling

FIXME: Explain how errors are handled within promises

---

## Promise Statuses

FIXME: Explain what statuses are available within a promise. 

---

## Caching Promise Result

FIXME: Explain how we can cache the result of promises

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
