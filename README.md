# flow-importing-and-exporting-types-error

An error given by following the flow documentation from:

* <https://flow.org/en/docs/types/modules/#toc-importing-and-exporting-types>
* <https://github.com/facebook/flow/blob/41b0eab99cdc5199421f7cccad9e0c4950f8b2f9/website/en/docs/types/modules.md>

## Error

**Error:(4, 25) Cannot reference type `Foo` [1] from a value position.**

**`exports.js`**

```js
// @flow
export default class Foo {}
export type MyObject = { name: string };
export interface MyInterface { age: number; }
```

**`importsError.js`**

```js
// @flow
import type Foo, { MyObject, MyInterface } from "./exports";

const concreteFoo = new Foo(); // ✗ Error:(4, 25) Cannot reference type `Foo` [1] from a value position.
```

## No Error

Importing the types and the class separately does not yield errors.

**`exports.js`**

```js
// @flow
export default class Foo {}
export type MyObject = { name: string };
export interface MyInterface { age: number; }
```

**`importsSuccess.js`**

```js
// @flow
import type { MyObject, MyInterface } from "./exports";
import Foo from "./exports";

const concreteFoo = new Foo(); // ✓ OK
```

## Full Console Error Printout

```txt
> flow

Error --------------------------------------------------------------------------------------------- importsError.js:4:25

Cannot reference type `Foo` [1] from a value position.

   importsError.js:4:25
   4| const concreteFoo = new Foo();
                              ^^^

References:
   importsError.js:2:13
   2| import type Foo, { MyObject, MyInterface } from "./exports";
                  ^^^ [1]
```