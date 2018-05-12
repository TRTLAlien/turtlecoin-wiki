# Wallet RPC API

TurtleCoin RPC Wallet is a HTTP server which provides JSON 2.0 RPC interface for TurtleCoin payment operations and address management.



## Interacting with the API

To make a JSON PRC request to your TurtleCoin RPC Wallet you should use a POST request that looks like this:

`http://<service address>:<service port>/json_rpc`

Parameter            | Description
-------------------- | ------------------------------------------------------------ 
`<service address>`  | IP of TurtleCoin RPC Wallet, if RPC Wallet is located on local machine it is either 127.0.0.1 or localhost
`<service port>`     | TurtleCoin RPC Wallet port, by default it is binded to 8070 port, but it can be manually binded to any port you want, read more about this here.



## reset

`reset()` method allows you to re-sync your wallet.

**Input**
 
Argument         | Mandatory   | Description      | Format
---------------- | ----------- | ---------------- | ------
viewSecretKey    | No          | Private view key | string


No output in case of success.

<aside class="notice">
  If the view_secret_key was not pointed out reset() methods resets the wallet and re-syncs it. If the view_secret_key argument was pointed out reset() method substitutes the existing wallet with a new one with a specified view_secret_key and creates an address for it.
</aside>



## save

`save()` method allows you to save your wallet by request.

No input.
No output in case of success.



## getViewKey

`getViewKey()` method returns your view key.

No input.

**Output**

Argument         | Description      | Format
---------------- | ---------------- | ------
viewSecretKey    | Private view key | string



## getSpendKeys

`getSpendKeys()` method returns your spend keys.

**Input**

Argument         | Mandatory    | Description                                  | Format
---------------- | ------------ | -------------------------------------------- | -------
address          | Yes          | Valid and existing in this container address | string

**Output**

Argument          | Description          | Format
----------------  | -------------------- | ------
spendSecretKey    | Private spend key    | string
spendPublicKey    | Public spend key     | string



## getStatus

`getStatus()` method returns information about the current RPC Wallet state: block_count, known_block_count, last_block_hash and peer_count.

No input.

**Output**

Argument         | Description                                                                | Format
---------------- | -------------------------------------------------------------------------- | ------
blockCount       | Node's known number of blocks                                              | uint32
knownBlockCount  | Maximum known number of blocks of all seeds that are connected to the node | uint32
lastBlockHash    | Hash of the last known block                                               | string
peerCount        | Connected peers number	                                                  | uint32	 



## getAddresses

`getAddresses()` method returns an array of your RPC Wallet's addresses.

No input.

**Output**

Argument          | Description                                           | Format
----------------- | ----------------------------------------------------- | ------
addresses	      | Array of strings, where each string is an address	  | array



## createAddress

`createAddress()` method creates an additional address in your wallet.

**Input**

Argument                 | Mandatory    | Description                                  | Format
------------------------ | ------------ | -------------------------------------------- | -------
secretSpendKey           | No           | Private spend key. If `secretSpendKey` was specified, RPC Wallet creates spend address | string
publicSpendKey           | No           | Public spend key. If `publicSpendKey` was specified, RPC Wallet creates view address   | string



## deleteAddress

`deleteAddress()` method deletes a specified address.

**Input**

Argument         | Mandatory    | Description                                  | Format
---------------- | ------------ | -------------------------------------------- | -------
address          | Yes          | An address to be deleted                     | string

**Output**

In case of success returns an empty JSON object.



## getBalance

`getBalance()` method returns a balance for a specified address.

<aside class="notice">
  If address is not specified, returns a cumulative balance of all RPC Wallet's addresses.
</aside>



## getBlockHashes

`getBlockHashes()` method returns an array of block hashes for a specified block range.

**Input**

Argument         | Mandatory    | Description                                     | Format
---------------- | ------------ | ----------------------------------------------- | -------
firstBlockIndex  | Yes          | Starting height	                              | uint32
blockCount       | Yes          | Number of blocks to process		              | uint32

**Output**

Argument              | Description                                             | Format
--------------------- | ------------------------------------------------------- | ------
blockHashes		      | Array of strings, where each element is a block hash	| array



## getTransactionHashes

`getTransactionHashes()` method returns an array of block and transaction hashes. Transaction consists of transfers.
Transfer is an amount-address pair. There could be several transfers in a single transaction.

**Input**

Argument         | Mandatory                                                                | Description                                                   | Format
---------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------- | -------
addresses        | No                                                                       | Array of strings, where each string is an address	            | array
blockHash        | Only one of these parameters (blockHash or firstBlockIndex) is allowed   | Hash of the starting block                                    | string
firstBlockIndex  | Only one of these parameters (blockHash or firstBlockIndex) is allowed   | Starting height	                                            | uint32
blockCount       | Yes                                                                      | Number of blocks to return transaction hashes from	        | uint32
paymentId        | No                                                                       | Valid payment_id	                                            | string

