# Angular

## Observables

### Promises vs Observables

**Promise**

Once `resolve()` or `reject()` is called, it is finished.

```js
function test(i) {
    return new Promise((resolve, reject) => {
        if (i < 0) reject('negative');
        else resolve('zero or positive');
    });
}

test(-1)
    .then(msg => console.log(`message: ${msg}`))
    .catch(err => console.log(`error: ${err}`)) // prints "error: negative"
test(1)
    .then(msg => console.log(`message: ${msg}`)) // prints "message: zero or positive"
    .catch(err => console.log(`error: ${err}`))

// Chaining
test(1)
    .then(i => 42) // same as .then(i => { return 42; })
    .then(j => console.log(j)) // prints "42"
```

**Observable**

`next()` can be called a lot of times. Look at my `of()` implementatio below.

Once `error()` or `complete()` is called, it is finished.


```js
function test(i) {
    return new Observable((obs) => {
        if (i < 0) obs.error('negative');
        if (i === 0) obs.complete();
        else obs.next('positive');
    });
}

const myObserver = {
    next: msg => console.log(`message: ${msg}`),
    error: err => console.log(`error: ${err}`),
    complete: () => console.log('completed'),
}

test(-1).subscribe(myObserver); // prints "error: negative"
test(1).subscribe(myObserver); // prints "message: positive"
test(0).subscribe(myObserver); // prints "completed"

// Chaining
test(1).pipe(
        switchMap(msg => of('msg has been received')) // of returns an observable
    )
    .subscribe(msg => console.log(msg)) // prints "msg has been received"
```
**My `of()` implementation** (not sure if it is the exact same behaviour)
```js
function of (...values) {
    return new Observable(obs => {
        for (const val of values) {
            obs.next(val);
        }
    })
}
```
