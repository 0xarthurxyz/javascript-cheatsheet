# Other Notes

## JavaScript for pythonistas

|                    | Python                   | JavaScript                    |
|--------------------|--------------------------|-------------------------------|
| Code Editor / IDE  | PyCharm, VS Code         | Atom, VS Code, WebStorm       |
| Code Formatter     | black                    | Prettier                      |
| Dependency Manager | Pipenv, poetry           | bower (deprecated), npm, yarn |
| Documentation Tool | Sphinx                   | JSDoc, sphinx-js              |
| Interpreter        | bpython, ipython, python | node                          |
| Library            | requests, dateutil       | axios, moment                 |
| Linter             | flake8, pyflakes, pylint | eslint, tslint                |
| Package Manager    | pip, twine               | bower (deprecated), npm, yarn |
| Package Registry   | PyPI                     | npm                           |
| Package Runner     | pipx                     | npx                           |
| Runtime Manager    | pyenv                    | nvm                           |
| Scaffolding Tool   | cookiecutter             | cookiecutter, Yeoman          |
| Test Framework     | doctest, nose, pytest    | Jasmine, Jest, Mocha          |
| Web Framework      | Django, Flask, Tornado   | Angular, React, Vue.js        |

<!-- Hyperlinks -->

[realpython javascript vs python]: https://realpython.com/python-vs-javascript/#ecosystem

## Lama Dev - Simple JS projects

Github: 0xarthurxyz/[vanilla-js-css-html][lama dev 3 projects github]

Source: Youtube - [3 Javascript Projects Every Beginner Should Build][lama dev 3 projects]

