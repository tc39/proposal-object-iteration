# Making mapping over Objects more concise

An ECMA TC39 proposal to improve the experience & performance of mapping over Objects

## Vitals

- **Current stage:** 1
- **Last presented:** October 2019
- **Author:** Jonathan Keslin (@decompil3d) -- GoDaddy
- **Champion:** Jonathan Keslin (@decompil3d) -- GoDaddy

## The problem

Objects are commonly used as the defacto dictionary type in Javascript. But Objects are not iterable by default, so
many of the valuable collection methods on Iterables are not available for Objects without first transforming the
Object into an Array.

## Options for solutions

### Provide functionality to get an Iterator on an Object and collect an iterator into an Object

```js
// Utility method to get an iterator on an Object
Iterator.from(obj)
  // Standard `map` function that operates on iterables
  .map(([key, value]) => [transform(key), transform(value)])
  // Collect the iterable data back into an Object, specifying key and value selectors
  .toObject(([key]) => key, ([, value]) => value);
```

#### How this extends the existing [Iterator Helpers] proposal

- As spec'd, `Iterator.from` only accepts an `Object` with an `@@iterator` method or an Iterator-like object (i.e. one
  with a callable `next` method). This proposal extends `Iterator.from` to allow building a synchronous iterator from
  an `Object`.
  - If this is not palatable to the committee, we could consider an alternative of `Object.iterate(obj)` or similar.
- As spec'd, `Iterator.prototype` does not contain `toObject`, so we must add this.
  - Should we consider having a default key and value selector to make it easier to handle common key-value-pair items?

## History of this proposal

This proposal has morphed a bit since original presentation.

### October 2019

The initial proposal was `Object.map` -- providing a static API method on `Object` to transform the keys and/or values
and return the transformed `Object`.

Slides: <https://1drv.ms/p/s!As13Waij_jkUqeV6IHXsJBMDkNIgXw>

#### Resolution from the meeting

There was general support for improving the experience and optimizability of mapping over Objects, but folks were
concerned about the slippery slope that adding a `map` method would create.

Instead, we agreed that the proposal could change to be an exploration on improving mapping over Objects and agreed on
Stage 1.

[Iterator Helpers]: https://github.com/tc39/proposal-iterator-helpers
