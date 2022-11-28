# A convenient(?) way of filtering null values
Think about your array of values, and for some reason after doing a bunch of operations, you end up with an array containing _[null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null)_ and _[undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)_. 

It could for example be if you have an optional field and _[map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)_ a function over that value. 

Here is an example:
```JavaScript
[{ foo: 1 }, { foo: 2 }, { foo: null }, { foo: 3 }, { bar: 4 }].map((x) => x.foo);
// => [1, 2, null, 3, undefined]
```

If we make use of the identity function `(x) => x`, we can easily _[filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)_ out `null` and `undefined` like this:
```JavaScript
[1, 2, null, 3, undefined].filter((x) => x);
// => [1, 2, 3]
```

The problem with this approach is that it might cause unexpected behaviors because some values are _[falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)_. For example the number `0` or the empty string `""`.

```JavaScript
[0, 1, 2, null, 3, 4, NaN].filter((x) => x);
// => [1, 2, 3, 4]
```
```JavaScript
["foo", null, "bar", "", undefined, "baz"].filter((x) => x);
// => ["foo", "bar", "baz"]
```

Where did `0` go and should `NaN` be there or not?

A safe way to do this is to create a more specific function that does not care is values are _[truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)_ or falsy. One way of doing this is to check if `[null, undefined]` _[includes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)_ the mapped value:
```JavaScript
[0, 1, 2, 3, null, undefined].filter((x) => ![null, undefined].includes(x));
```

## Continuation

I prefer naming this function `isDefined` that returns `true` if the input is not `null` or `undefined`, otherwise `false`

```JavaScript
const isDefined = (x) => ![null, undefined].includes(x);

[0, 1, 2, 3, null, undefined].filter(isDefined);
```

A more generic approach would be to define a function `notIn` that is curried and takes an array of values not supported like:
```JavaScript
const notIn = (compare) => (x) => !compare.includes(x);

[1, 2, 3, 4, 5, 6, 7].filter(notIn([2, 4, 6]));
// => [ 1, 3, 5, 7 ]
```

So another way of creating `isDefined` would be
```JavaScript
const isDefined = notIn([null, undefined]);
```