* If `paymentId` parameter is set, `getTransactionHashes()` method returns transaction hashes of transactions that contain specified payment_id. (in the set block range).
* If `addresses` parameter is set, `getTransactionHashes()` method returns transaction hashes of transactions that contain transfer from at least one of specified addresses.
* If both above mentioned parameters are set, `getTransactionHashes()` method returns transaction hashes of transactions that contain both specified payment_id and transfer from at least one of specified addresses.

**Output**

Argument   | Description                                         |                                                              |            |                                       
---------- | --------------------------------------------------- | ------------------------------------------------------------ | ---------- |
items	   | **Array of**                                        |	                                                            |            |                                                                 
    	   | **Attribute**            	                         | **Description**                                              | **Format** |                                        
           | blockHash                                           | hash of the block which contains transaction hashes          | string     |
           | transactionHashes                                   | array of strings, where each string is a transaction hash    | array      |



## getTransactions

`getTransactions()` method returns an array of block and transaction hashes.
Transaction consists of transfers. Transfer is an amount-address pair. There could be several transfers in a single transaction.

**Input**

Argument        | Mandatory                                                                    | Description                                            | Format
--------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------ | -------
addresses       | No                                                                           | Array of strings, where each string is an address		| array
blockHash       | Only one of these parameters (`blockHash` or `firstBlockIndex`) is allowed.  | Hash of the starting block		                        | string
firstBlockIndex | Only one of these parameters (`blockHash` or `firstBlockIndex`) is allowed.  | Starting height		                                | uint32
blockCount      | Yes                                                                          | Number of blocks to return transaction hashes from		| uint32
paymentId       | No                                                                           | Valid `payment_id`		                                | string

* If `paymentId` parameter is set, `getTransactions()` method returns transactions that contain specified `payment_id`. (in the set block range)
* If `addresses` parameter is set, `getTransactions()` method returns transactions that contain transfer from at least one of specified addresses.
* If both above mentioned parameters are set, `getTransactions()` method returns transactions that contain both specified `payment_id` and transfer from at least one of specified addresses.

**Output**

Argument   |                              | Description                                       | Format
---------- | ---------------------------- | --------------------------------------------------|-----------
items	   | **Array of**                 |                                                   |
    	   | block_hash                   | hash of the block which contains a transaction    | string
    	   | transactions                 | see below                                         | array

Transaction attributes:

Argument            | Description                                       | Format
------------------- | --------------------------------------------------|-----------
transactionHash     | hash of the  transaction                                                      | string 
blockIndex          | number of the block that contains a transaction                               | uint32
timestamp           | timestamp of the transaction                                                  | uint64 
isBase              | shows if the transaction is a CoinBase transaction or not                     | boolean
unlockTime          | height of the block when transaction is going to be available for spending    | uint64
amount              | amount of the transaction                                                     | int64 
fee                 | transaction fee                                                               | uint64
extra               | hash of the  transaction                                                      | string 
paymentId           | payment_id of the transaction (optional)                                      | string 
transfers           | array of address (string), amount (uint64)                                    | array



## getUnconfirmedTransactionHashes

`getUnconfirmedTransactionHashes()` method returns information about the current unconfirmed transaction pool or for a specified addresses.

Transaction consists of transfers. Transfer is an amount-address pair. There could be several transfers in a single transaction.

**Input**

Argument    | Mandatory     | Description                                                | Format
----------- | ------------- | ---------------------------------------------------------- | -------
addresses   | No            | Array of strings, where each string is a valid address     | array 

<aside class="notice">
  If addresses parameter is set, transactions that contain transfer from at least one of specified addresses are returned.
</aside>

**Output**

Argument               | Description                                                                    | Format
---------------------- | ------------------------------------------------------------------------------ | ------
transactionHashes      | Array of strings, where each string is a hash of an unconfirmed transaction	| array



## getTransaction

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
transactionHash     | hash of the  transaction                                                      | string 
blockIndex          | number of the block that contains a transaction                               | uint32
timestamp           | timestamp of the transaction                                                  | uint64 
isBase              | shows if the transaction is a CoinBase transaction or not                     | boolean
unlockTime          | height of the block when transaction is going to be available for spending    | uint64
amount              | amount of the transaction                                                     | int64 
fee                 | transaction fee                                                               | uint64
extra               | hash of the  transaction                                                      | string 
paymentId           | payment_id of the transaction (optional)                                      | string 
transfers           | array of address (string), amount (uint64)                                    | array



## sendTransaction

