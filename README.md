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

## Arithmetic Operators (from [exercism.org](https://exercism.org/tracks/javascript))

Source: [exercism.org](https://exercism.org/tracks/javascript/concepts/arithmetic-operators)

> [!TIP]  
> Most of the concepts and text below are taken from the JavaScript track on
> [exercism.org](https://exercism.org/tracks/javascript). The material is licensed under a MIT
> license, which is included in this repository as well.

### About Arithmetic Operators

JavaScript provides 6 different operators to perform basic arithmetic operations on numbers.

-   `+`: The addition operator is used to find the sum of numbers.

    ```js
    1 + 2; // => 3
    2.5 + 3.9; // => 6.5
    ```

-   `-`: The subtraction operator is used to find the difference between two numbers

    ```js
    19 - 2; // => 17
    7.4 - 1.2; // => 6.2
    ```

-   `*`: The multiplication operator is used to find the product of two numbers

    ```js
    7 * 5; // => 35
    9.2 * 6.3; // => 57.959999999999994
    ```

-   `/`: The division operator is used to divide two numbers. Since JavaScript numbers are always
    floating-point numbers, there is no integer division.

    ```js
    8 / 2; // => 4
    25 / 3; // => 8.333333333333334
    ```

-   `%`: The remainder operator is used to find the remainder of a division performed.

    ```js
    40 % 4; // => 0
    11 % 4; // => 3
    -11 % 4; // => -3
    ```

-   `**`: The exponentiation operator is used to raise a number to a power. It is the equivalent of
    using [`Math.pow()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/pow)

    ```js
    4 ** 3; // => 64
    4 ** 1 / 2; // => 2
    ```

### Order of Operations

When using multiple operators in a line, JavaScript follows an order of precedence as shown
in [this precedence table](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence#table).
To simplify it to our context, JavaScript uses the PEDMAS (Parentheses, Exponents,
Division/Multiplication, Addition/Subtraction) rule we've learnt in elementary math classes.

```js
const result = 3 ** 3 + (9 * 4) / (3 - 1);
// => 3 ** 3 + 9 * 4/2
// => 27 + 9 * 4/2
// => 27 + 18
// => 45
```

### Shorthand Assignment Operators

Shorthand assignment operators are a shorter way of writing code conducting arithmetic operations on
a variable, and assigning the new value to the same variable. For example, consider two
variables `x` and `y`. Then, `x += y` is same as `x = x + y`. Often, this is used with a number
instead of a variable `y`. The 5 other operations can also be conducted in a similar style.

```js
let x = 5;
x += 25; // x is now 30

let y = 31;
y %= 3; // y is now 1
```

## Strings (from [exercism.org](https://exercism.org/tracks/javascript))

Source: [exercism.org](https://exercism.org/tracks/javascript/concepts/strings)

> [!TIP]  
> Most of the concepts and text below are taken from the JavaScript track on
> [exercism.org](https://exercism.org/tracks/javascript). The material is licensed under a MIT
> license, which is included in this repository as well.

### About Strings

A *string* is the JavaScript data type to store text data. There is no separate data type for an
individual character.

### Creating a String

You create a
string [literal](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#literals) by
wrapping the text in single quotes or double quotes. On Exercism, single quotes are used.

```js
"Hello, World!";
"Hello, World!";
```

Some special characters of the text need to be prefixed with a backslash `\`,
see [escape sequences on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#escape_sequences).

```js
const text = "I'm having fun.\nWhat about you?";
console.log(text);
// => I'm having fun.
// => What about you?
```

Besides single and double quotes, you can also create a string by wrapping the text in backticks.
This syntax allows to include single or double quotes and even new lines without the need to escape
them. It also allows to embed expressions,
see [Template Strings](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) for
more details.

```js
`A multi-line string
with 'single quotes'
and "double quotes"`;
```

### Strings as Lists of Characters

A string can be treated as a list of characters where the first character has index `0`. You can
access an individual character of the string using square brackets and the index of the letter you
want to retrieve. Alternatively, there is also the `charAt` method.

```js
"cat"[1];
// => 'a'

"cat".charAt(2);
// => 't'
```

You can determine the number of characters in a string by accessing the `length` property.

```js
"cat".length;
// => 3
```

### Concatenation and Methods

The simplest way to concatenate strings is to use the addition operator `+`.

```js
"I like" + " " + "cats.";
// => "I like cats."
```

Strings provide a lot of helper methods,
see [MDN Docs on String Methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#Instance_methods) for
a full list. The following list shows some commonly used helpers.

-   [`toUpperCase`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase) and [`toLowerCase`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase) \-
    change the case of all characters
-   [`trim`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/Trim) \-
    remove whitespace at the beginning and end
-   [`includes`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/includes), [`startsWith`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/startsWith) and [`endsWith`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith) \-
    determine whether another string is part of the given string
-   [`slice`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice) \-
    extract a section of the string

### Strings are Immutable

Applying the methods above will never change the original string. Instead a new string will be
created and returned. Strings (and other primitive data types) are immutable in JavaScript. That
also means you cannot assign a different character at some index using the bracket syntax shown
above (like you would in arrays).

```js
const str = "cat";
str[1] = "u"; // fails silently
console.log(str);
// => 'cat'
```

## Booleans (from [exercism.org](https://exercism.org/tracks/javascript))

Source: [exercism.org](https://exercism.org/tracks/javascript/concepts/booleans)

> [!TIP]  
> Most of the concepts and text below are taken from the JavaScript track on
> [exercism.org](https://exercism.org/tracks/javascript). The material is licensed under a MIT
> license, which is included in this repository as well.

JavaScript uses `true` and `false` to represent the two truth values of logic.

In JavaScript, for each of the three logical operations (AND, OR and NOT) there is a corresponding
operator: `&&`, `||` and `!`. In general, there are rules regarding the order of the operations and,
in this case, `!` (negation) is applied first, and then `&&` (conjunction) and
then `||` (disjunction).

The order of operations between the operators can be overcome by using an operator with higher
precedence: `( )`, named the 'Grouping operator' or simply said 'parentheses'. As a matter of fact,
the `( )` operator has the highest precedence of all JavaScript operators. More information about
operators precedence is
found [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence).

## Arrays (from [exercism.org](https://exercism.org/tracks/javascript))

Source: [exercism.org](https://exercism.org/tracks/javascript/concepts/arrays)

> [!TIP]  
> Most of the concepts and text below are taken from the JavaScript track on
> [exercism.org](https://exercism.org/tracks/javascript). The material is licensed under a MIT
> license, which is included in this repository as well.

### About Arrays

In JavaScript, an array is a list-like structure with no fixed length which can hold any type of
primitives or objects, even mixed types. It includes the `length` property and also lots
of [useful methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#Instance_methods) for
traversing and mutating the array.

To create an array, add elements between square brackets `[]`. To read from the array, put the index
in square brackets `[]` after the identifier. The indices of an array start at zero.

For example:

```js
const names = ["Jack", "Laura", "Paul", "Megan"];
names[1];
// => Laura
```

Arrays can also be created using the constructor syntax, but for most uses, the array literal syntax
is recommended.

```js
const names = new Array();
names.push("Jack", "Laura", "Paul", "Megan");

names[1];
// => Laura
```

Arrays cannot use `strings` as element indexes but must use integers
([`number`](https://exercism.org/tracks/javascript/concepts/numbers)). Setting or accessing via
non-integers using bracket notation (or dot notation) will not set or retrieve an element from the
array list itself, but will set or access a variable associated with that array's object property
collection. The array's object properties and list of array elements are separate, and the array's
traversal and mutation operations cannot be applied to these named properties.

```js
const names = ["Jack", "Laura", "Paul", "Megan"];
names.length;
// => 4

// Properties can be set on arrays using bracket ['property'] or
// dot .property notation, and this will affect the length, as
// shown below.

names.magician = "Elyse";
names.length;
// => 4

// The property shows up when logging the array, making it seem
// that the property is somehow incorporated in the array.

names;
// => ["Jack", "Laura", "Paul", "Megan", magician: "Elyse"]

// However, be aware. Properties added via non-numeric keys are
// NOT part of the array's internal list, and are not traversed
// or mutated when using one of the traversal or mutation
// operations.

names.forEach((name) => console.log(name));
// => Jack
// => Laura
// => Paul
// => Megan
```

### Deleting items from an array

Arrays in JavaScript are regular `objects`, and items can be deleted using the `delete` keyword.
However, this does not change the *length* of the array and leaves a hole of `empty`. In other
languages, this is similar to a sparse array. The `empty` holes are skipped when using traversal or
mutation operations.

```
const names = ['Jack', 'Laura', 'Paul', 'Megan'];
delete names[1];

names;
// =>  ["Jack", empty, "Paul", "Megan"]

names.length;
// => 4

names.forEach((name) => console.log(name));
// => Jack
// => Paul
// => Megan

```

If there should be no holes, and if the `length` should reflect the number of items that will be
traversed or mutated, use `splice` instead.

> The `splice()` method changes the contents of an array by removing or replacing existing elements
> and/or adding new elements in place.

Source:
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

For example:

```js
const names = ["Jack", "Laura", "Paul", "Megan"];
names.splice(1, 1);

names;
// =>  ["Jack", "Paul", "Megan"]

names.length;
// => 3

names.forEach((name) => console.log(name));
// => Jack
// => Paul
// => Megan
```

### Array length can be mutated

The `length` property of an array is connected to the list of items the array holds. It can be
mutated. When the length is increased, it creates `empty` holes, that are not considered when
traversing or mutating the array. When the length is decreased, it *removes* the elements at the end
of the array.

```js
const names = ["Jack", "Laura", "Paul", "Megan"];
names.length = 6;

names;
// => ["Jack", "Laura", "Paul", "Megan", empty × 2]

names.length = 2;
// =>  ["Jack", "Laura"]
```

### Checking if something is an Array

Because arrays are `objects`, `typeof names` gives `"object"`. To check if something is an Array,
use `Array.isArray`:

```js
const names = ["Jack", "Laura", "Paul", "Megan"];

typeof names;
// => "object"

Array.isArray(names);
// => true

const object = {};
Array.isArray(object);
// => false
```

You might be tempted to use `names instanceof Array`, and that can work, but not under all
circumstances. Read [this article](https://web.mit.edu/jwalden/www/isArray.html) for more
information.

### Array Methods

Some of
the [methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) that
are available on every Array object can be used to add or remove from the array. Here are a few of
them:

#### push

> The `push()` method adds one or more elements to the end of an array and returns the new length of
> the array.

Source:
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)

```js
const names = ["Jack", "Laura", "Paul", "Megan"];
names.push("Jill"); // => 5
names;
// => ['Jack', 'Laura', 'Paul', 'Megan', 'Jill']
```

#### pop

> The `pop()` method removes the last element from an array and returns that element. This method
> changes the length of the array.

Source:
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)

```js
const names = ["Jack", "Laura", "Paul", "Megan"];
names.pop(); // => 'Megan'
names;
// => ['Jack', 'Laura', 'Paul']
```

#### shift

> The `shift()` method removes the first element from an array and returns that removed element.
> This method changes the length of the array.

Source:
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)

```js
const names = ["Jack", "Laura", "Paul", "Megan"];
names.shift(); // => 'Jack'
names;
// => ['Laura', 'Paul', 'Megan']
```

#### unshift

> The unshift() method adds one or more elements to the beginning of an array and returns the new
> length of the array.

Source:
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift)

```js
const names = ["Jack", "Laura", "Paul", "Megan"];
names.unshift("Jill"); // => 5
names;
// => ['Jill', 'Jack', 'Laura', 'Paul', 'Megan']
```

#### splice

> The splice() method changes the contents of an array by removing or replacing existing elements
> and/or adding new elements in place. This method returns an array containing the deleted elements.

Source:
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

```js
const names = ["Jack", "Laura", "Paul", "Megan"];
names.splice(2, 1, "Jill"); // => ['Paul']
names;
// => ['Jack', 'Laura', 'Jill', 'Megan']
```

## Conditionals (from [exercism.org](https://exercism.org/tracks/javascript))

Source: [exercism.org](https://exercism.org/tracks/javascript/concepts/conditionals)

> [!TIP]  
> Most of the concepts and text below are taken from the JavaScript track on
> [exercism.org](https://exercism.org/tracks/javascript). The material is licensed under a MIT
> license, which is included in this repository as well.

### General Syntax

A common way to conditionally execute logic in JavaScript is the if-statement. It consists of
the `if` keyword, a condition wrapped in round brackets and a code block wrapped in curly brackets.
The code block will only be executed if the condition evaluates to `true`.

```js
if (condition) {
    // code that is executed if "condition" is true
}
```

It can be used stand-alone or combined with the `else` keyword.

```js
if (condition) {
    // code that is executed if "condition" is true
} else {
    // code that is executed otherwise
}
```

### Nested If-Statements

To nest another condition into the `else` statement you can use `else if`. Note that there is
no `elseif` keyword in JavaScript. Instead, write `else` followed by another `if` statement.

```js
if (condition1) {
    // code that is executed if "condition1" is true
} else if (condition2) {
    // code that is executed if "condition2" is true
    // but "condition1" was false
} else {
    // code that is executed otherwise
}
```

Theoretically, you can nest as many additional conditions as you want. In practice you would use
a [`switch` statement](https://exercism.org/tracks/javascript/concepts/conditionals-switch) instead
in these cases.

```js
if (condition1) {
    // ...
} else if (condition2) {
    // ...
} else if (condition3) {
    // ...
} else if (condition4) {
    // ...
} else {
    // ...
}
```

### Condition

When constructing complex conditions, refer to
the [operator precedence table](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence#table) to
avoid unnecessary brackets.

```js
if (num >= 0 && num < 1) {
  // ...
}

// The inner brackets are obsolete because relational operators
// have higher precedence than logical operators.
<!-- prettier-ignore-start -->

if ((num >= 0) && (num < 1)) {
  // ...
}
<!-- prettier-ignore-end -->
```

Also, consider using additional variables to make the code more readable.

```js
const isPositive = num >= 0;
const isSmall = num < 1;
if (isPositive && isSmall) {
    // ...
}
```

In JavaScript, the condition does not have to be of type boolean. If any other type than boolean is
provided in a boolean context like the if-statement, JavaScript will implicitly convert the value to
boolean. Refer to
the [type conversion concept](https://exercism.org/tracks/javascript/concepts/type-conversion) for
details on which values are *truthy* and *falsy*, respectively.

```js
const num = 4;
if (num) {
    // this code block will be executed because 4 is truthy
}
```

### Short-Hand Notations

If you only want to execute one statement in the code block for `if` or `else`, it is possible in
JavaScript to omit the curly brackets.

```js
if (condition) doSomething();

// or

if (condition) doSomething();
```

This is sometimes used when checking for an error condition for example. In general, it is not
recommended because it is easy to forget to add the brackets back in when adding a second statement
that should depend on the same condition.

When writing functions, it is a common pattern to omit the `else` block and use an early `return` in
the `if` block instead. In many cases, this reduces *nesting* and makes the code more readable and
easier to follow.

```js
function checkNumber(num) {
    let message = "";

    if (num === 0) {
        message = "You passed 0, please provide another number.";
    } else {
        message = "Thanks for passing such a nice number.";
    }

    return message;
}

// Can also be written as ...
function checkNumber(num) {
    if (num === 0) {
        return "You passed 0, please provide another number.";
    }

    return "Thanks for passing such a nice number.";
}
```

## Comparison (from [exercism.org](https://exercism.org/tracks/javascript))

Source: [exercism.org](https://exercism.org/tracks/javascript/concepts/comparison)

> [!TIP]  
> Most of the concepts and text below are taken from the JavaScript track on
> [exercism.org](https://exercism.org/tracks/javascript). The material is licensed under a MIT
> license, which is included in this repository as well.

### Comparing Numbers

In JavaScript numbers can be compared using the following relational and equality operators.

| Comparison             | Operator  |
| ---------------------- | --------- |
| Greater than           | `a > b`   |
| Greater than or equals | `a >= b`  |
| Less than              | `a < b`   |
| Less than or equals    | `a <= b`  |
| (Strict) Equals        | `a === b` |
| Not (strict) equals    | `a !== b` |

The result of the comparison is always a boolean value, so either `true` or `false`.

```js
1 < 3;
// => true

2 !== 2;
// => false

1 === 1.0;
// => true
// All numbers are floating-points, so this is different syntax
// for the exact same value.
```

### Comparing Strings

In JavaScript, the comparison operators above can also be used to compare strings. In that case, a
dictionary (lexicographical) order is applied. You can find a list of the exact order of all the
characters [here](https://www.fileformat.info/info/charset/UTF-16/list.htm).

```js
"Apple" > "Pear";
// => false

"a" < "above";
// => true

"a" === "A";
// => false
```

You need to be careful when you compare two variables that appear to contain numeric values but are
of type string. Due to the dictionary order, the result will not be the same as comparing values of
type `Number`.

```js
10 < 2;
// => false

"10" < "2";
// => true (because "1" comes before "2")
```

Another way to compare strings is
the [localeCompare](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare) method.
It allows setting a variety
of [options](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/Collator/Collator#parameters) to
adjust the way strings are compared.

### Strict Equality

You might wonder about the three equal signs for checking equality in JavaScript. `===` represents
the check for *strict equality* which means that no type conversion is performed and values of
different types are always unequal.

```js
"3" === 3;
// => false
// The value on the left has type string, the value on the right
// has type number.

1 === 1n;
// => false
// The value on the left has type number, the value on the right
// has type bigint.
```

Using `===` and `!==` is the recommended way of checking equality in JavaScript.

### Avoiding Implicit Type Conversion

There is also `==` and `!=` which represents checking for *loose equality*. You should avoid it
because it will apply implicit type conversion before performing the comparison. The outcomes in
these cases are hard to predict and sometimes not what you would expect. You can read more about
it [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality).

```js
0 == false;
// => true
```

In theory, you can also compare values of different types (e.g., `"1" < 2`). Then the values will be
implicitly converted to determine whether the result is true or false. Just as checking for loose
equality, this is also not recommended for the same reason as mentioned above.

What should you do instead? You can
apply [explicit type conversion](https://exercism.org/tracks/javascript/concepts/type-conversion).
With that, you can then ensure values have the correct type before performing the comparison. Then
your code will be easier to understand and less error-prone.

## Increment/Decrement (from [exercism.org](https://exercism.org/tracks/javascript))

Source: [exercism.org](https://exercism.org/tracks/javascript/concepts/increment-decrement)

> [!TIP]  
> Most of the concepts and text below are taken from the JavaScript track on
> [exercism.org](https://exercism.org/tracks/javascript). The material is licensed under a MIT
> license, which is included in this repository as well.

JavaScript has an increment and a decrement operator. They modify a variable in place. `++` adds one
to a number, `--` subtracts one from a number.

```js
let i = 3;
i++;
// i is now 4

let j = 0;
j--;
// j is now -1
```

The example above shows the postfix increment/decrement operator. In this case the operator is
placed after the variable and the return value of the expression is the value of the
variable *before* the increase/decrease.

```js
let a = 3;
let b = a++;
// a = 4
// b = 3
```

There is also the prefix variant where the operator is placed before the variable. Then the return
value is the value of the variable *after* the increase/decrease.

```js
let a = 3;
let b = ++a;
// a = 4
// b = 4
```

## For Loops (from [exercism.org](https://exercism.org/tracks/javascript))

Source: [exercism.org](https://exercism.org/tracks/javascript/concepts/for-loops)

> [!TIP]  
> Most of the concepts and text below are taken from the JavaScript track on
> [exercism.org](https://exercism.org/tracks/javascript). The material is licensed under a MIT
> license, which is included in this repository as well.

### General Syntax

The for loop is one of the most commonly used statements to repeatedly execute some logic. In
JavaScript, it consists of the `for` keyword, a *header* wrapped in round brackets and a code block
that contains the *body* of the loop wrapped in curly brackets.

```js
for (initialization; condition; step) {
    // code that is executed repeatedly as long as the condition is
    // true
}
```

### Header

The initialization usually sets up a counter variable, the condition checks whether the loop should
be continued or stopped and the step increments the counter at the end of each repetition. The
individual parts of the header are separated by semicolons.

```js
const list = ["a", "b", "c"];
for (let i = 0; i < list.length; i++) {
    // code that should be executed for each item in the array
}
```

All three parts of the header are optional. This is rarely utilized in practice. You can read more
about this in
the [MDN documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for#optional_for_expressions).

### Nested For Loops

For loops can be nested, for example to iterate over nested
(multi-dimensional) [arrays](https://exercism.org/tracks/javascript/concepts/arrays). Make sure to
set up a different counter variable for each loop.

```js
const coords = [
    [1, 2],
    [4, 7],
    [10, -3],
];

for (let i = 0; i < coords.length; i++) {
    for (let j = 0; j < coords[i].length; j++) {
        // do something with coords[i][j]
    }
}
```

### Break, Continue and Labels

Inside a loop body, you can use the `break` keyword to stop the execution of the loop entirely. In
contrast, the keyword `continue` only stops the execution of the current iteration and continues
with the next one.

When working with nested loops, `break` and `continue` always apply to the innermost loop by
default. You can use labels to change that behavior. A label is an identifier name followed by a
colon. It is placed in front of (or above) the loop. Such a label can then be combined
with `break` or `continue` to define to which loop the statement should apply.

```js
outer: for (let i = 0; i < 10; i++) {
    for (let j = 0; j < 10; j++) {
        // ...
        if (j > 5) {
            continue outer;
        }
        // ...
    }
}
```

### Loop Counter and Asynchronous Code

You need to be careful to correctly declare your counter variable when dealing
with [asynchronous code](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Concepts).
Let us look at an example where we sum up the counter variable asynchronously
with `setTimeout` after waiting for 1s. When declaring the counter with `var` or with `let` but
outside the header, the code does not yield the expected result.

```js
let sum = 0;
for (var i = 1; i < 4; i++) {
    setTimeout(function () {
        sum += i;
    }, 1000);
}
// eventually sum will be 12 instead of 6 (1+2+3)
```

```js
let sum = 0;
let i;
for (i = 1; i < 4; i++) {
    setTimeout(function () {
        sum += i;
    }, 1000);
}
// same here, eventually sum will be 12 instead of 6 (1+2+3)
```

In these two cases `i` is not scoped to one specific loop iteration. By the time the sum is
calculated, `i` already reached its final value `4` which leads to the sum being `4 + 4 + 4 = 12`.

This problem can be avoided by declaring the counter with `let` inside the header. This has the
special effect that each iteration gets its very own variable `let i` that is scoped to exactly that
one execution of the loop body. So when the sum is calculated later,
each [callback](https://exercism.org/tracks/javascript/concepts/callbacks) function refers to their
own variable `i` that still holds the correct value.

```js
let sum = 0;
for (let i = 1; i < 4; i++) {
    setTimeout(function () {
        sum += i;
    }, 1000);
}
// eventually sum will be 6 (1+2+3) as expected
```

## While Loops

(from [exercism.org](https://exercism.org/tracks/javascript))

Source: [exercism.org](https://exercism.org/tracks/javascript/concepts/while-loops)

> [!TIP]  
> Most of the concepts and text below are taken from the JavaScript track on
> [exercism.org](https://exercism.org/tracks/javascript). The material is licensed under a MIT
> license, which is included in this repository as well.

### General Syntax

With a while loop, you can execute code repeatably as long as a certain condition is fulfilled.

It is written with the `while` keyword followed by a condition wrapped in round brackets and a code
block that contains the *body* of the loop wrapped in curly brackets.

```js
while (condition) {
  // code that is executed repeatedly as long as the condition is
  // true
}
```

JavaScript also has a do-while loop. Here the condition is checked after the loop body was executed.
This is useful when the condition depends on evaluations done in the body.

```js
do {
  // The code here will always be executed once and then
  // repeatedly while the condition is true.
} while (condition);
```

### Break

Inside a loop body, you can use the `break` keyword to stop the execution of the loop entirely. This
is often used in combination with `true` as the condition. With that, you can control when the loop
should stop from any place inside the loop body.

```js
const winningNumber = 7;

while (true) {
  const num = readUserGuess();
  if (num === winningNumber) {
    break;
  }
}
```

The `break` keyword cannot be used inside a function that is nested in the loop, see
the [MDN documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/break#break_within_functions) for
an example.

### Continue

In contrast to `break`, the keyword `continue` only stops the execution of the current iteration and
continues with the next one. With `continue` you can often avoid wrapping big parts of the loop body
in an if-statement.

```js
let i = 0;

while (i < 100) {
  i = i + 2;

  if (i % 3 === 0) {
    continue;
  }

  // The code here will only be executed when i was not divisible
  // by 3 in the check above.
}
```

### Infinite Loops

A loop that is (theoretically) repeated forever is created when the loop condition is always
fulfilled and no break or return statement is reached in the loop body. The execution has to be
terminated from the outside. Depending on the environment in which such code runs, this will be done
automatically or needs manual intervention.

```js
let i = 0;

while (i < 100) {
  if (i % 3 === 0) {
    continue;
  }

  i = i + 2;
}

// This loop runs forever since the variable i does not change
// anymore after it is divisible by 3 the first time.
```

Spotting infinite loops might seem trivial in this toy example, but is not always that easy with
more complex code. It is good practice to thoroughly think about whether your condition eventually
becomes false or whether your break or return statement is reached.

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
