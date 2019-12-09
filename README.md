# Improving iteration on Objects

An ECMA TC39 proposal to improve the experience & performance of iterating and mapping over Objects

## Vitals

- **Current stage:** 1
- **Last presented:** October 2019
- **Next presentation:** February 2020
- **Author:** Jonathan Keslin (@decompil3d) -- GoDaddy
- **Champion:** Jonathan Keslin (@decompil3d) -- GoDaddy

## The problem

Objects are commonly used as the defacto dictionary type in Javascript. But Objects are not iterable by default, so
many of the valuable collection methods on Iterables are not available for Objects without first transforming the
Object into an Array<sup>[1](#footnote-1)</sup>.

## Proposed solution

### Provide functionality to get an `Iterator` on an Object

```js
// Collect result back into an Object
Object.fromEntries(
  // Utility method to get an iterator on an Object
  Object.iterateEntries(obj)
    // Standard `map` function that operates on iterables
    .map(([key, value]) => [transform(key), transform(value)])
);
```

### What exactly is being proposed

- Adding up to three static methods on `Object` to provide in-place iteration on data stored therein:
  - `Object.iterateEntries`: iterates through the Object, providing key-value pairs much like `Map.prototype.entries`
  - `Object.iterateKeys`: iterates through the Object, providing just the keys like `Map.prototype.keys`
  - `Object.iterateValues`: iterates through the Object, providing just the values like `Map.prototype.values`
  
#### `Object.iterateEntries`

```js
const obj = { foo: 'bar', baz: 'blah' };
const entriesIterator = Object.iterateEntries(obj);
for (const [key, value] of entriesIterator) {
  console.log(`${key} -> ${value}`);
}
```

#### `Object.iterateKeys`

```js
const obj = { foo: 'bar', baz: 'blah' };
const keysIterator = Object.iterateKeys(obj);
for (const key of keysIterator) {
  console.log(key);
}
```

#### `Object.iterateValues`

```js
const obj = { foo: 'bar', baz: 'blah' };
const valuesIterator = Object.iterateValues(obj);
for (const value of valuesIterator) {
  console.log(value);
}
```

#### Relationship to the existing [Iterator Helpers] proposal

- The [Iterator Helpers] proposal is not a requirement to make this proposal useful, but it substantially improves
  developer experience
- Without this proposal, the helpers provided in [Iterator Helpers] are not as valuable for data stored in Objects,
  as there are currently no mechanisms to in-place iterate through an Object.

## History of this proposal

This proposal has morphed a bit since original presentation.

### October 2019

The initial proposal was `Object.map` -- providing a static API method on `Object` to transform the keys and/or values
and return the transformed Object.

Slides: <https://1drv.ms/p/s!As13Waij_jkUqeV6IHXsJBMDkNIgXw>

#### Resolution from the meeting

There was general support for improving the experience and optimizability of mapping over Objects, but folks were
concerned about the slippery slope that adding a `map` method would create.

Instead, we agreed that the proposal could change to be an exploration on improving mapping over Objects and agreed on
Stage 1.

#### After meeting discussion

After the October meeting, the champions & authors of this proposal and the [Iterator Helpers] proposal met and
discussed how the two proposals are related. It was determined that augmenting `Iterator.from` for this purpose
would not work, since it would be ambiguous for Objects that extend `Iterator.prototype`.

Since [Iterator Helpers] provides the needed `map` mechanism, this proposal could be adjusted to focus on getting
an `Iterator` from an Object.

##### Notes

<a id='footnote-1'><sup>1</sup></a> Specifically, `Object.entries`, `Object.keys`, and `Object.values`

[Iterator Helpers]: https://github.com/tc39/proposal-iterator-helpers
