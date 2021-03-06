# eosplayer

Eosplayer is a bonding layer of eosjs, which is based on eosjs and provides better usability for the application layer. It can be used on both the node.js server and in a browser or Dapp wallet that already has Scatter installed.

frontd releases : [https://github.com/bagaking/eosplayer/releases](https://github.com/bagaking/eosplayer/releases)

LICENSE : [Apache 2.0](https://github.com/bagaking/eosplayer/blob/master/LICENSE)

## structure

![Alt text](./dependency.svg)

## build (for broswer)

`npm run build` or `yarn run build`

## play

1. clone the repo  
2. cd into the folder  
3. install http-server : `npm i http-server -g`
4. serve the play folder : `http-server ./play`
5. open the test site in your chrome : `http://localhost:8080` by default 
6. try it in your chrome console

or using the online play ground:
[http://eosplayer.kihand.com/](http://eosplayer.kihand.com/)

- [transfer tool](http://eosplayer-doc.kihand.com/#/1)
- [sign tool](http://eosplayer-doc.kihand.com/#/2)
- [docs](http://eosplayer-doc.kihand.com/#/3)

## api documents : 

Doc site:
[eosplayer-doc.kihand.pro](http://eosplayer-doc.kihand.com)   
or
[playground](http://eosplayer.kihand.com/#/3)

---

> you can using `help` commond to show api documents on chrome console

## log

there are 4 levels of log  

- verbose
- info
- warning
- error

in the initial situation, the level `verbose` is **closed**, and `info|warning|error` are **opened**.

> if you would like to **disable all level**, set the level filter to `'-'`.  
>
> when enable called, origin setting will be override **(not append)**.
>

### Enable/Disable Log Level in Browser

- open specific level of log : `localStorage.debug = 'warning:*,info:*'`
- open all : `localStorage.debug = '*'`

then, refresh the broswer

### Enable/Disable Log Level in Prompt

#### Unix

- `DEBUG=error:* node test.js`

#### CMD

- `set DEBUG=error:* & node app.js`

#### PowerShell

- `$env:DEBUG = "error:*"`

### Enable/Disable Log Level in program dynamically

```js
const debug = require('debug');
debug.enable('error:*,warning:*,info:*');
```

> (temporary in beta.11) these code should be placed after the require operation of eosplayer.

## Usage of eosplayer (cli)

you can use call methods of eosplayer.chain by cli 

install: `npm i -g eosplayer`
use: `eosplayer --help`

## Usage of eosplayer (for browser)

### Log

### Events

`ERR_TRANSCAL_FAILED`
`ERR_TRANSFER_FAILED`
`ERR_TRANSEND_FAILED`

### APIs

```js
{String} get help // get help info of usage
{String} get version // get the version info
{Chain} get chain // get the chain

{Void} eosplayer.event.setEvent(event, fnCallback, context) //listen to a event

{Eos} get eosplayer.eosClient // get eos instance
{Identity} async eosplayer.getIdentity() // get identity

{AccountInfo} async eosplayer.getAccountInfo(account_name = identity.name) 
    // get account info for any user

{String} async eosplayer.getBalance(account_name = undefined, code = "eosio.token", symbolName = undefined)  
    // get balance string of a account. ex. "1.0000 EOS", null means that the account dosen't have any token,

{String} async eosplayer.getBalances(account_name = undefined, code = "eosio.token")  
    // get balances array of a account. ex. ["1.0000 CT"]

{String} async eosplayer.getBalanceAsset(account_name = undefined, code = "eosio.token") 
    // get balance structure of a account. ex. {val:1, sym:"EOS", decimal:4}

{Tx} async eosplayer.transfer(target, quantity, memo = "")
    // transfer tokens to target

{Tx} async eosplayer.transcal(code, quantity, func, ...args) 
    // send a action of transcal to contract
    
{Tx} async eosplayer.transget(code, symbol, func, ...args) 
    // send a action of trancal (quantity value = 0.0001) to contract

{Contract} async eosplayer.contract(code)
    // get contract object

{Tx} async eosplayer.call(code, func, jsonData)
    // send a action to contract
    
{Tx} async eosplayer.newAccount (name, activeKey, ownerKey)
    // create a account with public key
```

### Chain API

```js
{Object} async getInfo() // get info of the chain connected
{Object} async getBlock(blockNumOrId) // get specific block of the chain

{Contract} async getContract(code) // get contract
{Object} async getAbi(code) // get abi of contract
{Object} async getTableAbi(code, tableName) // get table abi of contract
{Object} async abiJsonToBin(code, action, args) 

{Object} async getAccountInfo(account_name) // get account info of any user
{string} async getPubKey(account_name, authority = "active") // get the first public key of an account
{Array} async getPubKeys(account_name, authority = "active") // get public keys of an account
{string} async recoverSign(signature, message) // recover sign and to the public key
{string} async validateSign (signature, message, account, authority = 'active', accountsPermisionPlugins) 
// validate if signed data is signed by a account. it returns the matched public key 

{Number} async getActionCount(account_name) // get a account's action count
{Number} async getActionMaxSeq(account_name) // get a account's max action seq
{Array} async getRecentActions(account_name) // get recent actions
{Array} async getActions(account_name, startPos = 0, offset = 0) // get all actions of an account
{Array} async getAllActionsBatch (account_name, cbReceive, startPos = 0, count = 100, concurrent = 10) // get all actions in bulk

{String} async getBalance(account_name, code = "eosio.token", symbolName = undefined) // get balance of specific account
{Array.<String>} async getBalances(account_name, code = "eosio.token") // get all balance of specific account
{Tx} async transfer(account, target, quantity, memo = "", cbError) // the format of account should be {name, authority}

{Tx} async waitTx(txID, maxRound = 12, timeSpanMS = 1009) // check a transaction info, retry once per sec until success

{Tx} async call(code, func, jsonData, ...authorization) // send action to a contract

{Array} async getTable(code, tableName, scope, lower, upper, ...hint) // get all items in a table
{Array} async checkTable(code, tableName, scope, limit = 10, lower_bound = 0, upper_bound = -1, index_position = 1) // check a table
{Array} async checkTableMore(code, tableName, scope, primaryKey, limit = 9999999, lower_bound = 0, upper_bound = -1, index_position = 1)
{Array} async checkTableRange(code, tableName, scope, from, length = 1, index_position = 1) // check range in table
{Object} async checkTableItem(code, tableName, scope, key = 0) // check a item in a table

{Object} async updateAuth(account, permission, parent, threshold, keys, accounts, waits) // update auth
```

## Usage of eosplayer (for broswer)

### Events

```js
ERR_GET_SCATTER_FAILED  
ERR_GET_IDENTITY_FAILED
ERR_LOGOUT_FAILED
```

### APIs  

```js
{void} eosplayer.switchNetwork(val) // switch network
{void} eosplayer.setNetConf(network_name, conf) // add a network config at runtime    

get {Scatter} eosplayer.scatter // get scatter instance
get {Scatter} async getScatterAsync(maxTry = 100) // get scatter instance

get {string} eosplayer.netName // get current network name
get {string} eosplayer.netConf // get current network config

async {Identity} eosplayer.login() // let user allow you using identity
async {void} eosplayer.logout() // return back the identity

async sign(message) // sign a message with current identity
```

## Imported libs

```js

window.eosjs = Eos; /** the eosjs lib @see {@url https://www.npmjs.com/package/eosjs} */
window.eosjs_ecc = Ecc; /** the eosjs-ecc lib @see {@url https://www.npmjs.com/package/eosjs-ecc} */
window.BigNumber = BigNumber; /** big number library @see {@url https://www.npmjs.com/package/bignumber.js} */
window.env = env; /** {isPc} */
window.idb = idb; /** idb lib for browser storage @see {@url https://www.npmjs.com/package/idb } */
window.eosplayer = new ScatterPlayer(networks);

```

## Usage of eosplayer (for node.js)

1. install eosplayer  
    `npm i eosplayer --save` or `yarn add eosplayer`
2. import eosplayer  
    `import * from 'eosplayer'` or `const Player = require('eosplayer')`
3. extend Player to create the glue layer, implement methods : eosClient and getIdentity
    see [scatterPlayer](https://github.com/bagaking/eosplayer/blob/master/scatterBinder/scatterPlayer.js)
    ```js
    import Player from 'eosplayer'
    import Eos from 'eosjs'
    class MyPlayer extends Player {
            get eosClient() {
                if (!this._eosClient) {
                    this._eosClient = new Eos(myAwsomeConf);
                }
                return this._eosClient;
            }

            async getIdentity() {
                return { name: "myawsomename", authority: "active" }
            }
    }
    ```
4. there some out-of-box implementation of player
   1. if you wanna reading data of the chain or signing and sending messages to the chain.
   ```js
   import { signPlayer } from 'eosplayer'
   ```
   2. if you need the player automatic fuse and switch node, readingPlayer is a good choice
   ```js
   import { readingPlayer } from 'eosplayer'
   ``` 
5. have fun

---  

## Updates

### 0.4.0

#### add

- Class: src/signPlayer
- Class: src/readingPlayer

### 0.3.0

#### add

- Class: helper/kh.js
- Class: utils/log.js
- Event: ERR_TRANSFER_FAILED
- Event: ERR_TRANSEND_FAILED
- Event: ERR_LOGOUT_FAILED
- Method: get eosplayer.kh
- Method: chain.getTable
- Method: chain.getActionCount
- Method: chain.getRecentActions
- Method: chain.getBalances
- Method: chain.getActions
- Method: chain.getAllActionsBatch
- Method: chain.checkTableMore
- Method: chain.transfer
- Method: chain.getPubKey
- Method: chain.getPubKeys
- Method: chain.recoverSign
- Method: chain.validateSign
- Method: player.getBalances
- Method: player.getAuth
- Method: player.newAccount

#### modify

- Method: the checkTableRange methods deals with 'more' now
- Method: chain.getBalance add param symbolName

#### Export

- Scatter: bigNumber

#### Deprecated

- eosplayer.waitTx (eosplayer.chain.waitTx instead)
- eosplayer.checkTable (eosplayer.chain.checkTable instead)
- eosplayer.checkTableRange (eosplayer.chain.checkTableRange instead)
- eosplayer.checkTableItem (eosplayer.chain.checkTableItem instead)


### 0.2.0 (no release)

#### add

- Class: chain.js
- Method: get eosplayer.chain

### 0.1.2

in this version, scatter are split from the Player.

#### add

- Module: scatterBinder
- Class: src/eosProvider
- Class: scatterBinder/scatterPlayer
- Method: void eventHandler.enableEvents(eventKeys)

#### remove

- Player.EventNames

#### modify

- Rename events :
  - ERR_TRANSCAL_FAILED
  - ERR_GET_SCATTER_FAILED
  - ERR_GET_IDENTITY_FAILED

### 0.1.1

#### add

- async eosplayer.transfer(target, quantity, memo)
- async eosplayer.contract(code)

---

## Contact

email : [kinghand@foxmail.com](kinghand@foxmail.com)  
issue : [https://github.com/bagaking/eosplayer/issues](https://github.com/bagaking/eosplayer/issues)
