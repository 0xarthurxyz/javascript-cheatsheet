# JavaScript (Cheat Sheet)

This is a cheatsheet on JavaScript/TypeScript (mostly notes-to-self). They are incomplete by default.

## Basics

### References (variables)

Source: [Airbnb style guide](/airbnb.md#references).

Use `const` for all of your references; avoid using `var`.

> Why? This ensures that you can’t reassign your references, which can lead to bugs and difficult 
> to comprehend code.

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

+	JavaScript objects

	+	are key-value pairs that can contain strings, numbers, arrays, functions, booleans, and 
		other objects

	```js
	// JavaScript Object
	const jsObj = {
		name: 'Alice',
		age: 30,
	};
	```

+	JSON objects

	+	are text-only (that means both keys and values are `strings`)

		```js
		// JSON Object
		const jsonObj = {
			"name": "Alice",
			"age": "30",
		};
		```
	+	you can validate JSON objects using [jsonlint.com](https://jsonlint.com/)

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
	name: 'San Francisco',
};
obj[getKey('enabled')] = true;

// good
const obj = {
	id: 5,
	name: 'San Francisco',
	[getKey('enabled')]: true,
};
```

Group your shorthand properties at the beginning of your object declaration.

> Why? It’s easier to tell which properties are using the shorthand.

```javascript
const anakinSkywalker = 'Anakin Skywalker';
const lukeSkywalker = 'Luke Skywalker';

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

Only quote properties that are invalid identifiers. eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props)

> Why? In general we consider it subjectively easier to read. It improves syntax highlighting, and is also more easily optimized by many JS engines.

```javascript
// bad
const bad = {
	'foo': 3,
	'bar': 4,
	'data-blah': 5,
};

// good
const good = {
	foo: 3,
	bar: 4,
	'data-blah': 5,
};
```

Source: [Airbnb style guide](/airbnb.md#objects).

## Package managers (npm)

Source: [devhints.io](https://devhints.io/npm)

### Package management

| Command                           | Description                                               |
| ---                               | ---                                                       |
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


`--save` is the default as of npm@5. Previously, using `npm install` without `--save` doesn't 
update package.json.

### Install names

| Command                              | Description             |
| ---                                  | ---                     |
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

| Command                 | Description                                                         |
| ---                     | ---                                                                 |
| `npm list`              | Lists the installed versions of all dependencies in this software   | 
| `npm list -g --depth 0` | Lists the installed versions of all globally installed packages     | 
| `npm view`              | Lists the latest versions of all dependencies in this software      | 
| `npm outdated`          | Lists only the dependencies in this software which are outdated     |

### Updating

| Command             | Description                |
| ---                 | ---                        |
| `npm update`        | Update production packages |
| `npm update --dev`  | Update dev packages        |
| `npm update -g`     | Update global packages     |
| ---                 | ---                        |
| `npm update lodash` | Update a package           |


### Removing

| Command             | Description                        |
| ---                 | ---                                |
| `npm rm lodash`     | Remove package production packages |

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




## TypeScript

### Initialize a TypeScript project

```bash
npm init -y # shortcut for npm init --yes
npm install typescript --save-dev # install typescript
```

Manually create `tsconfig.json` file if necessary:

```bash
tsc --init
```

Source: [typescriptlang.org](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

### Microsoft Cheat Sheets

Source: [typescriptlang.org](https://www.typescriptlang.org/cheatsheets)

<img src="assets/images/TypeScript-Control-Flow-Analysis.png" width="950">

<img src="assets/images/TypeScript-Types.png" width="950">

<img src="assets/images/TypeScript-Classes.png" width="950">

<img src="assets/images/TypeScript-Interfaces.png" width="950">

### TypeScript for Java/OOP Programmers

Source: [typescriptlang.org](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-oop.html)

+	In Java:

	+	everything belongs to a class or interface

	+	it's meaningful to think of a one-to-one correspondence between runtime types and 
		their compile-time declarations

	+	types are related to their declarations, not their structures.

	+	the type system is (reified) _nominal_.

		> A [type system](https://en.wikipedia.org/wiki/Type_system) is **nominal,** 
		> **nominative,** or **name-based** if compatibility and equivalence of 
		> [data types](https://en.wikipedia.org/wiki/Data_type) is determined by explicit 
		> declarations and/or the name of the types. Nominal systems are used to determine if types 
		> are equivalent, as well as if a type is a subtype of another. 
		> 
		> Nominal type systems contrast with 
		> [structural systems](https://en.wikipedia.org/wiki/Structural_type_system), where 
		> comparisons  are based on the structure of the types in question and do not require
		> explicit declarations.

		Source: [wikipedia.org](https://en.wikipedia.org/wiki/Nominal_type_system)

+	In TypeScript:

	+	_free functions_ (those not associated with a class) working over data without an implied 
		OOP hierarchy are the preferred model for writing programs (in JavaScript more broadly)
	+	types are _sets_ (a particular value can belong to *many* sets or types at the same 
		time)
	+	classes and many common patterns such as  interfaces, inheritance, and static methods
		are supported

For example:

```ts
// Example
interface Pointlike {
	x: number;
	y: number;
}
interface Named {
	name: string;
}
 
function logPoint(point: Pointlike) {
	console.log("x = " + point.x + ", y = " + point.y);
}
 
function logName(x: Named) {
	console.log("Hello, " + x.name);
}
 
const obj = {
	x: 0,
	y: 0,
	name: "Origin",
};
 
logPoint(obj);
logName(obj);
```

Source: [other notes](other.md#typescript-for-javac-programmers).

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

