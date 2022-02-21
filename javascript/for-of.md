# For Of loops

It's a common sight to see code like the following:

```js
let len = array.length;
for (let i = 0; i < len; i++) {
  const name = array[i];
}
```

However, this can be refactored to 

```js
for (const name of array) {

}
```

If you want a deeper look at this, check out the difference between for of and for in in javascript. (of iterates values, in iterates keys)
