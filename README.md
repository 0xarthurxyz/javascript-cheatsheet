# JavaScript (Cheat Sheet)

This is a cheat sheet on JavaScript (mostly notes-to-self). They are incomplete by default.

## Basics (from [exercism.org](https://exercism.org/tracks/javascript))

Source: [exercism.org](https://exercism.org/tracks/javascript/concepts/basics)

> [!TIP]  
> Most of the concepts and text below are taken from the JavaScript track on
> [exercism.org](https://exercism.org/tracks/javascript). The material is licensed under a MIT
> license, which is included in this repository as well.

### (Re-)Assignment

There are a few primary ways to assign values to names in JavaScript - using variables or constants.
Typically, variables are written
in [camelCase](https://en.wikipedia.org/wiki/Camel_case "https://en.wikipedia.org/wiki/Camel_case");
constants are written
in [SCREAMING_SNAKE_CASE](https://en.wikipedia.org/wiki/Snake_case "https://en.wikipedia.org/wiki/Snake_case").

Variables in JavaScript can be defined using
the [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const), [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let) or [`var`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var) keyword.

A variable can reference different values over its lifetime when using `let` or `var`.

For example, `myFirstVariable` can be defined and redefined many times using the assignment
operator `=`:

```js
let myFirstVariable = 1;
myFirstVariable = "Some string";
myFirstVariable = new SomeComplexClass();
```

In contrast, variables that are defined with `const` can only be assigned once. This is used to
define constants in JavaScript.

```js
const MY_FIRST_CONSTANT = 10;

// Can not be re-assigned.
MY_FIRST_CONSTANT = 20;
// => TypeError: Assignment to constant variable.
```

#### Constant Assignment

The `const` keyword is mentioned *both* for variables and constants. Another concept often mentioned
around constants is [(im)-mutability](https://en.wikipedia.org/wiki/Immutable_object).

The `const` keyword only makes the *binding* immutable, that is, you can only assign a value to
a `const` variable once. In JavaScript,
only [primitive](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) values are immutable.
However, [non-primitive](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) values can
still be mutated.

```js
const MY_MUTABLE_VALUE_CONSTANT = { food: "apple" };

// This is possible
MY_MUTABLE_VALUE_CONSTANT.food = "pear";

MY_MUTABLE_VALUE_CONSTANT;
// => { food: "pear" }
```

#### Constant Value (Immutability)

As a rule, on Exercism, and many other organizations and project style guides, don't mutate values
that look like `const SCREAMING_SNAKE_CASE`. Technically the values *can* be changed, but for
clarity and expectation management on Exercism this is discouraged. When this *must* be enforced,
use [`Object.freeze(value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze).

```js
const MY_VALUE_CONSTANT = Object.freeze({ food: "apple" });

// This silently fails
MY_VALUE_CONSTANT.food = "pear";

MY_VALUE_CONSTANT;
// => { food: "apple" }
```

In the wild, it's unlikely to see `Object.freeze` all over a code base, but the rule to not mutate
a `SCREAMING_SNAKE_CASE` value ever, is a good rule; often enforced using automated analysis such as
a linter.

### Function Declarations

In JavaScript, units of functionality are encapsulated in *functions*, usually grouping functions
together in the same file if they belong together. These functions can take parameters (arguments),
and can *return* a value using the `return` keyword. Functions are invoked using `()` syntax.

```js
function add(num1, num2) {
    return num1 + num2;
}

add(1, 3);
// => 4
```

> 💡 In JavaScript there are *many* different ways to declare a function. These other ways look
> different than using the `function` keyword. The track tries to gradually introduce them

### Export and Import

The `export` and `import` keywords are powerful tools that turn a regular JavaScript file into
a [JavaScript module](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules). Apart
from allowing code to selectively expose components, such as functions, classes, variables and
constants, it also enables a whole range of other features, such as:

-   [Renaming exports and imports](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#Renaming_imports_and_exports),
    which allows you to avoid naming conflicts,
-   [Dynamic Imports](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#Dynamic_Imports),
    which loads code on demand,
-   [Tree shaking](https://bitsofco.de/what-is-tree-shaking/), which reduces the size of the final
    code by eliminating side-effect free modules and even contents of modules *that are not used*,
-   Exporting [_live bindings_](https://2ality.com/2015/07/es6-module-exports.html#es6-modules-export-immutable-bindings),
    which allows you to export a value that mutates everywhere it's imported if the original value
    mutates.

A concrete example is how the tests work on Exercism's JavaScript Track. Each exercise has at least
one implementation file, for example `lasagna.js`, and each exercise has at least one test file, for
example `lasagna.spec.js`. The implementation file uses `export` to expose the public API and the
test file uses `import` to access these, which is how it can test the implementation's outcomes.

```js
// file.js
export const MY_VALUE = 10;

export function add(num1, num2) {
    return num1 + num2;
}

// file.spec.js
import { MY_VALUE, add } from "./file";

add(MY_VALUE, 5);
// => 15
```

## Numbers (from [exercism.org](https://exercism.org/tracks/javascript))

Source: [exercism.org](https://exercism.org/tracks/javascript/concepts/numbers)

> [!TIP]  
> Most of the concepts and text below are taken from the JavaScript track on
> [exercism.org](https://exercism.org/tracks/javascript). The material is licensed under a MIT
> license, which is included in this repository as well.

### About numbers

There are two different kinds of numbers in JavaScript - numbers and "bigints"

Numbers are the most used, and represent numeric data type in the double-precision 64-bit
floating-point format.

-   `number`: a numeric data type in the double-precision 64-bit floating-point format (IEEE 754).
    Examples
    are `-6`, `-2.4`, `0`, `0.1`, `1`, `3.14`, `16.984025`, `25`, `976`, `1024.0` and `500000`.
-   `bigint`: a numeric data type that can represent *integers* in the arbitrary precision format.
    Examples are `-12n`, `0n`, `4n`, and `9007199254740991n`.

```js
let numericValue = 42;
// => 42
```

A number literal like `42` in JavaScript code is a floating-point value, not an integer. There is no
separate integer type in common everyday use. The `bigint` type is not designed to replace
the `number` type for everyday uses. `42` is still a `Number`, not a `BigInt`.

Numbers may also be expressed in literal forms like `0b101`, `0o13`, `0x0A`. Learn more on numeric
lexical
grammar [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#numeric_literals).

### Special Notations

#### Exponential Notation

The E-notation indicates a number that should be multiplied by 10 raised to a given power. The
format of E-notation is to have a number, followed by `e` or `E`, than by the power of 10 to
multiply by.

```js
const num = 3.125e7;
// => 31250000
// The notation essentially says, "Take 3.125 and multiply it by 10^7".
```

E-notation can also be used to represent very small numbers:

```js
const num = 325987e-6; // Equals to 0. 325987
// The notation essentially says, "Take 325987 and multiply it by 10^-6.
```

#### Underscore Notation

Underscores can be used to make large numbers easier to read for the user. The compiler will
completely ignore the underscores.

```
const num = 1_000_000; // You can read this as 1,000,000
console.log(num);
// => 1000000
```

### Built-in Object

There are two built-in objects that are useful when dealing with numbers:

-   [`Number`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number):
    static properties for common / useful values, static methods
    for [type-checking](https://exercism.org/tracks/javascript/concepts/type-checking) and [type-conversion](https://exercism.org/tracks/javascript/concepts/type-conversion),
    instance methods
    for [type-conversion](https://exercism.org/tracks/javascript/concepts/type-conversion) and [formatting numbers as strings](https://exercism.org/tracks/javascript/concepts/string-formatting).
-   [`Math`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math):
    properties and methods for mathematical constants and functions, does **not** work
    with `BigInt`.

`Math` also includes methods for rounding numbers. You can read more about the available rounding
options in this [javascript.info article on rounding](https://javascript.info/number#rounding).

```js
Math.floor(234.34); // => 234
Math.ceil(234.34); // => 235
```

The `Number` built-in global `object` is *also* a global `function` that can be used to
convert *almost anything* number-like to a `number`. It is less forgiving
than *parsing* a `string` to a `number`.

```js
const date = new Date("December 17, 1995 03:24:00");
const unix = Number(date);

unix;
// => 819199440000
```

There are three types of maximum (and minimum / maximum negative) values for numbers in JavaScript:

-   `VALUE`: given by `Number.MAX_VALUE` and `Number.MIN_VALUE`
-   `INFINITY`: given by `Number.POSITIVE_INFINITY` and `Number.NEGATIVE_INFINITY`
-   `SAFE_INTEGER`: given by `Number.MAX_SAFE_INTEGER` and `Number.MIN_SAFE_INTEGER`

Because of how numbers in JavaScript are implemented, **not** every number
between `Number.MIN_VALUE` and `Number.MAX_VALUE` can be represented. However, *every* number
between `Number.MIN_SAFE_INTEGER - 1` and `Number.MAX_SAFE_INTEGER + 1` **can** be represented.

### Special Numbers Values

JavaScript has several special number values:

-   Two error values, `NaN` and `Infinity`.
-   Two values for zero, `+0` and `-0`.

#### NaN - Not a Number

The error value `NaN`(aka "Not a Number") is produced in the following cases.

-   A number could not be parsed:

    ```js
    Number("123"); // => 123
    Number("Hello, World!"); // => NaN
    ```

-   An operation failed:

    ```js
    Math.sqrt(-64); // => NaN
    ```

-   One of the operands is NaN:
    ```js
    NaN + 69; // => NaN
    ```

`NaN` is the only value that is not equal to itself:

```js
NaN === NaN; // => false
```

If you want to check whether a value is `NaN`, you have to use the global function `isNaN()`:

```js
isNaN(NaN); // => true
isNaN(123); // => false
```

#### Infinity

`Infinity` is an error value indicating one of two problems:

-   A number can't be represented because its magnitude is too large.

    ```js
    Math.pow(2, 1024); // => Infinity
    ```

-   A division by zero has happened.
    ```js
    6 / 0; // => Infinity
    -6 / 0; // => -Infinity
    ```

`Infinity` is larger than any other number (except `NaN`). Similarly, `-Infinity` is smaller than
any other number (except `NaN`)

The global function `isFinite()` allows you to check whether a value is an actual number (neither
infinite nor `NaN`):

```js
isFinite(80085); // => true
isFinite(Infinity); // => false
isFinite(NaN); // => false
```

#### The Two Zeros

`+0` or `-0` are distinct numbers in JavaScript. They can be produced if you represented a number,
that is so small that it is indistinguishable from 0. The signed zero allows you to record "from
which direction" you approached zero; that is, what sign the number had before it was considered
zero. It is best practise to pretend there's only one zero.

### Comparison

Numbers are considered equal if they have the same value.

```js
1 == 1.0;
// => true

1 === 1.0;
// => true
// Remember, all numbers are floating-points, so this is
// different syntax for the exact same value.

1 === 1n;
// => false
// Strictly checking a number against a bigint will always result
// in false.
```

See [comparison](https://exercism.org/tracks/javascript/concepts/comparison) for more information on
comparisons in general and comparing numeric values in JavaScript.

### Pitfalls

Because numbers in JavaScript are floating-point numbers, all math using these values is
floating-point math. Therefore, in JavaScript:

```js
0.1 + 0.2 === 0.3;
// => false
```

See [0.30000000000000004.com](https://0.30000000000000004.com/) for a brief explanation
and [Appendix D](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html) of Oracle's
Numerical Computation Guide "What Every Computer Scientist Should Know About Floating-Point
Arithmetic" for an in-depth explanation.

## Other notes

### References (variables)

Source: [Airbnb style guide](/airbnb.md#references).

Use `const` for all of your references; avoid using `var`.

> Why? This ensures that you can’t reassign your references, which can lead to bugs and difficult to
> comprehend code.

```js
// bad
var a = 1;
var b = 2;

// good
const a = 1;
const b = 2;
```

If you must reassign references, use `let` instead of `var`.

> Why? `let` is block-scoped rather than function-scoped like `var`.

```js
// bad
var count = 1;
if (true) {
    count += 1;
}

// good, use the let.
let count = 1;
if (true) {
    count += 1;
}
```

Both `let` and `const` are _block-scoped_, whereas `var` is _function-scoped_.

```js
// const and let only exist in the blocks they are defined in.
{
    let a = 1;
    const b = 1;
    var c = 1;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
console.log(c); // Prints 1
```

You can see that referencing `a` and `b` will produce a ReferenceError, while `c` contains the
number. This is because `a` and `b` are block scoped, while `c` is scoped to the containing
function.

### Objects

-   JavaScript objects

    -   are key-value pairs that can contain strings, numbers, arrays, functions, booleans, and
        other objects

    ```js
    // JavaScript Object
    const jsObj = {
        name: "Alice",
        age: 30,
    };
    ```

-   JSON objects

    -   are text-only (that means both keys and values are `strings`)

        ```js
        // JSON Object
        const jsonObj = {
            name: "Alice",
            age: "30",
        };
        ```

    -   you can validate JSON objects using [jsonlint.com](https://jsonlint.com/)

Source: [other notes](other.md#json-object-v-javascript-object).

Use the literal syntax for object creation.

```javascript
// bad
const item = new Object();

// good
const item = {};
```

Use computed property names when creating objects with dynamic property names.

> Why? They allow you to define all the properties of an object in one place.

```javascript
function getKey(k) {
    return `a key named ${k}`;
}

// bad
const obj = {
    id: 5,
    name: "San Francisco",
};
obj[getKey("enabled")] = true;

// good
const obj = {
    id: 5,
    name: "San Francisco",
    [getKey("enabled")]: true,
};
```

Group your shorthand properties at the beginning of your object declaration.

> Why? It’s easier to tell which properties are using the shorthand.

```javascript
const anakinSkywalker = "Anakin Skywalker";
const lukeSkywalker = "Luke Skywalker";

// bad
const obj = {
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    lukeSkywalker,
    episodeThree: 3,
    mayTheFourth: 4,
    anakinSkywalker,
};

// good
const obj = {
    lukeSkywalker,
    anakinSkywalker,
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    episodeThree: 3,
    mayTheFourth: 4,
};
```

Only quote properties that are invalid identifiers.

> Why? In general we consider it subjectively easier to read. It improves syntax highlighting, and
> is also more easily optimized by many JS engines.

```javascript
// bad
const bad = {
    foo: 3,
    bar: 4,
    "data-blah": 5,
};

// good
const good = {
    foo: 3,
    bar: 4,
    "data-blah": 5,
};
```

Source: [Airbnb style guide](/airbnb.md#objects).

### Spread syntax (`...`)

Source: [developer.mozilla.org][1]

The **spread (`...`)** syntax allows an iterable, such as an array or string, to be expanded in
places where zero or more arguments (for function calls) or elements (for array literals) are
expected. In an object literal, the spread syntax enumerates the properties of an object and adds
the key-value pairs to the object being created.

Spread syntax looks exactly like rest syntax. In a way, spread syntax is the opposite of rest
syntax. Spread syntax "expands" an array into its elements, while rest syntax collects multiple
elements and "condenses" them into a single element. See [rest parameters][2] and [rest
property][3].

Spread syntax can be used when all elements from an object or array need to be included in a new
array or object, or should be applied one-by-one in a function call's arguments list. There are
three distinct places that accept the spread syntax:

-   [Function arguments][4] list (`myFunction(a, ...iterableObj, b)`)
-   [Array literals][5] (`[1, ...iterableObj, '4', 'five', 6]`)
-   [Object literals][6] (`{ ...obj, key: 'value' }`)

Although the syntax looks the same, they come with slightly different semantics.

Only [iterable][7] values, like [`Array`][8] and [`String`][10] can be spread in  [array
literals][9] and argument lists. Many objects are not iterable, including all  [plain
objects][11] that lack a [`Symbol.iterator`][12] method:

```js
const obj = { key1: "value1" };
const array = [...obj]; // TypeError: obj is not iterable
```

[1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax
[2]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters
[3]:
    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#rest_property
[4]:
    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#spread_in_function_calls
[5]:
    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#spread_in_array_literals
[6]:
    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#spread_in_object_literals
[7]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols
[8]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array
[9]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#array_literals
[10]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String
[11]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object
[12]:
    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/iterator

Example:

```js
function sum(x, y, z) {
    return x + y + z;
}

const numbers = [1, 2, 3];

console.log(sum(...numbers));
// Expected output: 6
```

Syntax:

```js
myFunction(a, ...iterableObj, b)
[1, ...iterableObj, '4', 'five', 6]
{ ...obj, key: 'value' }
```

### Rest parameters (`...theArgs`)

Source: [developer.mozilla.org][13]

The **rest parameter** syntax allows a function to accept an indefinite number of arguments as an
array, providing a way to represent [variadic functions][14] in JavaScript.

For example:

```js
function sum(...theArgs) {
    let total = 0;
    for (const arg of theArgs) {
        total += arg;
    }
    return total;
}

console.log(sum(1, 2, 3));
// Expected output: 6
```

Syntax:

```js
function f(a, b, ...theArgs) {
    // …
}
```

A function definition's last parameter can be prefixed with `...`, which will cause all remaining
(user supplied) parameters to be placed within an [`Array`][8] object.

```js
function myFun(a, b, ...manyMoreArgs) {
    console.log("a", a);
    console.log("b", b);
    console.log("manyMoreArgs", manyMoreArgs);
}

myFun("one", "two", "three", "four", "five", "six");

// Console Output:
// a, one
// b, two
// manyMoreArgs, ["three", "four", "five", "six"]
```

A function definition can only have one rest parameter, and the rest parameter must be the last
parameter in the function definition.

```js
function wrong1(...one, ...wrong) {}
function wrong2(...wrong, arg2, arg3) {}
```

The rest parameter is not counted towards the function's [`length`][15] property.

[13]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters
[14]: https://en.wikipedia.org/wiki/Variadic_function
[15]:
    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/length

### Rest property in destructuring assignments

Source: [developer.mozilla.org][16]

You can end a destructuring pattern with a rest property `...rest`. This pattern will store all
remaining properties of the object or array into a new object or array.

```js
const { a, ...others } = { a: 1, b: 2, c: 3 };
console.log(others); // { b: 2, c: 3 }

const [first, ...others2] = [1, 2, 3];
console.log(others2); // [2, 3]
```

The rest property must be the last in the pattern, and must not have a trailing comma.

```js
const [a, ...b] = [1, 2, 3];

// SyntaxError: rest element may not have a trailing comma
// Always consider using rest operator as the last element
```

[16]:
    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#rest_property

## Package managers (npm)

Source: [devhints.io](https://devhints.io/npm)

### Package management

| Command                           | Description                                               |
| --------------------------------- | --------------------------------------------------------- |
| `npm i`                           | Alias for `npm install`                                   |
| `npm install`                     | Install everything in package.json                        |
| `npm install --production`        | Install everything in package.json, except devDependecies |
| ---                               | ---                                                       |
| `npm install lodash`              | Install a package                                         |
| `npm install --save-dev lodash`   | Install as devDependency                                  |
| `npm install --save-exact lodash` | Install with exact                                        |
| ---                               | ---                                                       |
| `npm version 1.2.3`               | Bump the package version to 1.2.3                         |
| `npm version major`               | Bump the major package version by 1 (1.2.3 → 2.0.0)       |
| `npm version minor`               | Bump the minor package version by 1 (1.2.3 → 1.3.0)       |
| `npm version patch`               | Bump the patch package version by 1 (1.2.3 → 1.2.4)       |

`-D` flag is a shorthand for `--save-dev` that saves the package as a dev-dependency.

`--save` is the default as of npm@5. Previously, using `npm install` without `--save` doesn't update
package.json.

### Install names

| Command                              | Description             |
| ------------------------------------ | ----------------------- |
| `npm i sax`                          | NPM package             |
| `npm i sax@latest`                   | Specify tag `latest`    |
| `npm i sax@3.0.0`                    | Specify version `3.0.0` |
| `npm i sax@">=1 <2.0"`               | Specify version range   |
| ---                                  | ---                     |
| `npm i @org/sax`                     | Scoped NPM package      |
| ---                                  | ---                     |
| `npm i user/repo`                    | GitHub                  |
| `npm i user/repo#master`             | GitHub                  |
| `npm i github:user/repo`             | GitHub                  |
| `npm i gitlab:user/repo`             | GitLab                  |
| ---                                  | ---                     |
| `npm i /path/to/repo`                | Absolute path           |
| `npm i ./archive.tgz`                | Tarball                 |
| `npm i https://site.com/archive.tgz` | Tarball via HTTP        |

### Listing

| Command                 | Description                                                       |
| ----------------------- | ----------------------------------------------------------------- |
| `npm list`              | Lists the installed versions of all dependencies in this software |
| `npm list -g --depth 0` | Lists the installed versions of all globally installed packages   |
| `npm view`              | Lists the latest versions of all dependencies in this software    |
| `npm outdated`          | Lists only the dependencies in this software which are outdated   |

### Updating

| Command             | Description                |
| ------------------- | -------------------------- |
| `npm update`        | Update production packages |
| `npm update --dev`  | Update dev packages        |
| `npm update -g`     | Update global packages     |
| ---                 | ---                        |
| `npm update lodash` | Update a package           |

### Removing

| Command         | Description                        |
| --------------- | ---------------------------------- |
| `npm rm lodash` | Remove package production packages |

### Misc features

```bash
# Add someone as an owner
npm owner add USERNAME PACKAGENAME
```

```bash
# list packages
npm ls
```

```bash
# Adds warning to those that install a package of old versions
npm deprecate PACKAGE@"< 0.2.0" "critical bug fixed in v0.2.0"
```

```bash
# update all packages, or selected packages
npm update [-g] PACKAGE
```

```bash
# Check for outdated packages
npm outdated [PACKAGE]
```

## Runtime environment (Node.js)

### How to start a simple Node project

1. Create a repository on Github
2. Clone the repo
3. Add `.gitignore` (example below)

```gitignore
node_modules/
.env
.vscode
```

4. Run `npm init` (takes you through mini questionnaire in CLI and creates file)
5. Run `npm install <your_required_package_name>` to install and add dependencies to the
   `package.json` file

### Package.json

To make package.json files from from scratch:

-   run `npm init` (takes you through mini questionnaire in CLI and creates file)
-   run `npm install <package>` (automatically adds to dependency in the package.json file)

### NVM (node version manager)

> [Node Version Manager is a tool that helps us manage Node versions](https://github.com/nvm-sh/nvm)
> and is a convenient way to install Node. Think of it as npm or Yarn that helps manage Node
> packages, but instead of packages, NVM manages Node versions.

Source: [logrocket](https://blog.logrocket.com/how-switch-node-js-versions-nvm/)

### Displaying a list of Node.js versions

> We can now view all the versions we downloaded so far; currently, we have three Node versions
> installed using NVM.
>
> To see the full list, run the following command:
>
> ```bash
> nvm ls
> ```
>
> The list then appears:

```bash
->     v12.22.5
       v14.19.0
        v18.3.0
         system
default -> 12 (-> v12.22.5)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v18.3.0) (default)
stable -> 18.3 (-> v18.3.0) (default)
lts/* -> lts/gallium (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.12 (-> N/A)
lts/fermium -> v14.19.3 (-> N/A)
lts/gallium -> v16.15.1 (-> N/A)
```

Source: [logrocket](https://blog.logrocket.com/how-switch-node-js-versions-nvm/)

### Switching among Node.js Version

> The best feature about NVM is the ability to easily switch between different Node versions.

```bash
nvm use 18.3.0
```

Source: [logrocket](https://blog.logrocket.com/how-switch-node-js-versions-nvm/)

### Removing a Node.js version

> To remove a version, just run the following command:

```bash
nvm uninstall <the version number>
```

Source: [logrocket](https://blog.logrocket.com/how-switch-node-js-versions-nvm/)

## React.js

Resources:

-   React docs > Learn React (beta) > [Quick Start](https://beta.reactjs.org/learn)
-   React docs > [Hello world](https://reactjs.org/docs/hello-world.html)

### React Elements

React elements are written just like regular HTML elements. You can write any valid HTML element in
React:

```js
<h1>My Header</h1>
<p>My paragraph>
<button>My button</button>
```

Source:
[freeCodeCamp - React cheat sheet for 2022](https://www.freecodecamp.org/news/the-react-cheatsheet/)

Styling: Inline styles are not written as plain strings, but as properties on objects:

```js
<h1 style={{ fontSize: 24, margin: "0 auto", textAlign: "center" }}>My header</h1>
```

### React Components

> We can organize groups of elements into React components.

Here is the basic syntax of a React function component:

1. Component names **must start with a capital letter** (that is, MyComponent, instead of
   myComponent)
2. Components, unlike JavaScript functions, **must return JSX**.

```js
function App() {
    return <div>Hello world!</div>;
}
```

Source:
[freeCodeCamp - React cheat sheet for 2022](https://www.freecodecamp.org/news/the-react-cheatsheet/)

### React Props

React components can accept data passed to them in **objects** called `props`. Props are passed from
the parent component to a child component.

Here we are passing a prop `name` from App to the User component.

```js
function App() {
    return <User name="John Doe" />;
}

function User(props) {
    return <h1>Hello, {props.name}</h1>; // Hello, John Doe!
}
```

Or simpler (if only one attribute):

```js
function App() {
    return <User name="John Doe" />;
}

function User({ name }) {
    return <h1>Hello, {name}!</h1>; // Hello, John Doe!
}
```

Source:
[freeCodeCamp - React cheat sheet for 2022](https://www.freecodecamp.org/news/the-react-cheatsheet/)

### React conditionals

If statement:

```js
function App() {
    const isAuthUser = useAuth();

    if (isAuthUser) {
        // if our user is authenticated, let them use the app
        return <AuthApp />;
    }

    // if user is not authenticated, show a different screen
    return <UnAuthApp />;
}
```

Source:
[freeCodeCamp - React cheat sheet for 2022](https://www.freecodecamp.org/news/the-react-cheatsheet/)

Ternary list:

```js
function App() {
    const isAuthUser = useAuth();

    return (
        <>
            <h1>My App</h1>
            {isAuthUser ? <AuthApp /> : <UnAuthApp />}
        </>
    );
}
```

Source:
[freeCodeCamp - React cheat sheet for 2022](https://www.freecodecamp.org/news/the-react-cheatsheet/)

### React Lists

Lists of React components can be output using the `.map()` function.

`.map()` allows us to loop over arrays of data and output JSX.

```js
function SoccerPlayers() {
    const players = ["Messi", "Ronaldo", "Laspada"];

    return (
        <div>
            {players.map((playerName) => (
                <SoccerPlayer key={playerName} name={playerName} />
            ))}
        </div>
    );
}
```

Source:
[freeCodeCamp - React cheat sheet for 2022](https://www.freecodecamp.org/news/the-react-cheatsheet/)

## EthersJS

These are noob notes for the [EthersJS](https://github.com/ethers-io/ethers.js) web3 library (mostly
notes-to-self). They are incomplete by default.

### Provider (read-only)

Syntax:

```js
new ethers.Contract(address, abi, signerOrProvider);
```

_Source:
[EthersJS > Contract > Creating instances](https://docs.ethers.org/v5/api/contract/contract/#Contract--creating)_

Creates instance of Provider:

```js
const { ethers } = require("ethers");

// Instantiate: Provider
const rpcUrl = "https://forno.celo.org";
const chainId = "42220";
const provider = new ethers.providers.JsonRpcProvider(rpcUrl, chainId);
```

Temporary bug: Some
[issue](https://stackoverflow.com/questions/75385248/typeerror-cannot-read-properties-of-undefined-reading-jsonrpcprovider)
in ethers v6 with `jsonRpcProvider`.

### Signer (read and write)

```js
const { ethers, Wallet } = require("ethers");

// Instantiate: Provider
const rpcUrl = "https://forno.celo.org";
const chainId = "42220";
const provider = new ethers.providers.JsonRpcProvider(rpcUrl, chainId);

// .env file
const dotenv = require("dotenv");
dotenv.config(); // to use .env file

// Instantiate: Signer
const privateKey = process.env.WALLET_PRIVATE_KEY;
const signer = new Wallet(privateKey, provider);
```

### Contract

### Contract functions

Syntax: You can add specific human-readable ABI (simply copy function signature from smart
contract):

```js
const abi = ["function getCurrentReleasedTotalAmount() public view returns (uint256)"];
```

E.g. this smart contract function:

```java
/**
* @dev Calculates the total amount that has already released up to now.
* @return The already released amount up to the point of call.
* @dev The returned amount may vary over time due to locked gold rewards.
*/
function getCurrentReleasedTotalAmount() public view returns (uint256)
```

Example:

```js
// Instantiate: Contract
const contractAddress = process.env.SMART_CONTRACT_ADDRESS;
const abi = ["function getCurrentReleasedTotalAmount() public view returns (uint256)"];
let contract = new ethers.Contract(contractAddress, abi, signer);
```

### Contract variables

To query public variables via ethers, you can simply add getter function in your ABI with
parentheses.

> The compiler automatically creates getter functions for all **public** state variables. For the
> contract given below, the compiler will generate a function called `data` that does not take any
> arguments and returns a `uint`, the value of the state variable `data`.

Source:
[Solidity docs > Getter Functions](https://docs.soliditylang.org/en/v0.8.4/contracts.html#getter-functions),
originally found via [Stack Overflow](https://ethereum.stackexchange.com/a/99587)

E.g.

```js
const abi = ["function totalWithdrawn() public view returns (uint256)"];
// ...
const releaseSchedule = await releaseGoldContract.totalWithdrawn();
// ...
```

For the following public variable:

```java
// Indicates how much of the released amount has been withdrawn so far.
uint256 public totalWithdrawn;
```

Source:
[Celo Monorepo > ReleaseGold.sol](https://github.com/celo-org/celo-monorepo/blob/75c223a1b5296ac0fecdb54fb29c52432cf3fece/packages/protocol/contracts/governance/ReleaseGold.sol#L63-L64)

### Contract structs

Human-readable structs are not currently supported in ethers, but the following workaround makes
them work:

> While struct syntax is not supported, this works for me
>
> ```ts
> const User = "(address user, string email)";
> const abi = [
>     `event registered(${User} user)`,
>     `function getId(${User} user) view returns (uint id)`,
>     `function getUser(uint id) view returns (${User} user)`,
>     `function addUser(${User} user)`,
> ];
> const Relationship = `(${User} a, ${User} b, string relationship)`;
> ```

Source:
[Github > ethers-io > Issues](https://github.com/ethers-io/ethers.js/issues/315#issuecomment-1035675960)

Example:

```js
// Define struct-like variable
const ReleaseSchedule =
    "(uint256 releaseStartTime, uint256 releaseCliff, uint256 numReleasePeriods, uint256 releasePeriod, uint256 amountReleasedPerPeriod)";
// Reference struct-like variable in ABI
const abi = [`function releaseSchedule() public view returns (${ReleaseSchedule})`];
// Query struct
const releaseSchedule = await releaseGoldContract.releaseSchedule();
console.log(releaseSchedule.toString());
```

For the

```java
struct ReleaseSchedule {
    // Timestamp (in UNIX time) that releasing begins.
    uint256 releaseStartTime;
    // Timestamp (in UNIX time) of the releasing cliff.
    uint256 releaseCliff;
    // Number of release periods.
    uint256 numReleasePeriods;
    // Duration (in seconds) of one period.
    uint256 releasePeriod;
    // Amount that is to be released per period.
    uint256 amountReleasedPerPeriod;
}

// Public struct housing params pertaining to releasing gold.
ReleaseSchedule public releaseSchedule;
```

Source:
[Celo Monorepo > ReleaseGold.sol](https://github.com/celo-org/celo-monorepo/blob/75c223a1b5296ac0fecdb54fb29c52432cf3fece/packages/protocol/contracts/governance/ReleaseGold.sol#L21-L32)

### Read-only methods

Querying a contract:

```js
const { ethers } = require("ethers");

// Instantiate: Provider
const rpcUrl = "https://forno.celo.org";
const chainId = "42220";
const provider = new ethers.providers.JsonRpcProvider(rpcUrl, chainId);

// Instantiate: Contract
const contractAddress = process.env.SMART_CONTRACT_ADDRESS;
const abi = ["function getCurrentReleasedTotalAmount() public view returns (uint256)"];
let contract = new ethers.Contract(contractAddress, abi, signer);

// Query function in ReleaseGold.sol contract
const getReleasedAmountReceipt = await releaseGoldContract.getCurrentReleasedTotalAmount();

// Visualize response
console.log(ethers.utils.formatEther(getReleasedAmountReceipt.toString()));
```

Things to learn:

-   [ ] State changing methods
-   [ ] Listening to events
-   [ ] Query historic events
-   [ ] Signing Messages
