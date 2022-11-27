# A convenient(?) way of filtering null values
Think about your array of values and for some reason after doing a bunch of operations you end up with an array with some null values. I could for example be if you have an optional field and maps over that value. Here is an example
```JavaScript
[{ foo: 1 }, { foo: 2 }, { foo: null }, { foo: 3 }, { bar: 4 }].map((x) => x.foo);
// => [1, 2, null, 3, undefined]
```

If we make use of the identity function `(x) => x` we can easily filter out `null` and `undefined` like this
```JavaScript
[1, 2, null, 3, undefined].filter((x) => x);
// => [1, 2, 3]
```

Problem is that this might causes unexpected behaviors because of falsiness of for instance the number `0` or the empty string `""`

```JavaScript
[0, 1, 2, null, 3, 4, NaN].filter((x) => x);
// => [1, 2, 3, 4] // where did 0 go?

["foo", null, "bar", "", undefined, "baz"].filter((x) => x);
// => ["foo", "bar", "baz"] // where did "" go?
```

The safe way to do it is to create a more specific function that does not rely on the nature of truthy/falsy values e.g.
```JavaScript
[0, 1, 2, 3, null, undefined].filter((x) => ![null, undefined].includes(x));
```

---

_Read more about what [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) and [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) is in JavaScript._
