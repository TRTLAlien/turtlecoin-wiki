# Wallet RPC API

TurtleCoin RPC Wallet is a HTTP server which provides JSON 2.0 RPC interface for TurtleCoin payment operations and address management.

Currently we support the following official client bindings:

* [JavaScript](https://github.com/turtlecoin/turtlecoin-walletd-rpc-js)
* [Python](https://github.com/turtlecoin/turtlecoin-walletd-rpc-python)
* [Go](https://github.com/turtlecoin/turtlecoin-walletd-rpc-go)
* [PHP](https://github.com/turtlecoin/turtlecoin-walletd-rpc-php)


```javascript
npm install turtlecoin-walletd-rpc-js
```

```php
composer require turtlecoin/turtlecoin-walletd-rpc-php
```

```python
pip install turtlecoin
```

## Interacting with the API

> API endpoint example

```
http://localhost:8070/json_rpc
```

> Configuration and instantiation

```php
<?php
use TurtleCoin\Walletd;

$config = [
    'rpcHost'     => 'http://localhost',
    'rpcPort'     => 8070,
    'rpcPassword' => 'passw0rd',
];

$walletd = new Walletd\Client($config);
```

To make a JSON RPC request to your TurtleCoin RPC Wallet you should use a POST request that looks like this:

`http://<service address>:<service port>/json_rpc`

Parameter            | Description
-------------------- | ------------------------------------------------------------ 
`<service address>`  | IP of TurtleCoin RPC Wallet, if RPC Wallet is located on local machine it is either 127.0.0.1 or localhost
`<service port>`     | TurtleCoin RPC Wallet port, by default it is binded to 8070 port, but it can be manually binded to any port you want



## reset

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"reset","params":{"viewSecretKey":"xxxxx..."}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$viewSecretKey = 'xxxxx...';
$response = $walletd->reset($viewSecretKey);
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{}
}
```

`reset()` method allows you to re-sync your wallet.

**Input**
 
Argument         | Mandatory   | Description      | Format
---------------- | ----------- | ---------------- | ------
viewSecretKey    | No          | Private view key | string


No output in case of success.

<aside class="notice">
  <div>If the <code>viewSecretKey</code> argument is not provided, the <code>reset()</code> method resets the wallet and 
  re-syncs it. If the <code>viewSecretKey</code> argument is provided, the <code>reset()</code> method substitutes the 
  existing wallet with a new one with the specified key and creates an address for it.</div>
</aside>



## save

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"save","params":{}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$response = $walletd->save();
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{}
}
```

`save()` method allows you to save your wallet by request.

No input.
No output in case of success.



## getViewKey

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getViewKey","params":{}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$response = $walletd->getViewKey();
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "viewSecretKey":"xxxxx..."
  }
}
```

`getViewKey()` method returns your view key.

No input.

**Output**

Argument         | Description      | Format
---------------- | ---------------- | ------
viewSecretKey    | Private view key | string



## getSpendKeys

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getSpendKeys","params":{"address":"TRTLxxxx..."}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$address = 'TRTLxxxx...';
$response = $walletd->getSpendKeys($address);
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "spendSecretKey":"xxxxx...",
    "spendPublicKey":"xxxxx..."
  }
}
```

`getSpendKeys()` method returns your spend keys.

**Input**

Argument         | Mandatory    | Description                                  | Format
---------------- | ------------ | -------------------------------------------- | -------
address          | Yes          | Valid address that exists in this container  | string

**Output**

Argument          | Description          | Format
----------------  | -------------------- | ------
spendSecretKey    | Private spend key    | string
spendPublicKey    | Public spend key     | string



## getStatus

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getStatus","params":{}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$response = $walletd->getStatus();
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "blockCount":455956,
    "knownBlockCount":455955,
    "lastBlockHash":"8d6f8...",
    "peerCount":8
  }
}
```

`getStatus()` method returns information about the current RPC Wallet state: block count, known block count, last block hash and peer count.

No input.

**Output**

Argument         | Description                                                                | Format
---------------- | -------------------------------------------------------------------------- | ------
blockCount       | Node's known number of blocks                                              | int
knownBlockCount  | Maximum known number of blocks of all seeds that are connected to the node | int
lastBlockHash    | Hash of the last known block                                               | string
peerCount        | Connected peers number	                                                  | int	 



## getAddresses

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getAddresses","params":{}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$response = $walletd->getAddresses();
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "addresses":[
      "TRTLxxxx...",
      "TRTLxxxx..."
    ]
  }
}
```