[![](https://img.youtube.com/vi/uDeb2iwZMkA/0.jpg)](https://www.youtube.com/watch?v=uDeb2iwZMkA)

```css
display: flex
```

> In the flex layout model, the children of a flex container can be laid out in any direction, 
> and can "flex" their sizes, either growing to fill unused space or shrinking to avoid overflowing 
> the parent. Both horizontal and vertical alignment of the children can be easily manipulated.

Source: [MDN docs][mdn display flex]

<!-- Hyperlinks -->

[lama dev 3 projects github]: https://github.com/0xarthurxyz/vanilla-js-css-html
[lama dev 3 projects]: https://www.youtube.com/watch?v=uDeb2iwZMkA&t=1620s
[mdn display flex]: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout

## Dynamic table libraries

> + [React-data-grid][React-data-grid] (very impressive)
> + [Rsuite-table][Rsuite-table] (very impressive)
> + [react-table][react-table]
> + [Material-ui-table][Material-ui-table]
> + [React-bootstrap-table][React-bootstrap-table]
> + [ka-table][ka-table]
> + [tanStack table][tanStack table]

Source: [copycat.dev - react table][copycat dev]

## `react-data-grid` (npm package)

Demo: https://adazzle.github.io/react-data-grid/#/common-features

Code examples: 

+ All features: https://github.com/adazzle/react-data-grid/blob/main/website/demos/AllFeatures.tsx
+ Common features: https://github.com/adazzle/react-data-grid/blob/main/website/demos/CommonFeatures.tsx

<!-- Hyperlinks -->

[copycat dev]: https://www.copycat.dev/blog/react-table/
[React-data-grid]: https://adazzle.github.io/react-data-grid/#/common-features
[react-table]: https://react-table-v7.tanstack.com/docs/installation
[Material-ui-table]: https://material-table.com/#/docs/get-started
[Rsuite-table]: https://rsuitejs.com/components/table/
[React-bootstrap-table]: https://react-bootstrap-table.github.io/react-bootstrap-table2/
[ka-table]: https://ka-table.com/
[tanStack table]: https://tanstack.com/table/v8/docs/guide/introduction

## CSS Dot Notation Naming Convention

Source: [Stack Overflow][stack overflow css dot notation]

A dot in css is for what is called a class.

They can be called almost anything, for example in your CSS you would create a class and add 
style for it (in this case, I'm making the background black);

```css
.my-first-class {
    background-color: #000;
    ...
}
```

and to apply this class to an HTML element, you would do the following

```html
<body class="my-first-class">
    ...
</body>
```

this would mean the body of the page would become black.

Now, you can create classes for CSS style or you can reference HTML elements directly, 
for example (CSS again);

```css
body {
    background-color: #000;
}
```

would directly reference the `<body>` element on the page and do the same again.

The main difference between the two is that CSS classes are reusable. 
By comparison, referencing the HTML tag directly will affect all tags on the page 
(that are the same), for example (CSS again);

```css
body {
    background-color: #000;
}

.my-first-class {
    background-color: #FFF;
}
```

and now for some HTML;

```html
<body>
    <p class="my-first-class">This is the first line</p>
    <p class="my-first-class">This is the second line</p>
</body>x
```

<!-- Hyperlinks -->

[stack overflow css dot notation]: https://stackoverflow.com/a/25174773

## Learning Functional Programming with JavaScript - JSUnconf

Source: [Learning Functional Programming with JavaScript][jsconf functional programming] - Anjana 
Vakil - JSUnconf

[![](https://img.youtube.com/vi/e-5obm1G_FY/0.jpg)](https://www.youtube.com/watch?v=e-5obm1G_FY)

Slides:

Avoid imperative style (for, while, if, else, etc.), use functions instead (input -> output)

Not functional:

```js
var name = "Anjana";
var greeting = "Hi, I’m ";
console.log(greeting + name);
// => “Hi, I’m Anjana”
```

Functional:

```js
function greet(name) {
    return "Hi, I’m ” + name";
}
greet("Anjana");
// => “Hi, I’m Anjana”
```

Avoid side effects, use pure functions.

Not pure:

```js
var name = "Anjana";
function greet() {
    console.log("Hi, I’m " + name);
}
```

Pure:
    
```js
function greet(name) {
    return "Hi, I’m " + name;
}
```

Use higher-order functions (functions that take functions as arguments or return functions)

```js
function makeAdjectifier(adjective) {
    return function (string) { 
        return adjective + “ ” + string;
    };
}

var coolifier = makeAdjectifier(“cool”);

coolifier(“conference”);  
// => “cool conference”
```

Avoid loops, use map, filter, reduce

Avoid mutability (changing objects in place), use immutable data

Mutable:

```js
var rooms = [“H1”, “H2”, “H3”];

rooms[2] = “H4”;

rooms;
// => ["H1", "H2", "H4"]
```

Immutable:

```js
var rooms = [“H1”, “H2”, “H3”];
var newRooms = rooms.map(function (rm) {  
    if (rm === “H3”) { 
        return “H4”; 
    }  else { 
        return rm; 
    }
});

newRooms; 
// => ["H1", "H2", "H4"]
rooms; 
// => ["H1", "H2", "H3"]
```

Consider using persistent data structures for efficient immutability to avoid copying 
immutable data all the time. 

For example, ideal hash trees use nodes to share data between versions. 
[Ideal Hash Trees][ideal hash trees paper] were invented by Phil Bagwell around 2001.

<img src="assets/images/ideal-hash-tree-example.png" width="450">

Some libraries for functional programming in JS:

+   [Mori][mori library]
+   [Immutable.js][immutable js library]
+   [Underscore][underscore library]
+   [Lodash][lodash library]
+   [Ramda][ramda library]

<!-- Hyperlinks -->

[ideal hash trees paper]: https://lampwww.epfl.ch/papers/idealhashtrees.pdf
[jsconf functional programming]: https://www.youtube.com/watch?v=e-5obm1G_FY
[mori library]: http://swannodette.github.io/mori
[immutable js library]: https://facebook.github.io/immutable-js/
[underscore library]: http://underscorejs.org
[lodash library]: https://lodash.com
[ramda library]: http://ramdajs.com

## What the heck is the event loop anyway? | Philip Roberts | JSConf EU

Source: [What the heck is the event loop anyway?][jsconf event loop]

[![](https://img.youtube.com/vi/8aGhZQkoFbQ/0.jpg)](https://www.youtube.com/watch?v=8aGhZQkoFbQ)

+   Transcript: [What the heck is the event loop anyway?][jsconf event loop transcript]
+   LinkedIn: [Philip Roberts][philip roberts linkedin]

(Reminder) You always put things on top of the stack and pop them of the top of the stack.

With JS, function calls are added to the stack and removed from the stack after they `return`.

Callbacks sitting in the callback queue only get back on the call stack when the call stack is 
empty.

JS runtime isn't only the engine (V8), it also has a heap (for memory), a call stack, 
external WebAPIs you can call (or Node APIs in C++ for backend apps), a callback queue, and 
an event loop.

<img src="assets/images/js-architecture-event-loop.png" width="450">

`setTimeout` is not guaranteed to run after the specified time, it is guaranteed to run at least 
after the time specified if the call stack is empty ("it's a minimum delay not maximum delay").
Example with `setTimeout(..., 0)` that only executes when the call stack is empty again and not
after 0ms (immediately).

<!-- Hyperlinks -->

[jsconf event loop]: https://www.youtube.com/watch?v=8aGhZQkoFbQ
[jsconf event loop transcript]: https://2014.jsconf.eu/speakers/philip-roberts-what-the-heck-is-the-event-loop-anyway.html
[philip roberts linkedin]: https://www.linkedin.com/in/--philip-roberts--/?originalSubdomain=uk

## Franziska Hinkelmann: JavaScript engines - how do they even? | JSConf EU

+   Article: [Medium - Understanding V8’s Bytecode][v8 bytecode article]
+   Slides (`.key`): [Presentations/JSConfEU2017.key][jsconf v8 slides]
+   Slides (`.pdf`): [JavaScript Engines - how do they even?][jsconf v8 pdf slides]
+   Video: [JavaScript engines - how do they even? | JSConf EU][jsconf v8 video]

[![](https://img.youtube.com/vi/p-iiEDtpy6I/0.jpg)](https://www.youtube.com/watch?v=p-iiEDtpy6I)

JS is just in time compiled (JIT) which means the code is not first compiled and then run,
but compiled step by step as the execution happens.

<img src="assets/images/jit-vs-aot-compiling.png" width="400">

Because JS is dynamically typed, the compiler can't know the types of variables ahead of time,
which is typically required to optimise the code for execution. So instead, JS engines optimise
code that is run often ("hot code") and deoptimise code that is run rarely.

<img src="assets/images/recompile-decompile-hot-code.png" width="400">

Essentially JS code is (1) parsed, (2) turned into an [abstract syntax tree (AST)][AST wikipedia],
(3) compiled into ("average") assembly machine code (baseline compiler), and (4) optimised into 
more performant machine code (optimising compiler) when the code is run often.


<img src="assets/images/optimise-deoptimise-machine-code.png" width="400">

Because of this, changing types often makes it harder for the compiler to optimise code.
So the "optimizing compiler uses previously seen type information - don’t change types!".
To get the best performance out code, consider writing code that looks like statically typed.

Computed names in object literal definitions

+   ES5

    ```js
    function foo() {
        let o = {};
        o[x] = 1;
        return o;
    }
    ```

+   ES6

    ```js
    function foo() {
        return {[x]: 1};
    }
    ```

Inspect compiler code with Node.js or Chrome:

+   `—print-opt-code`: code generated by optimizing compiler
+   `—print-bytecode`: bytecode generated by interpreter
+   `—trace-ic`: different object types a call sites encounters
+   `—trace-opt` and `—trace-deopt`: which functions are (de)optimized

<!-- Hyperlinks -->

[AST wikipedia]: https://en.wikipedia.org/wiki/Abstract_syntax_tree
[v8 bytecode article]: https://medium.com/dailyjs/understanding-v8s-bytecode-317d46c94775
[jsconf v8 slides]: https://github.com/fhinkel/Presentations/blob/main/JSConfEU2017.key
[jsconf v8 pdf slides]: /assets/pdf/JSConfEU2017-v8-fhinkel.pdf
[jsconf v8 video]: https://www.youtube.com/watch?v=p-iiEDtpy6I\

## Adopting Typescript at Scale - Brie Bunge | JSConf Hawaii 2019

+   YouTube: [Adopting Typescript at Scale][jsconf adopting typescript video]

[![](https://img.youtube.com/vi/P-J9Eg7hJwE/0.jpg)](https://www.youtube.com/watch?v=P-J9Eg7hJwE)

Parameter types:

```js
function greet(name) {
    return `Hello, ${name}!`;
}
```

```ts
function greet(name: string) {
    return `Hello, ${name}!`;
}

greeter('JSConf Hawaii'); // compiles fine
greeter(['JSConf', 'Hawaii']); // compile error
```

Types of other objects:

```ts
interface Person {
    firstName: string;
    lastName: string;
}

function greet(person: Person) {
    return `Hello, ${person.firstName} ${person.lastName}!`;
}
```

Great editor integrations with autocompletion and typechecking.


At Airbnb they have 2m+ lines of JS code and 100+ internal npm packages.
1300+ engineers of which 200+ are frontend engineers.

Reasons they were interested in TS:

+   Fewer bugs (which stand in the way of helping users)
+   Better developer experience
+   end-to-end type safety (from backend API to frontend app)

Type script declaration files (`.d.ts`) are used to describe the shape of JS code.
This helped Airbnb circumvent the circular dependency problem between JS and TS code, 
e.g. the repo depending on npm packages that are written in JS (i.e. without type safety).

```ts
// .d.ts file
export default function greeter(name: string): string;
```

```js
// .js file
export default function greeter(name) {
    return `Hello, ${name}!`;
}
```

Declaration files are handy because they can be shared across multiple repos. 
This is how types for React (`@types/react`) and others (`@types/*`) come about. 
They are maintained by a community on Github [`DefinitelyTyped/DefinitelyTyped`][types repo github].

<img src="assets/images/npm-ts-declaration-file.png" width="300">

<img src="assets/images/react-ts-types.png" width="300">

This works well for public npm packages, for internal npm packages they mirrored the 
DefinitelyTyped pattern and saved all internal types at `@airbnb-types/*` 
(see GitHub repo: [brieb/types-starter][types starter github repo]).

Brie read tons of postmortems to see if JavaScript related problems could have been prevented
with TypeScript. Examples:

+   Missing parameters in function calls can be prevented with "Expected 1 argument, but got 0"
+   Strict null-checking 
    +   "Object is possibly 'null' or 'undefined'"
    +   "Cannot invoke an object which is possibly 'null' or 'undefined'"
+   Type mismatches "Type '...' is not assignable to type '...'"

Brie found that 38% of Airbnb bugs were preventable with TypeScript according to postmortems.

[ASTExplorer.net][AST explorer js to ts] for exploring ASTs of JS and TS code.

Some tips on large migrations:

+   gather evidence and support
+   gradually introduce change
+   provide a migration path

<!-- Hyperlinks -->

[AST explorer js to ts]: https://astexplorer.net/
[types starter github repo]: https://github.com/brieb/types-starter
[types repo github]: https://github.com/DefinitelyTyped/DefinitelyTyped
[jsconf adopting typescript video]: https://www.youtube.com/watch?v=P-J9Eg7hJwE

## Some things to learn

Must have:

+   Basic Jest testing framework
+   Basic JS programming patterns (e.g. closures, promises, async/await)
+   TypeScript typing function parameters and return types
+   Basic Express.js
+   Basic MongoDB (mongoose)

Nice to have:

+   New programming patterns (differences between ES5 vs ES6)
+   Understand what event loop looks like and how it works (browser and node)
+   Basic React

## How I Learned To Stop Worrying And Trust The Compiler - Felix Rieseberg - Node Summit

+   YouTube: [How I Learned To Stop Worrying And Trust The Compiler][TS Node Sumit Felix Slack]

[![](https://img.youtube.com/vi/mgTenYbX2Kw/0.jpg)](https://www.youtube.com/watch?v=mgTenYbX2Kw)

`tsc -init` to initialise a typescript project. This simply creates a `tsconfig.json` file.

<img src="assets/images/ts-config-example.png" width="500">

`tsc -w` to watch for changes and compile TS to JS automatically (so you can see changes as you go).

<img src="assets/images/ts-watch-example.png" width="500">

Example used in the talk:

```ts
// demo.ts
interface Person {
    name: string;
    age: number;
}

class PersonManager {
    hello: string;

    constructor() {
        this.hello = 'foo';
    }
}

function sortPeople(input = Person[] = []) {
    const result = input.slice(0);

    result.sort((a, b) => {
        return a.name.localeCompare(b.name);
    });

    return result;
}

sortPeople(5); // Error: Argument of type '5' is not assignable to parameter of type 'any []'
```

TypeScript has a very neat built-in definition feature ("peek definition") for vanilla JS functions 
so you don't have to look up the JS documentation in the browser.

<img src="assets/images/TS-definitions-documentation.png" width="500">

<img src="assets/images/TS-definition-example.png" width="500">

<!-- Hyperlinks -->

[TS Node Sumit Felix Slack]: https://www.youtube.com/watch?v=mgTenYbX2Kw