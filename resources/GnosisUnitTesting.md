# Running Gnosis Unit Tests 

### Prerequisites

- `Node` version >=7 and `npm` installed
- Gnosis [safe-contracts](https://github.com/gnosis/safe-contracts) repository cloned locally

### Instalation

The first step is to go to the cloned repository and install all the required dependencies which are specified in the [package.json](https://github.com/gnosis/safe-contracts/blob/development/package.json) file.

```sh
$ cd safe-contracts/
$ npm install
```

The second command will download most of the required dependencies, including `Truffle` and `ganache-cli` and install them locally under the `./node_modules` folder. 
All the execution scripts will be placed under `./node_modules/bin`.

At the time of writing, the project is using `Truffle` version `4.x`, because `Truffle 5.x` is using a dependency (`web3`) which is actually still in beta and is not backward compatible.
But since Truffle 4.x does not come with `solc 0.5` as default, the next step is to install it manually.

```sh
$ cd node_modules/truffle && npm install solc@0.5.0 && cd ../..
```

The contracts need to be compiled before running the unit tests. This can be done using:

```sh
$ node_modules/.bin/truffle compile
```
Which will compile all Solidity contracts from `./contracts` folder into `json` files and place them under `./build/contracts`.

### Running Unit Tests

There are several ways to run the unit tests. The easiest one would be:
```sh
$ npm test
```
Which will run the script mentioned in the [package.json](https://github.com/gnosis/safe-contracts/blob/development/package.json#L15) file.

This is equivalent with the following commands:
```sh
$ node_modules/.bin/ganache-cli -l 20000000 --noVMErrorsOnRPCResponse true
```
that starts `ganache-cli`, a blockchain emulator on which the unit tests will run, setting a gas limit of `20000000`; and in a separate terminal:
```sh
$ node_modules/.bin/truffle test
```
which will run all the tests inside `./test`.

### Notes

When running the tests, it is most likely that an error will be thrown:
```sh
Error: More than one instance of bitcore-lib found. Please make sure to require bitcore-lib and check that submodules do not also include their own bitcore-lib dependency.

     at Object.bitcore.versionGuard (/home/anvacaru/Work/safe-contracts/node_modules/bitcore-mnemonic/node_modules/bitcore-lib/index.js:12:11)
   ...

```

A workaround for this error is to edit the `index.js` file of the bitcore-lib mentioned in the error and add a `return;` at the beggining of the function. This will prevent the error to be thrown.

```js
bitcore.versionGuard = function(version) { return;
  if (version !== undefined) {
    var message = 'More than one instance of bitcore-lib found. ' +
      'Please make sure to require bitcore-lib and check that submodules do' +
      ' not also include their own bitcore-lib dependency.';
    throw new Error(message);
  }
```
