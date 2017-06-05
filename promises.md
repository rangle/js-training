---
layout: lesson
title: "Promises"
permalink: /promises/
---


## Trying to sequence code using callbacks

Example: Online scheduling platform for booking transit tickets

```
get(`${baseUrl}/schedule`, trips => {
    trips.forEach(trip => {
        const tripItemElement = createTripItem();

        tripItemElement.book.onClick = () => {
            get(`${baseUrl}/schedule/available/${trip.id}`, isAvailable => {
                if(isAvailable) {
                    post(`${baseUrl}/schedule/book/${trip.id}`, result => {
                        if(result.success) {
                            showSuccessMessage();
                        } else {
                            showErrorMessage();
                        }
                    });
                } else {
                    showUnavailableMessage();
                }
            });
        };
    });
});
```

---

## Trying to sequence code using callbacks

- Code forms a pyramid shape, sometimes referred to as the "Pyramid of Doom"
- Difficult to read and understand, scales poorly with complexity
- Side effects are now coupled with the initial async call
- Unable to catch errors thrown within callbacks
- Problem stems from needing to sequence the order of execution within asynchronous code

---

## What Are Promises?

- A `Promise` represents a value which may be available now, in the future, or never
- Was introduced as a part of the ES2015 spec
- Are _asynchronous_ and _eager_, meaning the promise will be queue'd up to run as soon as the promise is created

---

## How are promises created?

- Promises are created using the `Promise` constructor
- The constructor takes a function, which gets _resolve_ and _reject_ functions passed to it
- resolve and reject determine whether or not the promise completed successfully or ran into an error

```js
function greetLater(msg, who, timeout) {
    return new Promise((resolve, reject) => {
        setTimeout(() => resolve(`${msg} Hello ${who}!`), timeout)
    })
}
```

---

## Promise states

- Promises are always in one of three states:
    - __fulfilled__, the promise has completed running successfully
    - __rejected__, the promise has failed to complete
    - __pending__, the promise has yet to be fulfilled or rejected

---

## Sequencing Code With Promises

- code can be sequenced using a promise's `then` operator
- `then` accepts a function with the result of the source promise, and returns either:
    - a new value to pass down the promise chain
    - another promise that needs to be fulfilled or rejected before moving the chain forward

```js
function greetLater(msg, who, timeout) {
    return new Promise((resolve, reject) => {
        setTimeout(() => resolve(`${msg} Hello ${who}!`), timeout)
    })
}
greetLater('', 'Foo', 100)
    .then(msg =>
        msgAfterTimeout(msg, 'Bar', 200);
    )
    .then(msg => {
        // side effect is separated from rest of the logic!
        console.log(`done after 300ms:${msg}`)
    })
```

---

## Handling Errors

- errors are handled using a promise's `catch` operator
- accepts a function with the value that as `rejected` or the error thrown
- breaks the chain

```js
new Promise((resolve, reject) => {
    setTimeout(() => {
        reject(new Error('something bad happened!'));
    }, 0)
})
.then(() => {
    // this is skipped
})
.catch(err => {
    // this is the error object
});
```

```js
new Promise((resolve, reject) => {
    setTimeout(() => {
        throw new Error('something bad happened!');
    });
})
.catch(err => {
    // same error
})
```

---

## Things to Avoid

1. Avoid nesting promises within each other

#### Don't
```js
new Promise((resolve, reject) => {
    // do first thing here
    new Promise((resolve, reject) => {
        // do second thing here
        new Promise((resolve, reject) => {
            // do third thing here
        });
    });
});
```

#### Do
```js
new Promise((resolve, reject) => {
    // do first thing here
}).then(step1Result => {
    // do second thing here
    return new Promise((resolve, reject) => { });
}).then(step2Result => {
    // do third thing here
    return new Promise((resolve, reject) => { });
});
```

---

## Things to Avoid

### 2. Chaining promises that don't rely on each other

#### Don't

```js
doThing1()
    .then(doThing2)
    .then(doLastThing);
```

#### Do

```js
Promise.all([ doThing1(), doThing2() ])
    .then(doLastThing);
```