`sendTransaction()` method allows you to send transaction to one or several addresses. Also, it allows you to use a payment_id for a transaction to a single address.

**Input**

Argument        | Mandatory     | Description                                                                              | Format
--------------- | ------------- | ---------------------------------------------------------------------------------------- | -------
addresses       | No            | Array of strings, where each string is an address to take the funds from                 | array
transfers       | Yes           | Array of address (string), amount (int64)                                                | array
fee             | Yes           | Transaction fee. Minimal fee in TurtleCoin network is .01 BCN. This parameter should be specified in minimal available BCN units. For example, if your fee is .01 BCN, you should pass it as 1000000  | uint64 
unlockTime      | No            | Height of the block until which transaction is going to be locked for spending.          | uint64
anonymity       | Yes           | Privacy level (a discrete number from 1 to infinity). Level 6 and higher is recommended  | uint64
extra           | No            | String of variable length. Can contain A-Z, 0-9 characters.                              | string
paymentId       | No            | payment_id                                                                               | string 
changeAddress   | No            | Valid and existing in this container address.                                            | string 

* If container contains only 1 address, `changeAddress` field can be left empty and the change is going to be sent to this address.
* If addresses field contains only 1 address, `changeAddress` can be left empty and the change is going to be sent to this address.
* In the rest of the cases, `changeAddress` field is mandatory and must contain an address.

**Output**

Argument              | Description                         | Format
--------------------- | ----------------------------------- | ------
transactionHash	      | Hash of the sent transaction		| string



## createDelayedTransaction

`createDelayedTransaction()` method creates a delayed transaction. Such transactions are not sent into the network automatically and should be pushed using `sendDelayedTransaction` method.

**Input**

Argument        | Mandatory     | Description                                                                              | Format
--------------- | ------------- | ---------------------------------------------------------------------------------------- | -------
addresses       | No            | Array of strings, where each string is an address                                        | array
transfers       | Yes           | Array of address (string), amount (int64)                                                | array
fee             | Yes           | Transaction fee. Minimal fee in TurtleCoin network is .01 BCN. This parameter should be specified in minimal available BCN units. For example, if your fee is .01 BCN, you should pass it as 1000000 | uint64
unlockTime      | No	        | Height of the block until which transaction is going to be locked for spending.	       | uint64
anonymity       | Yes           | Privacy level (a discrete number from 1 to infinity). Level 6 and higher is recommended. | uint64
extra           | No            | String of variable length. Can contain A-Z, 0-9 characters.                              | string
paymentId       | No            | payment_id                                                                               | string
changeAddress   | No            | Valid and existing in this container address.                                            | string

* if container contains only 1 address, changeAddress field can be left empty and the change is going to be sent to this address
* if addresses field contains only 1 address, changeAddress can be left empty and the change is going to be sent to this address
* in the rest of the cases, changeAddress field is mandatory and must contain an address.
* outputs that were used for this transactions will be locked until the transaction is sent or cancelled

**Output**

Argument              | Description                         | Format
--------------------- | ----------------------------------- | ------
transactionHash	      | Hash of the sent transaction		| string



## getDelayedTransactionHashes

`getDelayedTransactionHashes()` method returns hashes of delayed transactions.

No input.

**Output**

Argument              | Description                                                     | Format
--------------------- | --------------------------------------------------------------- | ------
transactionHashes	  | Array of strings, where each string is a transaction hash		| array



## deleteDelayedTransaction

`deleteDelayedTransaction()` method deletes a specified delayed transaction.

**Input**

Argument              | Mandatory      | Description                              | Format
--------------------- | -------------- | ---------------------------------------- | -------
transactionHash       | Yes            | Valid, existing delayed transaction      | string

**Output**

In case of success returns an empty JSON object.



## sendDelayedTransaction

`sendDelayedTransaction()` method sends a specified delayed transaction.

**Input**

Argument              | Mandatory      | Description                              | Format
--------------------- | -------------- | ---------------------------------------- | -------
transactionHash       | Yes            | Valid, existing delayed transaction      | string

**Output**

In case of success returns an empty JSON object.



## sendFusionTransaction

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

* if container contains only 1 address, destinationAddress field can be left empty and the funds are going to be sent to this address.
* if addresses field contains only 1 address, destinationAddress can be left empty and the funds are going to be sent to this address.
* in the rest of the cases, destinationAddress field is mandatory and must contain an address.

**Output**

Argument              | Description                         | Format
--------------------- | ----------------------------------- | ------
transactionHash	      | Hash of the sent transaction		| string



## estimateFusion

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
totalOutputCount	| Total number of unspent outputs of the specified addresses. | uint64
fusionReadyCount    | Number of outputs that can be optimized.                    | uint64