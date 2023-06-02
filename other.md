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

Source: [YouTube] Learning Functional Programming with JavaScript - Anjana Vakil - JSUnconf

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

<!-- Hyperlinks -->

[realpython javascript vs python]: https://realpython.com/python-vs-javascript/#ecosystem
[ideal hash trees paper]: https://lampwww.epfl.ch/papers/idealhashtrees.pdf