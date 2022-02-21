# Let vs Const

Example code:

```js
let badArray = [];
if (someCondition) {
  badArray.push("hi");
}
```

Here, badArray can be a const. This is a small difference but it is important. When a variable is const, it is not reassignable. However, its methods are still accessible. So here, you could not run the following code,

```js
const badArray = [];
if (someCondition) {
  badArray = ["hi"]; // Cannot reassign using the = operator
}
```

But you can use the methods on a const, like this:

```js
const badArray = [];
if (someCondition) {
  badArray.push("hi");
}
```

This is a big gotcha when dealing with objects. Sometimes we think that the following isn't possible

```js
const myObj = {};
myObj.someProperty = "hi";
```

but that is perfectly valid javascript. Modifying a property is not the same as reassigning!
