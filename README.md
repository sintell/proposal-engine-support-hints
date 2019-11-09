# proposal-use-implemenetation-hints
Allow users to mark function as implementation/polyfill of some ES feature that is compilant to the spec and may be potentially replaced with engine internal implementation call

# Motivation
This feature will be benificial for any users who polyfill their code for it to be compilant with new ES specification but also have to support older versions.

As it now will allow JS Engine to replace userland implementation with internal one at a runtime to increase perfomance throughoutput


# Conditions
In order to use this optimisation engine must check if, for example, method has the same signature as the one that engine provides

# API
It is proposed to introduce new `"use implemenation X"` pragma that could be used in a function scope to tell ES runtime that it could swap function body with an internall call to the `X` feature

We could use it in a way like that to polyfill `Map.merge` method

```js
function mapMerge(iterable /* ...iterbles */) {
  "use implementation Map.merge";
  
  var map = anObject(this);
  var setter = aFunction(map.set);
  var i = 0;
  while (i < arguments.length) {
    iterate(arguments[i++], setter, map, true);
  }
  return map;
}

Map.prototype.merge = mapMerge;

```
As we can see in the example above – our `mapMerge` function is fully compatable with the [proposal](https://github.com/tc39/proposal-collection-methods#proposal) so at a runtime function body could be swapped with internal implementation

