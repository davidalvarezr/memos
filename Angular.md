# Angular

Angular is a single page application web framework

## Observables
Observables look like Promises, except that they can be "triggered" several times. Think about one Promise in which we could call the `resolve()` method several times.

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

### Imports
```js
import { Observable, Subscription, of, from, fromEvent, forkJoin } from 'rxjs';
import { switchMap, catchError, map  } from 'rxjs/operators';
```

### Observable **from** a Promise

```js
function test(i) {
    return new Promise((resolve, reject) => {
        if (i < 0) reject('negative');
        else resolve('zero or positive');
    });
}

const myObserver = {
    next: msg => console.log(`message: ${msg}`),
}

from(test(1)).subscribe(myObserver); // prints "zero or positive"
```

### An observable that emits several values
```js
const myObs = of (1, 'a', () => 42)
myObs.subscribe(x => console.log(x)) // prints "1" "a" "f()"
```

### Catching errors
```js
const myObs = new Observable(observer => observer.error('AN ERROR')) // An observer that throws an error
myObs
    .pipe(catchError(e => {
        console.error(e);
        return of('Error was successfully caught');
    }))
    .subscribe(x => console.log(x)) // prints "AN ERROR" "Error was successfully caught"
```

### Apply projection with each value from source (`map`)
```js
// Here it is the Observable that manages what happens on 'next()', not the Subscription anymore
const myObservable: Observable < any > = of (1, 2, 3)
    .pipe(map(x => console.log(x)));

const mySubscription: Subscription = myObservable.subscribe(); // prints "1" "2" "3"
```

### From events
#### Some events
```js
@ViewChild('hello', {static: false})
hello: ElementRef;

ngAfterViewInit() {
        fromEvent(this.hello.nativeElement, 'click')
            .subscribe(_ => console.warn('clicked'));

        fromEvent(this.hello.nativeElement, 'contextmenu')
            .subscribe(_ => console.warn('contextmenu'));

        fromEvent(this.hello.nativeElement, 'copy')
            .subscribe(_ => console.warn('copy'));

        fromEvent(this.hello.nativeElement, 'dblclick')
            .subscribe(_ => console.warn('dblclick'));
```

#### Calculate the time spent in a div
```js
@ViewChild('hello', {static: false})
hello: ElementRef;

ngAfterViewInit() {
    const mouseEnter = fromEvent(this.hello.nativeElement, 'mouseenter')
        .pipe(map(_ => {
            const d = new Date();
            console.log(`Enter: ${d.toLocaleTimeString()}`);
            return d;
        }))

    const mouseLeave = (dateMouseEnter) => fromEvent(this.hello.nativeElement, 'mouseleave')
        .pipe(map(_ => {
            const d = new Date();
            console.log(`Leave: ${d.toLocaleTimeString()}`);
            return d - dateMouseEnter;
        }))

    mouseEnter
        .pipe(switchMap(dateMouseEnter => mouseLeave(dateMouseEnter)))
        .subscribe(ml => console.log(ml + ' [ms]'));
}
```
#### Another way to do it with `zip()`
```js
const mouseEnter = fromEvent(this.hello.nativeElement, 'mouseenter')
    .pipe(map(_ => Date.now()))

const mouseLeave = fromEvent(this.hello.nativeElement, 'mouseleave')
    .pipe(map(_ => Date.now()))

zip(
    mouseEnter,
    mouseLeave
).subscribe(times => {
    const [time0, time1] = times;
    console.log(time1 - time0)
})
```

### Waiting for several Observables simultaneously, emits when last value is received
```js
const obs1 = of (1).pipe(delay(3000))
const obs2 = of (2).pipe(delay(1000))

forkJoin(
    obs1,
    obs2
).subscribe(results => {
    const [res1, res2] = results;
    console.log(res1 + res2); // print "3" after s seconds
})
```
