# The Truth About Falsy

Read more about what [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) and [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) is in JavaScript.

## A convenient way of filtering null values
Think about your array of values and for some reason after doing a bunch of operations you end up with an array with some null values. I could for example be if you have an optional field and maps over that value. Here is an example
```JavaScript
const xs = [
  { foo: 1 }, 
  { foo: 2 }, 
  { foo: null }, 
  { foo: 3 }, 
  { bar: 4 }
]
```

```JavaScript
xs.map(x => x.foo)
// => [1, 2, null, 3, undefined]
```

If we make use of the identity function we can easily filter out `null` and `undefined` like this
```JavaScript
const identity = x => x

[1, 2, null, 3, undefined].filter(identity)
// => [1, 2, 3]
```
