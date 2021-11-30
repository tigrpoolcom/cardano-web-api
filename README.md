
# Cardano Public API brought you by CutyMals.com
- [Cardano Public API brought you by CutyMals.com](#cardano-public-api-brought-you-by-cutymalscom)
  - [A Free Cardano Public API - What can i do with it ?](#a-free-cardano-public-api---what-can-i-do-with-it-)
  - [What is OData ?](#what-is-odata-)
  - [Trasaction Use-Cases](#trasaction-use-cases)
    - [How to identify sender of transaction?](#how-to-identify-sender-of-transaction)
  - [NFT Use-Cases](#nft-use-cases)
    - [Count all SpaceBudz ever minted](#count-all-spacebudz-ever-minted)
    - [Count all Transactions where SpaceBudz were involved](#count-all-transactions-where-spacebudz-were-involved)
    - [Count all SpaceBudz in existence (amount of tokens)](#count-all-spacebudz-in-existence-amount-of-tokens)
    - [Contains Search of NFT metadata (Transaction Metadata)](#contains-search-of-nft-metadata-transaction-metadata)
  - [API Tokens](#api-tokens)
    - [API Key Headers and API Key Parameter](#api-key-headers-and-api-key-parameter)
    - [Public API-Key](#public-api-key)
    - [Private API-Key](#private-api-key)
  - [Known Problems and Issues](#known-problems-and-issues)
    - [When i have testnet swagger and mainnet swagger open in the browser the request seems to go to the wrong server ?](#when-i-have-testnet-swagger-and-mainnet-swagger-open-in-the-browser-the-request-seems-to-go-to-the-wrong-server-)
    - [I am receiving old data or no results ?](#i-am-receiving-old-data-or-no-results-)
## A Free Cardano Public API - What can i do with it ?
Retrieve all Cardano blockchain data, as you could from a local database, but via REST. OData Syntax allows you to get the data you need for your application. Start building your application today, don't deal with infrastructure. It is free to use and we even prepared [example queries](https://github.com/tigrpoolcom/cardano-web-api/blob/main/README.md#nft-use-cases) for you.
To give you an impression of whats possible in the following you will see possible use-cases we have gathered for you. The API can be freely used by everyone. We offer an api for `testnet` and one for the `mainnet`. API Documentation is done in Swagger, to make it easy to try it out directly.
If you're new to Swagger you find an introduction to Swagger [here](https://www.youtube.com/watch?v=7MS1Z_1c5CU&list=PLnBvgoOXZNCOiV54qjDOPA9R7DIDazxBA&index=1).

Our `testnet` API is here:
https://testnet.cutymals.com/swagger/index.html

Our `mainnet` API is here:
https://mainnet.cutymals.com/swagger/index.html


With a click on the `Authorize` button on the Swagger UI
Enter in the field `Value:` the public api key `ILoveCutyMals`. 
Then click on Authorize. Swagger then automatically sends the `X-API-KEY=ILoveCutyMals` header in every request you try out. That makes it very easy to understand what you can do with the API.

## What is OData ?
OData allows you essentially to access a database as if it's your own.

The Api allows you to:
- `$filter` results based on predicates like greater than, equals, ...
- `$count` the number of results for your query
- `$select` the return values you need,
- `$orderby` values within your request
- `$top` limit the amount of returned results
- `$skip` navigate through the returned results
- `$expand` navigation properties of related tables

With these options you can adjust the response to your application needs.This reduces preprocessing overhead on your client side and allows direct integration for several types of applications. You can build a Minter, NFT-Showcase page, a game based on Cardano transactions or even run analytics with the data. It's up to you! 

See the possibilities on the [example queries section](https://github.com/tigrpoolcom/cardano-web-api/blob/main/README.md#nft-use-cases).


Further information about ODATA you can find in [Microsoft docs](https://docs.microsoft.com/en-us/odata/concepts/queryoptions-overview).

## Trasaction Use-Cases
### How to identify sender of transaction?
Assuming you have the following UTXO view and want to know which address did sent that amount.
|TxHash|TxIx|Amount|
|------|----|------|
|253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055|0|17622402 lovelace|

To identify the sender we take a look at the TransactionOut APIs. Every Output once in a time will be a input. While the input table on the cardano blockchain is just a link-table with ids, the output table contains also substantial information.

[-> Result](https://mainnet.cutymals.com/odata/TransactionsOut?txHashInHex=253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055&includeTxSender=true&includeTxReceiver=false&X-API-KEY=ILoveCutyMals)
```
https://mainnet.cutymals.com/odata/TransactionsOut?txHashInHex=253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055&includeTxSender=true&includeTxReceiver=false&X-API-KEY=ILoveCutyMals
```
Result (shortened)
```
  "@odata.context": "https://mainnet.cutymals.com/odata/$metadata#TransactionsOut",
  "value": [
    {
      "Id": 10686877,
      "TxId": 4470038,
      "Index": 0,
      "Address": "addr1q95cftm4jhf6gm4exl7f5fsk7n0djsldfa8wy7djrlj2lj6l2eqqxwvrhpmrdvkg0sfa73uywedv34ap6f3r77egfppsxqtuyk",
      ...
    },
    {
      "Id": 10691409,
      "TxId": 4471782,
      ...
  ]
```
We now got returned two Transactions as both were used as input for the Transactions [Verify this on Cardanoscan](https://cardanoscan.io/transaction/253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055). 

As we are just interested in the Address of the sender we will use `$select=Address` to just get returned the Address.

[-> Result](https://mainnet.cutymals.com/odata/TransactionsOut?txHashInHex=253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055&includeTxSender=true&includeTxReceiver=false&X-API-KEY=ILoveCutyMals&%24select=Address)
```
https://mainnet.cutymals.com/odata/TransactionsOut?txHashInHex=253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055&includeTxSender=true&includeTxReceiver=false&X-API-KEY=ILoveCutyMals&%24select=Address
```
Result
```
  "@odata.context": "https://mainnet.cutymals.com/odata/$metadata#TransactionsOut(Address)",
  "value": [
    {
      "Address": "addr1q95cftm4jhf6gm4exl7f5fsk7n0djsldfa8wy7djrlj2lj6l2eqqxwvrhpmrdvkg0sfa73uywedv34ap6f3r77egfppsxqtuyk"
    },
    {
      "Address": "addr1q95cftm4jhf6gm4exl7f5fsk7n0djsldfa8wy7djrlj2lj6l2eqqxwvrhpmrdvkg0sfa73uywedv34ap6f3r77egfppsxqtuyk"
    }
  ]
```

This output is now much cleaner and more suitable for out use case.

If a Transaction is just from one wallet (very likely to happen on every manual tx) we can assume that the input addresses always belong to the same wallet. Hence we can optimize the query even further by just taking the first result.

To do that we will optimize the query and add a `$top=1`, so we just get returned the ffirst result.

[-> Result](https://mainnet.cutymals.com/odata/TransactionsOut?txHashInHex=253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055&includeTxSender=true&includeTxReceiver=false&X-API-KEY=ILoveCutyMals&%24top=1&%24select=Address)
```
https://mainnet.cutymals.com/odata/TransactionsOut?txHashInHex=253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055&includeTxSender=true&includeTxReceiver=false&X-API-KEY=ILoveCutyMals&%24top=1&%24select=Address
```
Result
```
 "@odata.context": "https://mainnet.cutymals.com/odata/$metadata#TransactionsOut(Address)",
  "value": [
    {
      "Address": "addr1q95cftm4jhf6gm4exl7f5fsk7n0djsldfa8wy7djrlj2lj6l2eqqxwvrhpmrdvkg0sfa73uywedv34ap6f3r77egfppsxqtuyk"
    }
  ]
```

With that query it's now super easy to just consume that in javascript and to react to that. Without having to build a full domain model of the TxOut. Use the query which suits best to your use case 👍.


## NFT Use-Cases
### Count all SpaceBudz ever minted
[-> Result](https://mainnet.cutymals.com/odata/MultiAssetTransactionsMint?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&X-API-KEY=ILoveCutyMals&%24count=true)
```
https://mainnet.cutymals.com/odata/MultiAssetTransactionsMint?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&X-API-KEY=ILoveCutyMals&%24count=true
```
Result
```
@odata.context	"https://mainnet.cutymals.com/odata/$metadata#MultiAssetTransactionsMint"
@odata.count	10174
value	
0	
Id	1285
Policy	"1ea/BQA3jU8NpOjd5r7Ox2Ic2Mv1y7m4cBPUzA=="
PolicyInHex	"d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc"
Name	"U3BhY2VCdWQ2MjA2"
NameInHex	"537061636542756436323036"
NameInUtf8	"SpaceBud6206"
Quantity	1
TxId	5365308
1	{…}
2	{…}
3	{…}
4	{…}
5	{…}
6	{…}
7	{…}
8	{…}
9	{…}
@odata.nextLink	"https://mainnet.cutymals.com/odata/MultiAssetTransactionsMint?policyinhex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&x-api-key=ILoveCutyMals&$count=true&$skiptoken=Id-1296"
```
You will see that the count is greater than 10000 pcs. The reason for that is that the MultiAssetTransactionsMint API shows you all mints that took place. On Cardano you can mint and burn tokens, but each is a mint event.
If you mint 150 tokens, then burn 50 tokens (each of those in one transaction) you will then see 200 mint transaction.

If you instead want to know the existing circulation read the next.

### Count all Transactions where SpaceBudz were involved
[-> Result](https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&X-API-KEY=ILoveCutyMals&%24count=true)
```
https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&X-API-KEY=ILoveCutyMals&%24count=true
```
Result
```
@odata.context	"https://mainnet.cutymals.com/odata/$metadata#MultiAssetTransactionsOut"
@odata.count	998869
value	
0	{…}
1	{…}
2	{…}
3	{…}
4	{…}
5	{…}
6	{…}
7	{…}
8	{…}
9	{…}
@odata.nextLink	"https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyinhex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&x-api-key=ILoveCutyMals&$count=true&$skiptoken=Id-44276"
```
We get the data returned together with the @odata.count which is the amount of unique elements for our query. If we ar just interested in the number, we can adjust the query with the parameter $top=0 to return no values.

[-> Result](https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&X-API-KEY=ILoveCutyMals&%24top=0&%24count=true)
```
https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&X-API-KEY=ILoveCutyMals&%24top=0&%24count=true
```
Result
```
@odata.context	"https://mainnet.cutymals.com/odata/$metadata#MultiAssetTransactionsOut"
@odata.count	998869
value	[]
```
We spared us some traffic and just got the count which we now can use to do what we intended to do.

### Count all SpaceBudz in existence (amount of tokens)
To count all in existence we have to count the amount of spendable SpaceBudz on the Blockchain.
 to be done

### Contains Search of NFT metadata (Transaction Metadata)
Example NFT SpaceBud6206
```
"{\"d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc\": {\"SpaceBud6206\": {\"name\": \"SpaceBud #6206\", \"type\": \"Alien\", \"image\": \"ipfs://QmXjMxYcpeGAfSogZmkw5iPehRt55WaWQmC7ti4LCnL9Do\", \"traits\": [\"Chestplate\", \"Covered Helmet\", \"Eye Patch\", \"Lightsaber\", \"Wool Boots\"], \"arweaveId\": \"JK8ytLq7I25qTbkhBNhQGg-X8QSo_6iGA1tfLN2WAkQ\"}}}"
```
We offer you the ability to search the JSON string within the database with a contains search. That means whatever you put into the `searchInMetadata=` parameter will be searched within the Json.

[-> Result](https://mainnet.cutymals.com/odata/TransactionsMetadata?searchInMetadata=SpaceBud6206&X-API-KEY=ILoveCutyMals)
```
https://mainnet.cutymals.com/odata/TransactionsMetadata?searchInMetadata=SpaceBud6206&X-API-KEY=ILoveCutyMals
```
Result 
```
@odata.context	"https://mainnet.cutymals.com/odata/$metadata#TransactionsMetadata"
value	
0	
Id	67784
Key	721
Json	"{\"d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc\": {\"SpaceBud6206\": {\"name\": \"SpaceBud #6206\", \"type\": \"Alien\", \"image\": \"ipfs://QmXjMxYcpeGAfSogZmkw5iPehRt55WaWQmC7ti4LCnL9Do\", \"traits\": [\"Chestplate\", \"Covered Helmet\", \"Eye Patch\", \"Lightsaber\", \"Wool Boots\"], \"arweaveId\": \"JK8ytLq7I25qTbkhBNhQGg-X8QSo_6iGA1tfLN2WAkQ\"}}}"
Bytes	"oRkC0aF4OGQ1ZTZiZjA1MDAzNzhkNGYwZGE0ZThkZGU2YmVjZWM3NjIxY2Q4Y2JmNWNiYjliODcwMTNkNGNjoWxTcGFjZUJ1ZDYyMDalaWFyd2VhdmVJZHgrSks4eXRMcTdJMjVxVGJraEJOaFFHZy1YOFFTb182aUdBMXRmTE4yV0FrUWVpbWFnZXg1aXBmczovL1FtWGpNeFljcGVHQWZTb2dabWt3NWlQZWhSdDU1V2FXUW1DN3RpNExDbkw5RG9kbmFtZW5TcGFjZUJ1ZCAjNjIwNmZ0cmFpdHOFakNoZXN0cGxhdGVuQ292ZXJlZCBIZWxtZXRpRXllIFBhdGNoakxpZ2h0c2FiZXJqV29vbCBCb290c2R0eXBlZUFsaWVu"
TxId	5365308
1	{…}
2	{…}
```

## API Tokens 
### API Key Headers and API Key Parameter
To request data from the API you have to use the X-API-KEY parameter.
Our API Allows you to either set it as authentication header:
```
curl -X 'GET' \ 'https://mainnet.cutymals.com/api/CardanoSyncStatus' \
  -H 'X-API-KEY: ILoveCutyMals'
```
or set it as a GET parameter:
```
curl -X 'GET' \  'https://mainnet.cutymals.com/api/CardanoSyncStatus?X-API-KEY=ILoveCutyMals' \
```
We allow both for the sake of convenience and better testing. As long as you try out the API feel free to use the GET Parameter.

When you shift to production and register a private token, we advise to sent it via an Authentication Header as it's considered more secure and encrypted by HTTPs.

Security Tip: If you want to use our API within a website of yours (e.g. to count the amount of some NFTs, always use the public API-Key). Only use your private API-Key if you have a backend in place, which ensures a safe storage of the key.


### Public API-Key
The public API-Key is 
```
X-API-KEY=ILoveCutyMals
```
You can pass it either as Header or as a get parameter.
You can use it to test out the API and embedd it on public facing frontends, where you cant safely store a private key. One example would be a small counter on your homepage, which relies on Cardano blockchain data.

The Public API-Key is also used for our example queries.

### Private API-Key
It will be possible to register a private tokens to connect to the API. This will allow you to call the API more heavily and without interceptions of other users.
The API-Key will be bound to CutyMal tokens on the Cardano blockchain.

## Known Problems and Issues
### When i have testnet swagger and mainnet swagger open in the browser the request seems to go to the wrong server ?
It seems that Swagger cant distinguish between the API of `testnet.cutymals.com` and `mainnet.cutymals.com`. It uses one cache for both Swagger pages.

This leads to the issue that a request in the mainnet Swagger can go to the testnet APi (or other way around),because the cache always holds the target server of the last API you have opened.
As this is just a caching problem from Swagger, you will only encounter this in local tests.
You can solve this by pressing `STRG+F5` in the current tab so that Swagger overwrites the target server in your browser cache and the request goes to the right server.

Our `testnet` API is here:
https://testnet.cutymals.com/swagger/index.html

Our `mainnet` API is here:
https://mainnet.cutymals.com/swagger/index.html

<b>Note:</b> We mainly observed that in Firefox, while Edge and Chrome did not have this behavior.

### I am receiving old data or no results ?
Due to maintenance or other unforseen issues it might happen that the sync between the cardano relay node and the network is not 100%.
To give you the possibility to react to such event we offer the `/api/CardanoSyncStatus` route. With that you can check if the nodes are synced 100%. The syncedTimestamp is in UTC.

Direct Link for `testnet`:
https://testnet.cutymals.com/api/CardanoSyncStatus?X-API-KEY=ILoveCutyMals

Example Result
````
{
"syncedTimestamp":"2021-30-11T06:57:46",
"syncedInPercent":99.99999999999999
}
````

Direct link for `mainnet`
https://mainnet.cutymals.com/api/CardanoSyncStatus?X-API-KEY=ILoveCutyMals

Example Result
````
{
"syncedTimestamp":"2021-30-11T07:00:00",
"syncedInPercent":100.00000000000000
}
````