`getAddresses()` method returns an array of your RPC Wallet's addresses.

No input.

**Output**

Argument          | Description                                           | Format
----------------- | ----------------------------------------------------- | ------
addresses	      | Array of strings, where each string is an address	  | array



## createAddress

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"createAddress","params":{}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);

$secretSpendKey = null;
$publicSpendKey = null;
$response = $walletd->createAddress($secretSpendKey, $publicSpendKey);
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "address":"TRTLxxxx..."
  }
}
```

`createAddress()` method creates an additional address in your wallet.

**Input**

Argument                 | Mandatory    | Description                                  | Format
------------------------ | ------------ | -------------------------------------------- | -------
secretSpendKey           | No           | Private spend key. If `secretSpendKey` was specified, RPC Wallet creates spend address | string
publicSpendKey           | No           | Public spend key. If `publicSpendKey` was specified, RPC Wallet creates view address   | string



## deleteAddress

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"deleteAddress","params":{"address":"TRTLxxxx..."}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$address = 'TRTLxxxx...';
$response = $walletd->deleteAddress($address);
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{}
}
```

`deleteAddress()` method deletes a specified address.

**Input**

Argument         | Mandatory    | Description                                  | Format
---------------- | ------------ | -------------------------------------------- | -------
address          | Yes          | An address to be deleted                     | string

**Output**

In case of success returns an empty JSON object.



## getBalance

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getBalance","params":{"address":"TRTLxxxx..."}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$address = 'TRTLxxxx...';
$response = $walletd->getBalance($address);
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "availableBalance":10000,
    "lockedAmount":0
  }
}
```

`getBalance()` method returns a balance for a specified address.

**Input**

Argument         | Mandatory    | Description                                          | Format
---------------- | ------------ | ---------------------------------------------------- | -------
address          | No           | Valid address that exists in this container          | string

**Output**

Argument              | Description                                           | Format
--------------------- | ----------------------------------------------------- | ------
availableBalance      | Available balance of the specified address in shells  | int
lockedAmount          | Locked amount of the specified address in shells      | int

<aside class="notice">
  <div>If address is not specified, <code>getBalance()</code> returns a cumulative balance of all RPC Wallet's addresses.
  Also note, balances are expressed in shells, so a balance of 10000 is equal to 100.00 TRTL.</div>
</aside>



## getBlockHashes

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getBlockHashes","params":{"firstBlockIndex":0,"blockCount":3}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$firstBlockIndex = 0;
$blockCount = 3;
$response = $walletd->getBlockHashes($firstBlockIndex, $blockCount);
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "blockHashes":[
      "7fb97...",
      "8c973...",
      "2ef06..."
    ]
  }
}
```

`getBlockHashes()` method returns an array of block hashes for a specified block range.

**Input**

Argument         | Mandatory    | Description                                     | Format
---------------- | ------------ | ----------------------------------------------- | -------
firstBlockIndex  | Yes          | Starting height	                              | int
blockCount       | Yes          | Number of blocks to process		              | int

**Output**

Argument              | Description                                             | Format
--------------------- | ------------------------------------------------------- | ------
blockHashes		      | Array of strings, where each element is a block hash	| array



## getTransactionHashes

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getTransactionHashes","params":{"firstBlockIndex":400000,"blockCount":100000}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$blockCount = 100000;
$firstBlockIndex = 400000;
$blockHash = null;
$addresses = null;
$paymentId = null;

$response = $walletd->getTransactionHashes(
    $blockCount, $firstBlockIndex, $blockHash, $addresses, $paymentId
);

echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "items":[
      {
        "blockHash":"f0d8c...",
        "transactionHashes":["529ea..."]
      },
      {
        "blockHash":"4a1ae...",
        "transactionHashes":["2e709..."]
      }
    ]
  }
}
```

`getTransactionHashes()` method returns an array of block and transaction hashes. A transaction consists of transfers.
A transfer is an amount-address pair. There could be several transfers in a single transaction.

**Input**

Argument         | Mandatory                                                                | Description                                                   | Format
---------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------- | -------
addresses        | No                                                                       | Array of strings, where each string is an address	            | array
blockHash        | Only one of these parameters (blockHash or firstBlockIndex) is allowed   | Hash of the starting block                                    | string
firstBlockIndex  | Only one of these parameters (blockHash or firstBlockIndex) is allowed   | Starting height	                                            | int
blockCount       | Yes                                                                      | Number of blocks to return transaction hashes from	        | int
paymentId        | No                                                                       | Valid payment ID	                                            | string

* If `paymentId` parameter is set, `getTransactionHashes()` method returns transaction hashes of transactions that contain specified payment ID in the given block range.
* If `addresses` parameter is set, `getTransactionHashes()` method returns transaction hashes of transactions that contain transfer from at least one of specified addresses.
* If both above mentioned parameters are set, `getTransactionHashes()` method returns transaction hashes of transactions that contain both specified payment ID and transfer from at least one of specified addresses.

**Output**

Argument   | Description                                         |                                                              |            |                                       
---------- | --------------------------------------------------- | ------------------------------------------------------------ | ---------- |
items	   | **Array of**                                        |	                                                            |            |                                                                 
    	   | **Attribute**            	                         | **Description**                                              | **Format** |                                        
           | blockHash                                           | Hash of the block which contains transaction hashes          | string     |
           | transactionHashes                                   | Array of strings, where each string is a transaction hash    | array      |



## getTransactions

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getTransactions","params":{"firstBlockIndex":400000,"blockCount":100000}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$blockCount = 100000;
$firstBlockIndex = 400000;
$blockHash = null;
$addresses = null;
$paymentId = null;

$response = $walletd->getTransactions(
    $blockCount, $firstBlockIndex, $blockHash, $addresses, $paymentId
);

echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "items":[
      {
        "blockHash":"f0d8c...",
        "transactions":[
          {
            "amount":10000,
            "blockIndex":456018,
            "extra":"01bd0...",
            "fee":10,
            "isBase":false,
            "paymentId":"b6fc6...",
            "state":0,
            "timestamp":1526458339,
            "transactionHash":"529ea...",
            "transfers":[
              {"address":"TRTLxxxx...","amount":10000,"type":0},
              {"address":"","amount":-100000,"type":0},
              {"address":"","amount":89990,"type":0}
            ],
            "unlockTime":0
          }
        ]
      },
      {
        "blockHash":"4a1ae...",
        "transactions":[
          {
            "amount":5000,
            "blockIndex":456076,
            "extra":"018c1...",
            "fee":10,
            "isBase":false,
            "paymentId":"55255...",
            "state":0,
            "timestamp":1526460243,
            "transactionHash":"2e709...",
            "transfers":[
              {"address":"TRTLxxxx...","amount":5000,"type":0},
              {"address":"","amount":-8000,"type":0},
              {"address":"","amount":2990,"type":0}
            ],
            "unlockTime":0
          }
        ]
      }
    ]
  }
}
```

`getTransactions()` method returns an array of block and transaction hashes.
A transaction consists of transfers. A transfer is an amount-address pair. There could be several transfers in a single transaction.

**Input**

Argument        | Mandatory                                                                    | Description                                            | Format
--------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------ | -------
addresses       | No                                                                           | Array of strings, where each string is an address		| array
blockHash       | Only one of these parameters (`blockHash` or `firstBlockIndex`) is allowed.  | Hash of the starting block		                        | string
firstBlockIndex | Only one of these parameters (`blockHash` or `firstBlockIndex`) is allowed.  | Starting height		                                | int
blockCount      | Yes                                                                          | Number of blocks to return transaction hashes from		| int
paymentId       | No                                                                           | Valid payment ID		                                | string

* If `paymentId` parameter is set, `getTransactions()` method returns transactions that contain specified payment ID in the given block range.
* If `addresses` parameter is set, `getTransactions()` method returns transactions that contain transfer from at least one of specified addresses.
* If both above mentioned parameters are set, `getTransactions()` method returns transactions that contain both specified payment ID and transfer from at least one of specified addresses.

**Output**

Argument   |                              | Description                                       | Format
---------- | ---------------------------- | --------------------------------------------------|-----------
items	   | **Array of**                 |                                                   |
    	   | block_hash                   | hash of the block which contains a transaction    | string
    	   | transactions                 | see below                                         | array

Transaction attributes:

Argument            | Description                                       | Format
------------------- | --------------------------------------------------|-----------
transactionHash     | Hash of the transaction                                                      | string 
blockIndex          | Number of the block that contains a transaction                               | int
timestamp           | Timestamp of the transaction                                                  | int 
isBase              | Shows if the transaction is a CoinBase transaction or not                     | boolean
unlockTime          | Height of the block when transaction is going to be available for spending    | int
amount              | Amount of the transaction                                                     | int 
fee                 | Transaction fee                                                               | int
extra               | Hash of the  transaction                                                      | string 
paymentId           | Payment ID of the transaction (optional)                                      | string 
transfers           | Array of address (string), amount (int)                                       | array



