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

+   Mori (http://swannodette.github.io/mori)
+   Immutable.js (https://facebook.github.io/immutable-js/)
+   Underscore (http://underscorejs.org)
+   Lodash (https://lodash.com)
+   Ramda (http://ramdajs.com)

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

## Adopting Typescript at Scale - Brie Bunge | JSConf Hawaii 2019

+   Video: YouTube - [Adopting Typescript at Scale][jsconf adopting typescript video]

[![](https://img.youtube.com/vi/5kOzF5KpVY0/0.jpg)](https://www.youtube.com/watch?v=5kOzF5KpVY0)

[![](https://img.youtube.com/vi/P-J9Eg7hJwE/0.jpg)](https://www.youtube.com/watch?v=P-J9Eg7hJwE)

<!-- Hyperlinks -->

[realpython javascript vs python]: https://realpython.com/python-vs-javascript/#ecosystem
[ideal hash trees paper]: https://lampwww.epfl.ch/papers/idealhashtrees.pdf
[jsconf functional programming]: https://www.youtube.com/watch?v=e-5obm1G_FY
[jsconf event loop transcript]: https://2014.jsconf.eu/speakers/philip-roberts-what-the-heck-is-the-event-loop-anyway.html
[philip roberts linkedin]: https://www.linkedin.com/in/--philip-roberts--/?originalSubdomain=uk
[jsconf event loop]: https://www.youtube.com/watch?v=8aGhZQkoFbQ
[v8 bytecode article]: https://medium.com/dailyjs/understanding-v8s-bytecode-317d46c94775
[jsconf v8 slides]: https://github.com/fhinkel/Presentations/blob/main/JSConfEU2017.key
[jsconf v8 pdf slides]: /assets/pdf/JSConfEU2017-v8-fhinkel.pdf
[jsconf v8 video]: https://www.youtube.com/watch?v=p-iiEDtpy6I
[AST wikipedia]: https://en.wikipedia.org/wiki/Abstract_syntax_tree
[jsconf adopting typescript video]: https://www.youtube.com/watch?v=P-J9Eg7hJwE