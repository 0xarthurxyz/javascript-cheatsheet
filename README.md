# JavaScript (Cheat Sheet)

Context: These are noob notes on JavaScript/TypeScript (mostly notes-to-self). They are incomplete by default.

- [JavaScript References by Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript#reference)
- [TypeScript Documentation](https://www.typescriptlang.org/docs/)
- [RealPython: Python vs JavaScript for Pythonistas](https://realpython.com/python-vs-javascript/)

## Standard built-in objects

### Data types

Data types:

- undefined
- null
- boolean
- string `"a"` `\"a\"` `'"a"'` `'a'`
- symbol
- number
- object

### Assignment operators

- `var`: any data type over time (global scope)
- `let`: value can change - mutable (local scope)
- `const`: value cannot change - immutable

### Print

```js
var a = 7;
console.log(a)
>>> 7
```

### Arrays

```js
var arr = ["a", 1];
var nestedArr = [["a", 1], ["b", 2]]
```

Adds to end of array (`arr.push(<elements>)`):

```js
var arr = [1, 2, 3]
arr.push(4)
// arr = [1, 2, 3, 4]
arr.push([5, 6])
// arr = [1, 2, 3, 4, [5, 6]]
```

Adds to start of array (`arr.unshift(<elements>)`):

```js
var arr = [1, 2, 3]
arr.unshift(0)
// arr = [0, 1, 2, 3]
arr.unshift([-2, -1])
// arr = [[-2, -1], 0, 1, 2, 3]
```

Removes from end of array (`arr.pop()`):

```js
var arr = [1, 2, 3]
var lastElement = arr.pop()
// arr = [1, 2] and lastElement = 3
```

Removes from start of array (`arr.shift()`):

```js
var arr = [1, 2, 3]
var firstElement = arr.shift()
// arr = [2, 3] and firstElement = 1
```

### Functions

```js
function someReusableFunction(){
  console.log("hello world");
}

someReusableFunction()

function anotherReusableFunction(){
  console.log("foo bar");

anotherReusableFunction()

// hello world
// foo bar
```

Functions with arguments:

```js
function someFunctionWithArgs(a, b){
  console.log(a - b);
}

someFunctionWithArgs(10, 3);
// 7
```

Function with return:

```js
function subtract(a, b){
  return a - b;
}

var result = subtract(10, 3);
// result = 7
```

Variable Scope:

- a `var` declared outside function body has **global** scope
- a `var` declared in a function is scoped to the function, but (!) if it is declarated without keyword `var` in a function body it has global scope

Equality (`==`) vs strict equality (`===`):

- `==` attempts to covert both variables to a common data type (also for inequality `!=`)
- `===` does not perform type conversion (also for strict inequality `!==`)

```js
console.log(7 == '7')
// true

console.log(7 === '7')
// false

console.log(7 !== '7')
// true

console.log(7 != '7')
// false
```

### JavaScript Objects

- (in JS) objects are like arrays but instead of using indices you are using properties (like a key-value store or dictionnary in Python)
- you can assigning object properties with dot notation
- you can access object properties with bracket notation (good when `key` is a string with space, e.g. "location country")

```js
// 1. create object
var myFriend = {
  "name": "Joe",
  "location": "Venezuela",
  "age": 25,
  "friend": ["everyone!"]
};

// 2. access properties
var myFriendsName = myFriend.name;
// Joe
var myFriendsLocation = myFriend["location"];
// Venezuela

// 3. update properties
myFriend.age = 31;
/*
{
  name: 'Joe',
  location: 'Venezuela',
  age: 31,
  friend: [ 'everyone!' ]
}
*/

// 4. add new properties
myFriend["profession"] = "happy coder";
/* 
{
  name: 'Joe',
  location: 'Venezuela',
  age: 31,
  friend: [ 'everyone!' ],
  profession: 'happy coder'
}
*/

// 5. delete a property
delete myFriend.location
/* 
{
  name: 'Joe',
  age: 31,
  friend: [ 'everyone!' ],
  profession: 'happy coder'
}
*/

// 6. testing for property
myFriend.hasOwnProperty('name');
// true
```

### Convert Unix timestamp to time using `Date()`

> Simply multiply Unix timestamp by 1000 to convert it to a JavaScript time, because Unix timestamp measures time as a number of seconds, whereas in JavaScript time is fundamentally specified as the number of milliseconds (elapsed since January 1, 1970 at 00:00:00 UTC).

```js
// Timestamp in seconds
var unixTimestamp = 1651822834;

/* Create a new JavaScript Date object based on Unix timestamp.
Multiplied it by 1000 to convert it into milliseconds */
var date = new Date(unixTimestamp * 1000);

// Generate date string
console.log(date.toLocaleDateString("en-US"));   // Prints: 5/6/2022
console.log(date.toLocaleDateString("en-GB"));   // Prints: 06/05/2022
console.log(date.toLocaleDateString("default")); // Prints: 5/6/2022

// Generate time string
console.log(date.toLocaleTimeString("en-US"));   // Prints: 1:10:34 PM
console.log(date.toLocaleTimeString("it-IT"));   // Prints: 13:10:34
console.log(date.toLocaleTimeString("default")); // Prints: 1:10:34 PM
```

Source: [Tutorial Republic > How to Convert a Unix Timestamp to Time in JavaScript](https://www.tutorialrepublic.com/faq/how-to-convert-a-unix-timestamp-to-time-in-javascript.php)


Syntax: 
```js
toLocaleDateString()
toLocaleDateString(locales)
toLocaleDateString(locales, options) // prints: "Thursday, December 20, 2012, UTC"
```

`locale`: 

```js
// US English uses month-day-year order
console.log(date.toLocaleDateString("en-US"));
// "12/20/2012"

// British English uses day-month-year order
console.log(date.toLocaleDateString("en-GB"));
// "20/12/2012"

// Korean uses year-month-day order
console.log(date.toLocaleDateString("ko-KR"));
// "2012. 12. 20."
```

`options`: 

```js
const options = {
  weekday: "long",
  year: "numeric",
  month: "long",
  day: "numeric",
};
```

Example: 

```js
const date = new Date(Date.UTC(2012, 11, 20, 3, 0, 0));

// request a weekday along with a long date
const options = {
  weekday: "long",
  year: "numeric",
  month: "long",
  day: "numeric",
};
console.log(date.toLocaleDateString("de-DE", options));
// "Donnerstag, 20. Dezember 2012"

// an application may want to use UTC and make that visible
options.timeZone = "UTC";
options.timeZoneName = "short";
console.log(date.toLocaleDateString("en-US", options));
// "Thursday, December 20, 2012, UTC"
```

Source: [MDN Web Docs > `Date.prototype.toLocaleDateString()` ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleDateString#using_options)

## NPM and yarn (package managers)

Source: [dev.to - The Difference Between NPM and Yarn][yarn npm difference]

To see list of commands:
NPM - `npm`
Yarn - `yarn`

Install dependencies from package.json:
NPM - `npm install`
Yarn - `yarn`

Install a package and add to package.json:
NPM - `npm install package --save`
Yarn - `yarn add package`

Install a devDependency:
NPM - `npm install package --save-dev`
Yarn - `yarn add package --dev`

Remove a dependency:
NPM - `npm uninstall package --save`
Yarn - `yarn remove package`

Upgrade a package to its latest version:
NPM - `npm update --save`
Yarn - `yarn upgrade`

Install a package globally:
NPM - `npm install package -g`
Yarn - `yarn global add package`

## Node.js

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
5. Run `npm install <your_required_package_name>` to install and add dependencies to the `package.json` file

### Package.json

To make package.json files from from scratch:
- run `npm init` (takes you through mini questionnaire in CLI and creates file)
- run  `npm install <package>` (automatically adds to dependency in the package.json file)

### NVM (node version manager)

> [Node Version Manager is a tool that helps us manage Node versions](https://github.com/nvm-sh/nvm) and is a convenient way to install Node. Think of it as npm or Yarn that helps manage Node packages, but instead of packages, NVM manages Node versions.

Source: [logrocket](https://blog.logrocket.com/how-switch-node-js-versions-nvm/)

### Displaying a list of Node.js versions

> We can now view all the versions we downloaded so far; currently, we have three Node versions installed using NVM.
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


### Import vs Require

Source: [Stackdiary > Require vs. Import in JavaScript][stackdiary require import]

One of the main differences between `require` and `import` is that

+   `require` can only be used to import modules, whereas 
+   `import` can be used to import both modules and individual exports from those modules.

In general, `import` is preferred over require because it is a more modern and flexible syntax, 
and it will eventually replace `require` in the language.

For example, if you have a module named `myModule`, you can use `require` to import the 
entire module like this:

```js
const myModule = require('myModule');
```

To import a specific export from the module, you would need to use the `.` notation like this:

```js
const myFunction = require('myModule').myFunction;
```

Using `import`, you can import the entire module and all of its exports like this:

```js
import * as myModule from 'myModule';
```

Or you can import a specific export like this:

```js
import {myFunction} from 'myModule';
```

## TypeScript

### Microsoft Cheat Sheets

Source: [typescriptlang.org](https://www.typescriptlang.org/cheatsheets)

<img src="assets/images/TypeScript-Control-Flow-Analysis.png" width="950">

<img src="assets/images/TypeScript-Types.png" width="950">

<img src="assets/images/TypeScript-Classes.png" width="950">

<img src="assets/images/TypeScript-Interfaces.png" width="950">

## React.js

Resources:

- React docs > Learn React (beta) > [Quick Start](https://beta.reactjs.org/learn)
- React docs > [Hello world](https://reactjs.org/docs/hello-world.html)

### React Elements

React elements are written just like regular HTML elements. You can write any valid HTML element in React:

```js
<h1>My Header</h1>
<p>My paragraph>
<button>My button</button>
```

Source: [freeCodeCamp - React cheat sheet for 2022](https://www.freecodecamp.org/news/the-react-cheatsheet/)

Styling: Inline styles are not written as plain strings, but as properties on objects:

```js
<h1 style={{ fontSize: 24, margin: '0 auto', textAlign: 'center' }}>My header</h1>
```

### React Components

> We can organize groups of elements into React components.

Here is the basic syntax of a React function component:

1. Component names **must start with a capital letter** (that is, MyComponent, instead of myComponent)
2. Components, unlike JavaScript functions, **must return JSX**.

```js
function App() {
  return (
     <div>Hello world!</div>
  );
} 
```

Source: [freeCodeCamp - React cheat sheet for 2022](https://www.freecodecamp.org/news/the-react-cheatsheet/)


### React Props

React components can accept data passed to them in **objects** called `props`. Props are passed from the parent component to a child component.

Here we are passing a prop `name` from App to the User component.

```js
function App() {
  return <User name="John Doe" />
}

function User(props) {
  return <h1>Hello, {props.name}</h1>; // Hello, John Doe!
}
```

Or simpler (if only one attribute):

```js
function App() {
  return <User name="John Doe" />
}

function User({ name }) {
  return <h1>Hello, {name}!</h1>; // Hello, John Doe!
}
```

Source: [freeCodeCamp - React cheat sheet for 2022](https://www.freecodecamp.org/news/the-react-cheatsheet/)


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

Source: [freeCodeCamp - React cheat sheet for 2022](https://www.freecodecamp.org/news/the-react-cheatsheet/)


Ternary list:

```js
function App() {
	const isAuthUser = useAuth();

  return (
    <>
      <h1>My App</h1>
      {isAuthUser ? <AuthApp /> : <UnAuthApp />}
    </>
  ) 
}
```

Source: [freeCodeCamp - React cheat sheet for 2022](https://www.freecodecamp.org/news/the-react-cheatsheet/)


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

Source: [freeCodeCamp - React cheat sheet for 2022](https://www.freecodecamp.org/news/the-react-cheatsheet/)

## EthersJS

These are noob notes for the [EthersJS](https://github.com/ethers-io/ethers.js) web3 library (mostly notes-to-self). They are incomplete by default.

### Provider (read-only)

Syntax:

```js
new ethers.Contract(address, abi, signerOrProvider);
```

_Source: [EthersJS > Contract > Creating instances](https://docs.ethers.org/v5/api/contract/contract/#Contract--creating)_

Creates instance of Provider:

```js
const { ethers } = require("ethers");

// Instantiate: Provider
const rpcUrl = "https://forno.celo.org";
const chainId = "42220";
const provider = new ethers.providers.JsonRpcProvider(rpcUrl, chainId);
```

Temporary bug: Some [issue](https://stackoverflow.com/questions/75385248/typeerror-cannot-read-properties-of-undefined-reading-jsonrpcprovider) in ethers v6 with `jsonRpcProvider`.

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

Syntax: You can add specific human-readable ABI (simply copy function signature from smart contract):

```js
const abi = [
    "function getCurrentReleasedTotalAmount() public view returns (uint256)",
];
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
const abi = [
    "function getCurrentReleasedTotalAmount() public view returns (uint256)",
];
let contract = new ethers.Contract(contractAddress, abi, signer);
```

### Contract variables

To query public variables via ethers, you can simply add getter function in your ABI with parentheses.

> The compiler automatically creates getter functions for all **public** state variables. For the contract given below, the compiler will generate a function called `data` that does not take any arguments and returns a `uint`, the value of the state variable `data`.

Source: [Solidity docs > Getter Functions](https://docs.soliditylang.org/en/v0.8.4/contracts.html#getter-functions), originally found via [Stack Overflow](https://ethereum.stackexchange.com/a/99587)

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

Source: [Celo Monorepo > ReleaseGold.sol](https://github.com/celo-org/celo-monorepo/blob/75c223a1b5296ac0fecdb54fb29c52432cf3fece/packages/protocol/contracts/governance/ReleaseGold.sol#L63-L64)

### Contract structs

Human-readable structs are not currently supported in ethers, but the following workaround makes them work:

> While struct syntax is not supported, this works for me
> 
> ```ts
> const User = "(address user, string email)";
> const abi = [
>   `event registered(${User} user)`,
>   `function getId(${User} user) view returns (uint id)`,
>   `function getUser(uint id) view returns (${User} user)`,
>   `function addUser(${User} user)`,
> ];
> const Relationship = `(${User} a, ${User} b, string relationship)`;
> ```

Source: [Github > ethers-io > Issues](https://github.com/ethers-io/ethers.js/issues/315#issuecomment-1035675960)

Example:

```js
// Define struct-like variable
const ReleaseSchedule = "(uint256 releaseStartTime, uint256 releaseCliff, uint256 numReleasePeriods, uint256 releasePeriod, uint256 amountReleasedPerPeriod)";
// Reference struct-like variable in ABI
const abi = [
    `function releaseSchedule() public view returns (${ReleaseSchedule})`
];
// Query struct
const releaseSchedule = await releaseGoldContract.releaseSchedule();
console.log( releaseSchedule.toString() );
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

Source: [Celo Monorepo > ReleaseGold.sol](https://github.com/celo-org/celo-monorepo/blob/75c223a1b5296ac0fecdb54fb29c52432cf3fece/packages/protocol/contracts/governance/ReleaseGold.sol#L21-L32)

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
const abi = [
    "function getCurrentReleasedTotalAmount() public view returns (uint256)",
];
let contract = new ethers.Contract(contractAddress, abi, signer);

// Query function in ReleaseGold.sol contract
const getReleasedAmountReceipt =
    await releaseGoldContract.getCurrentReleasedTotalAmount();

// Visualize response
console.log(ethers.utils.formatEther(getReleasedAmountReceipt.toString()));
```

Things to learn:

+ [ ]   State changing methods
+ [ ]   Listening to events
+ [ ]   Query historic events
+ [ ]   Signing Messages

<!-- Hyperlinks -->

[stackdiary require import]: https://stackdiary.com/require-vs-import-in-javascript/
[yarn npm difference]: https://dev.to/samithawijesekara/the-difference-between-npm-and-yarn-2j3p