## getUnconfirmedTransactionHashes

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getUnconfirmedTransactionHashes","params":{}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$addresses = null;
$response = $walletd->getUnconfirmedTransactionHashes($addresses);
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "transactionHashes":[
      "55a23..."
    ]
  }
}
```

`getUnconfirmedTransactionHashes()` method returns information about the current unconfirmed transaction pool or for a specified addresses.

Transaction consists of transfers. Transfer is an amount-address pair. There could be several transfers in a single transaction.

**Input**

Argument    | Mandatory     | Description                                                | Format
----------- | ------------- | ---------------------------------------------------------- | -------
addresses   | No            | Array of strings, where each string is a valid address     | array 

<aside class="notice">
  <div>If addresses parameter is set, transactions that contain transfer from at least one of specified addresses are returned.</div>
</aside>

**Output**

Argument               | Description                                                                    | Format
---------------------- | ------------------------------------------------------------------------------ | ------
transactionHashes      | Array of strings, where each string is a hash of an unconfirmed transaction	| array



## getTransaction

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getTransaction","params":{"transactionHash":"55a23..."}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$transactionHash = '55a23...';
$response = $walletd->getTransaction($transactionHash);
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "transaction":{
      "amount":5000,
      "blockIndex":456635,
      "extra":"0134b...",
      "fee":10,
      "isBase":false,
      "paymentId":"ac9c5...",
      "state":0,
      "timestamp":1526477499,
      "transactionHash":"55a23...",
      "transfers":[
        {"address":"TRTLxxxx...","amount":5000,"type":0},
        {"address":"","amount":-10000,"type":0},
        {"address":"","amount":4990,"type":0}
      ],
      "unlockTime":0
    }
  }
}
```

`getTransaction()` method returns information about a particular transaction.

Transaction consists of transfers. Transfer is an amount-address pair. There could be several transfers in a single transaction.

**Input**

Argument            | Mandatory     | Description                                                | Format
------------------- | ------------- | ---------------------------------------------------------- | -------
transactionHash     | Yes           | Hash of the requested transaction                          | string 

**Output**

Argument   | Description
---------- | ------------
transaction| see below

Transaction attributes:

Argument            | Description                                                                   | Format
------------------- | ------------------------------------------------------------------------------|-------
transactionHash     | Hash of the transaction                                                      | string 
blockIndex          | Number of the block that contains a transaction                               | int
timestamp           | Timestamp of the transaction                                                  | int 
isBase              | Shows if the transaction is a CoinBase transaction or not                     | boolean
unlockTime          | Height of the block when transaction is going to be available for spending    | int
amount              | Amount of the transaction                                                     | int 
fee                 | Transaction fee                                                               | int
extra               | Hash of the  transaction                                                      | string 
paymentId           | Payment ID of the transaction (optional)                                      | string 
transfers           | Array of addresses (string), amount (int)                                  | array



## sendTransaction

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"sendTransaction","params":{"transfers":[{"address":"TRTLxxxx...","amount":"5000"}],"fee":10,"anonymity":3,"changeAddress":"TRTLyyyy..."}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$anonymity = 3;
$fee = 10;
$addresses = null;
$unlockTime = null;
$extra = null;
$paymentId = null;
$changeAddress = 'TRTLyyyy...';

$transfers = [
    ["address" => "TRTLxxxx...", "amount"  => 5000],
];

$response = $walletd->sendTransaction(
    $anonymity, $transfers, $fee, $addresses, $unlockTime, $extra, $paymentId, $changeAddress
);

echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "transactionHash":"ae57e..."
  }
}
```

`sendTransaction()` method allows you to send transaction(s) to one or several addresses. Also, it allows you to use a payment ID for a transaction to a single address.

**Input**

Argument        | Mandatory     | Description                                                                              | Format
--------------- | ------------- | ---------------------------------------------------------------------------------------- | -------
addresses       | No            | Array of strings, where each string is an address to take the funds from                 | array
transfers       | Yes           | Array of objects, address: (string address), amount: (int amount)                        | array
fee             | Yes           | Transaction fee. Minimal fee in TurtleCoin network is 0.10 TRTL. As with other amounts use whole units, 1 TRTL = 100 units, so 0.1 TRTL = 10 units | int
unlockTime      | No            | The block height at which the transaction will be unlocked for spending.                 | int
anonymity       | Yes           | Privacy (mixin) level (a discrete number from 0 to > than 0). 6 and higher is recommended| int
extra           | No            | String of variable length. Can contain A-Z, 0-9 characters.                              | string
paymentId       | No            | Payment ID                                                                               | string 
changeAddress   | No            | Valid and existing address in this container.                                            | string 

* If container contains only 1 address, `changeAddress` field can be left empty and the change is going to be sent to this address.
* If addresses field contains only 1 address, `changeAddress` can be left empty and the change is going to be sent to this address.
* In the rest of the cases, `changeAddress` field is mandatory and must contain an address.

**Output**

Argument              | Description                         | Format
--------------------- | ----------------------------------- | ------
transactionHash	      | Hash of the sent transaction		| string



## createDelayedTransaction

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"createDelayedTransaction","params":{"transfers":[{"address":"TRTLxxxx...","amount":"5000"}],"fee":10,"anonymity":3,"changeAddress":"TRTLyyyy..."}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$anonymity = 3;
$fee = 10;
$addresses = null;
$unlockTime = null;
$extra = null;
$paymentId = null;
$changeAddress = 'TRTLyyyy...';

$transfers = [
    ["address" => "TRTLxxxx...", "amount"  => 5000],
];

$response = $walletd->createDelayedTransaction(
    $anonymity, $transfers, $fee, $addresses, $unlockTime, $extra, $paymentId, $changeAddress
);

echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "transactionHash":"ae57e..."
  }
}
```

`createDelayedTransaction()` method creates a delayed transaction. Such transactions are not sent into the network automatically and should be pushed using `sendDelayedTransaction` method.

**Input**

Argument        | Mandatory     | Description                                                                              | Format
--------------- | ------------- | ---------------------------------------------------------------------------------------- | -------
addresses       | No            | Array of strings, where each string is an address                                        | array
transfers       | Yes           | Array of address (string), amount (int)                                                  | array
fee             | Yes           | Transaction fee. Minimal fee in TurtleCoin network is 0.10 TRTL. This parameter should be specified in minimal available TRTL units. For example, if your fee is 0.10 TRTL, you should pass it as 10. | int
unlockTime      | No	        | Height of the block until which transaction is going to be locked for spending.	       | int
anonymity       | Yes           | Privacy level (a discrete number from 1 to infinity). Level 6 and higher is recommended. | int
extra           | No            | String of variable length. Can contain A-Z, 0-9 characters.                              | string
paymentId       | No            | Payment ID                                                                               | string
changeAddress   | No            | Valid and existing in this container address.                                            | string

* If container contains only 1 address, `changeAddress` field can be left empty and the change is going to be sent to this address
* If addresses field contains only 1 address, `changeAddress` can be left empty and the change is going to be sent to this address
* In the rest of the cases, `changeAddress` field is mandatory and must contain an address.
* Outputs that were used for this transactions will be locked until the transaction is sent or cancelled

**Output**

Argument              | Description                         | Format
--------------------- | ----------------------------------- | ------
transactionHash	      | Hash of the sent transaction		| string



## getDelayedTransactionHashes

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getDelayedTransactionHashes","params":{}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$response = $walletd->getDelayedTransactionHashes();
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "transactionHashes":["b3e374..."]
  }
}
```

`getDelayedTransactionHashes()` method returns hashes of delayed transactions.

No input.

**Output**

Argument              | Description                                                     | Format
--------------------- | --------------------------------------------------------------- | ------
transactionHashes	  | Array of strings, where each string is a transaction hash		| array



## deleteDelayedTransaction

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"deleteDelayedTransaction","params":{"transactionHash":"b3e37..."}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$transactionHash = 'b3e37...';
$response = $walletd->deleteDelayedTransaction($transactionHash);
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{}
}
```

`deleteDelayedTransaction()` method deletes a specified delayed transaction.

**Input**

Argument              | Mandatory      | Description                              | Format
--------------------- | -------------- | ---------------------------------------- | -------
transactionHash       | Yes            | Valid, existing delayed transaction      | string

**Output**

In case of success returns an empty JSON object.



## sendDelayedTransaction

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"sendDelayedTransaction","params":{"transactionHash":"c37cd..."}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$transactionHash = 'c37cd...';
$response = $walletd->sendDelayedTransaction($transactionHash);

echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{}
}
```

`sendDelayedTransaction()` method sends a specified delayed transaction.

**Input**

Argument              | Mandatory      | Description                              | Format
--------------------- | -------------- | ---------------------------------------- | -------
transactionHash       | Yes            | Valid, existing delayed transaction      | string

**Output**

In case of success returns an empty JSON object.



## sendFusionTransaction

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"sendFusionTransaction","params":{"threshold":1000000,"anonymity":3,"addresses":["TRTLxxxx...","TRTLyyyy..."],"destinationAddress":"TRTLzzzz..."}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$threshold = 1000000;
$anonymity = 3;
$addresses = ['TRTLxxxx...', 'TRTLyyyy...'];
$destinationAddress = 'TRTLzzzz...';
$response = $walletd->sendFusionTransaction($threshold, $anonymity, $addresses, $destinationAddress);

echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "transactionHash":"93faed..."
  }
}
```

`sendFusionTransaction()` method allows you to send a fusion transaction, by taking funds from selected addresses and 
transferring them to the destination address.
If there aren't any outputs that can be optimized, `sendFusionTransaction()` will return an error. You can 
use `estimateFusion` to check the outputs, available for the optimization.

**Input**

Argument            | Mandatory  | Description                                                                                          | Format
------------------- | ---------- | ---------------------------------------------------------------------------------------------------- | -------
threshold           | Yes        | Value that determines which outputs will be optimized. Only the outputs, lesser than the threshold value, will be included into a fusion transaction. | string
anonymity           | Yes        | Privacy level (a discrete number from 1 to infinity). Level 6 and higher is recommended.             | string
addresses           | No         | Array of strings, where each string is an address to take the funds from.	                        | array
destinationAddress  | No         | An address that the optimized funds will be sent to. Valid and existing in this container address.	| string

* If container contains only 1 address, `destinationAddress` field can be left empty and the funds are going to be sent to this address.
* If addresses field contains only 1 address, `destinationAddress` can be left empty and the funds are going to be sent to this address.
* In the rest of the cases, `destinationAddress` field is mandatory and must contain an address.

**Output**

Argument              | Description                         | Format
--------------------- | ----------------------------------- | ------
transactionHash	      | Hash of the sent transaction		| string



## estimateFusion

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"estimateFusion","params":{"threshold":1000000,"addresses":["TRTLxxxx...","TRTLyyyy..."]}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$threshold = 1000000;
$addresses = ['TRTLxxxx...', 'TRTLyyyy...'];
$response = $walletd->estimateFusion($threshold, $addresses);

echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "fusionReadyCount":0,
    "totalOutputCount":8
  }
}
```

`estimateFusion()` method counts the number of unspent outputs of the specified addresses and returns how many of those outputs can be optimized.
This method is used to understand if a fusion transaction can be created. If `fusionReadyCount` returns a value = 0, then a fusion transaction cannot be created.

**Input**

Argument            | Mandatory  | Description                                                                                          | Format
------------------- | ---------- | ---------------------------------------------------------------------------------------------------- | -------
threshold           | Yes        | Value that determines which outputs will be optimized. Only the outputs, lesser than the threshold value, will be included into a fusion transaction. | string
addresses           | No         | Array of strings, where each string is an address to take the funds from.	                        | string

**Output**

Argument            | Description                                                 | Format
------------------- | ----------------------------------------------------------- | ------
totalOutputCount	| Total number of unspent outputs of the specified addresses. | int
fusionReadyCount    | Number of outputs that can be optimized.                    | int

## getMnemonicSeed

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getMnemonicSeed","params":{"address":"TRTLxxxx..."}}' http://localhost:8070/json_rpc
```

```php
<?php
use TurtleCoin\Walletd;

$walletd = new Walletd\Client($config);
$address = 'TRTLxxxx...';
$response = $walletd->getMnemonicSeed($address);
echo $response->getBody()->getContents();
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "mnemonicSeed":"turtles are cool..."
  }
}
```

`getMnemonicSeed()` method returns the mnemonic seed for the given _deterministic_ address. A mnemonic seed is a list of words which can be used to recover a wallet.

**Input**

Argument         | Mandatory    | Description                                  | Format
---------------- | ------------ | -------------------------------------------- | -------
address          | Yes          | Valid deterministic address that exists in this container | string

**Output**

Argument          | Description          | Format
----------------  | -------------------- | ------
mnemonicSeed      | Mnemonic seed        | string

<aside class="notice">
  <div>The first wallet address that is generated when the container is created is the deterministic address. Only one wallet from a multi-wallet container can be deterministic. If a non-deterministic address is given, the RPC response will be an error with the message: "Keys not deterministic."</div>
</aside>