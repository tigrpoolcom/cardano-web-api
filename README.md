
# Cardano Public API brought you by CutyMals.com
- [Cardano Public API brought you by CutyMals.com](#cardano-public-api-brought-you-by-cutymalscom)
  - [A Free Cardano Public API - What can i do with it ?](#a-free-cardano-public-api---what-can-i-do-with-it-)
  - [What is OData ?](#what-is-odata-)
  - [Trasaction Use-Cases](#trasaction-use-cases)
    - [How to identify sender of transaction?](#how-to-identify-sender-of-transaction)
  - [NFT Use-Cases](#nft-use-cases)
    - [Count mint events of a NFT (e.g. SpaceBud)](#count-mint-events-of-a-nft-eg-spacebud)
      - [What is a mint event ?](#what-is-a-mint-event-)
    - [Count all Transactions where SpaceBudz were involved](#count-all-transactions-where-spacebudz-were-involved)
    - [Count existing quantity of NFTs (e.g. total SpaceBuds in existence)](#count-existing-quantity-of-nfts-eg-total-spacebuds-in-existence)
    - [Contains Search of NFT metadata (Transaction Metadata)](#contains-search-of-nft-metadata-transaction-metadata)
  - [NFT Volume Data](#nft-volume-data)
    - [Retrieve the NFT Volume data from Cardano Blockchain](#retrieve-the-nft-volume-data-from-cardano-blockchain)
  - [Integration examples](#integration-examples)
    - [Javascript fetch number of NFTs in existence](#javascript-fetch-number-of-nfts-in-existence)
    - [Javascript fetch owner of NFT](#javascript-fetch-owner-of-nft)
  - [IPFS Use-Cases](#ipfs-use-cases)
    - [Retrieve a NFT picture from IPFS](#retrieve-a-nft-picture-from-ipfs)
    - [Embed NFT from IPFS in social media](#embed-nft-from-ipfs-in-social-media)
  - [API Tokens](#api-tokens)
    - [API Key Headers and API Key Parameter](#api-key-headers-and-api-key-parameter)
    - [Public API-Key](#public-api-key)
    - [Private API-Key](#private-api-key)
  - [Known Problems and Issues](#known-problems-and-issues)
    - [When i have testnet swagger and mainnet swagger open in the browser the request seems to go to the wrong server ?](#when-i-have-testnet-swagger-and-mainnet-swagger-open-in-the-browser-the-request-seems-to-go-to-the-wrong-server-)
    - [I am receiving old data or no results ?](#i-am-receiving-old-data-or-no-results-)
    - [I get a 500 Internal Server error or timeout](#i-get-a-500-internal-server-error-or-timeout)
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
Assuming you have the following UTXO view and you want to know which address has sent the `17622402 lovelace`.
|TxHash|TxIx|Amount|
|------|----|------|
|253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055|0|17622402 lovelace|

To identify the sender we take a look at the TransactionOut APIs. Every Output once in a time will be a input. While the input table on the cardano blockchain is just a link-table with ids, the output table contains also substantial information.

[https://mainnet.cutymals.com/odata/TransactionsOut?txHashInHex=253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055&includeTxSender=true&includeTxReceiver=false&X-API-KEY=ILoveCutyMals](https://mainnet.cutymals.com/odata/TransactionsOut?txHashInHex=253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055&includeTxSender=true&includeTxReceiver=false&X-API-KEY=ILoveCutyMals)

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

[https://mainnet.cutymals.com/odata/TransactionsOut?txHashInHex=253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055&includeTxSender=true&includeTxReceiver=false&X-API-KEY=ILoveCutyMals&%24select=Address](https://mainnet.cutymals.com/odata/TransactionsOut?txHashInHex=253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055&includeTxSender=true&includeTxReceiver=false&X-API-KEY=ILoveCutyMals&%24select=Address)

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

[https://mainnet.cutymals.com/odata/TransactionsOut?txHashInHex=253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055&includeTxSender=true&includeTxReceiver=false&X-API-KEY=ILoveCutyMals&%24top=1&%24select=Address](https://mainnet.cutymals.com/odata/TransactionsOut?txHashInHex=253e233a6b262aed75fce1819903f8d56cd31b23d56e204717424451cc287055&includeTxSender=true&includeTxReceiver=false&X-API-KEY=ILoveCutyMals&%24top=1&%24select=Address)
```
 "@odata.context": "https://mainnet.cutymals.com/odata/$metadata#TransactionsOut(Address)",
  "value": [
    {
      "Address": "addr1q95cftm4jhf6gm4exl7f5fsk7n0djsldfa8wy7djrlj2lj6l2eqqxwvrhpmrdvkg0sfa73uywedv34ap6f3r77egfppsxqtuyk"
    }
  ]
```

With that query it's now super easy to just consume that in javascript and to react to that. Without having to build a full domain model of the TxOut. Use the query which suits best to your use case ????.


## NFT Use-Cases
### Count mint events of a NFT (e.g. SpaceBud)
#### What is a mint event ?
Every NFT or Token is brought to live by a mint event. That means a transaction which can be created by the policy owner (creator).
- A token can be burned (e.g. mint quantiy <= -1)
- A token can be minted (e.g mint quantity >= +1)

The following shows how to read such mint events through the `/odata/MultiAssetTransactionsMint` endpoint.

[https://mainnet.cutymals.com/odata/MultiAssetTransactionsMint?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&X-API-KEY=ILoveCutyMals&%24count=true](https://mainnet.cutymals.com/odata/MultiAssetTransactionsMint?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&X-API-KEY=ILoveCutyMals&%24count=true)

```
{
  "@odata.context": "https://mainnet.cutymals.com/odata/$metadata#MultiAssetTransactionsMint",
  "@odata.count": 10174,
  "value": [
    {
      "Id": 1285,
      "Policy": "d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc",
      "Name": "537061636542756436323036",
      "NameInUtf8": "SpaceBud6206",
      "Quantity": 1,
      "TxId": 5365308
    },
    {1},
    {2},
    {3},
    {4},
    {5},
    {6},
    {7},
    {8},
    {
      "Id": 1296,
      "Policy": "d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc",
      "Name": "537061636542756434363333",
      "NameInUtf8": "SpaceBud4633",
      "Quantity": 1,
      "TxId": 5367440
    }
  ],
  "@odata.nextLink": "https://mainnet.cutymals.com/odata/MultiAssetTransactionsMint?policyinhex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&x-api-key=ILoveCutyMals&$count=true&$skiptoken=Id-1296"
}
```
You will see that the count is greater than 10000 pcs. The reason for that is that the MultiAssetTransactionsMint API shows you all mints that took place. On Cardano you can mint and burn tokens, but each is a mint event.
If you mint 150 tokens, then burn 50 tokens (each of those in one transaction) you will then see 200 mint transaction.

If you instead want to know the existing circulation read the next.

### Count all Transactions where SpaceBudz were involved
[https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&X-API-KEY=ILoveCutyMals&%24count=true](https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&X-API-KEY=ILoveCutyMals&%24count=true)
Result
```
@odata.context	"https://mainnet.cutymals.com/odata/$metadata#MultiAssetTransactionsOut"
@odata.count	998869
value	
0	{???}
1	{???}
2	{???}
3	{???}
4	{???}
5	{???}
6	{???}
7	{???}
8	{???}
9	{???}
@odata.nextLink	"https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyinhex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&x-api-key=ILoveCutyMals&$count=true&$skiptoken=Id-44276"
```
We get the data returned together with the @odata.count which is the amount of unique elements for our query. If we ar just interested in the number, we can adjust the query with the parameter $top=0 to return no values.

[https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&X-API-KEY=ILoveCutyMals&%24top=0&%24count=true](https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&X-API-KEY=ILoveCutyMals&%24top=0&%24count=true)
```
@odata.context	"https://mainnet.cutymals.com/odata/$metadata#MultiAssetTransactionsOut"
@odata.count	998869
value	[]
```
Query Explained:
- policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc (the Policy which is being filtered for)
- $top=0 [%24top=0] (limit to zero results, if only count is relevant, data has not to be sent)
- $count=true [%24count=true] (count all results, shown as @odata.count)

### Count existing quantity of NFTs (e.g. total SpaceBuds in existence)
The following query returns all SpaceBuds which are unspent. A token can be minted and destroyed during his lifetime. This does often happen if during the mint phase a token was minted twice, one of the created token is then usually being removed. This is done by a transaction in which the token is being used, without creating a new spendable output.

So with this Query we see the actual amount of existing tokens on the chain. If one is destroyed or created the count will change.

Hint: The MultiTransactionsOut table is the largest table on the cardano blockchain with more than 100 million entries. Using `unspent=true` will compare entries on the MultiTransactionsOut table with the MultiTransactionsIn table (second largest table).
If the requests takes more than 60 seconds, it's canceled.
Try one minute later.


[https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&unspentOnly=true&X-API-KEY=ILoveCutyMals&%24count=true](https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&unspentOnly=true&X-API-KEY=ILoveCutyMals&%24count=true)

Result 
```
{
  "@odata.context": "https://mainnet.cutymals.com/odata/$metadata#MultiAssetTransactionsOut",
  "@odata.count": 10002,
  "value": [
    {
      "Id": 44291,
      "Policy": "d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc",
      "Name": "537061636542756431323330",
      "NameInUtf8": "SpaceBud1230",
      "Quantity": 1,
      "TxOutId": 12945321
    }, ...
  ]
}
```
The result shows us that at the time of writing 2021-12-11 there are 10002 spendable SpaceBudz in existence. Thats interesting because [Cardanoscan.io](https://cardanoscan.io/tokenPolicy/d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc) tells us that there are 10000 tokens (ignoring the quantity).

We set up a small script which requests the count for every SpaceBud and iterated that in a foor loop. By doing that we get the following entries which shed a light on that.

[SpaceBud1903](https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&nameInUTF8=SpaceBud1903&unspentOnly=true&X-API-KEY=ILoveCutyMals&%24count=true)

[SpaceBud6413](https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&nameInUTF8=SpaceBud6413&unspentOnly=true&X-API-KEY=ILoveCutyMals&%24count=true)

```
SpaceBud1903
{"@odata.context":"https://...", "@odata.count":2,"value":[...]}

SpaceBud6413
{"@odata.context":"https://...", "@odata.count":2,"value":[...]}
```
There are two SpaceBudz in existence which have a quantity of two.Hence the total amount of SpaceBudz at the time of writing this 2021-12-11 is 10002 SpaceBudz.

Blockchain data is interesting! ????

### Contains Search of NFT metadata (Transaction Metadata)
Example NFT SpaceBud6206
```
"{\"d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc\": {\"SpaceBud6206\": {\"name\": \"SpaceBud #6206\", \"type\": \"Alien\", \"image\": \"ipfs://QmXjMxYcpeGAfSogZmkw5iPehRt55WaWQmC7ti4LCnL9Do\", \"traits\": [\"Chestplate\", \"Covered Helmet\", \"Eye Patch\", \"Lightsaber\", \"Wool Boots\"], \"arweaveId\": \"JK8ytLq7I25qTbkhBNhQGg-X8QSo_6iGA1tfLN2WAkQ\"}}}"
```
We offer you the ability to search the JSON string within the database with a contains search. That means whatever you put into the `searchInMetadata=` parameter will be searched within the Json.

[https://mainnet.cutymals.com/odata/TransactionsMetadata?searchInMetadata=SpaceBud6206&X-API-KEY=ILoveCutyMals](https://mainnet.cutymals.com/odata/TransactionsMetadata?searchInMetadata=SpaceBud6206&X-API-KEY=ILoveCutyMals)

 
```
{
  "@odata.context": "https://mainnet.cutymals.com/odata/$metadata#TransactionsMetadata",
  "value": [
    {
      "Id": 67784,
      "Key": 721,
      "Json": "{\"d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc\": {\"SpaceBud6206\": {\"name\": \"SpaceBud #6206\", \"type\": \"Alien\", \"image\": \"ipfs://QmXjMxYcpeGAfSogZmkw5iPehRt55WaWQmC7ti4LCnL9Do\", \"traits\": [\"Chestplate\", \"Covered Helmet\", \"Eye Patch\", \"Lightsaber\", \"Wool Boots\"], \"arweaveId\": \"JK8ytLq7I25qTbkhBNhQGg-X8QSo_6iGA1tfLN2WAkQ\"}}}",
      "Bytes": "a11902d1a178386435653662663035303033373864346630646134653864646536626563656337363231636438636266356362623962383730313364346363a16c537061636542756436323036a569617277656176654964782b4a4b3879744c71374932357154626b68424e685147672d583851536f5f366947413174664c4e3257416b5165696d6167657835697066733a2f2f516d586a4d7859637065474166536f675a6d6b77356950656852743535576157516d43377469344c436e4c39446f646e616d656e537061636542756420233632303666747261697473856a4368657374706c6174656e436f76657265642048656c6d6574694579652050617463686a4c6967687473616265726a576f6f6c20426f6f7473647479706565416c69656e",
      "TxId": 5365308
    },
	{1},
	{2}
  ]
}
```
## NFT Volume Data 
### Retrieve the NFT Volume data from Cardano Blockchain 
It's possible to retreive the NFT Volume based on sent transactions from the API.



|Type|Name|Description |Example|
|---|---|---|---|
|Route|`api/Reports/{policyID}` | The Endpoint | api/Reports/b84d709a29b5f2f0f79d48941df55d3e5823a1ecc290a6091d1f6841
|Parameter| {policyId}|The PolicyId for which the report is requested| b84d709a29b5f2f0f79d48941df55d3e5823a1ecc290a6091d1f6841
|Parameter (List)|tSpan | Timespan in which you want the Data (Days:Hours:Minutes:Seconds) | tSpan=60:00:05:00&tSpan=30:00:05:00&tSpan=07:00:15:00&tSpan=00:24:00:00&tSpan=00:00:06:00

Route `api/Reports/{policyID}`
Parameter: `tSp


[https://mainnet.cutymals.com/api/Reports/b84d709a29b5f2f0f79d48941df55d3e5823a1ecc290a6091d1f6841?tSpan=60%3A00%3A05%3A00&tSpan=30%3A00%3A05%3A00&tSpan=07%3A00%3A15%3A00&tSpan=00%3A24%3A00%3A00&tSpan=00%3A00%3A06%3A00&X-API-KEY=ILoveCutyMals](https://mainnet.cutymals.com/api/Reports/b84d709a29b5f2f0f79d48941df55d3e5823a1ecc290a6091d1f6841?tSpan=60%3A00%3A05%3A00&tSpan=30%3A00%3A05%3A00&tSpan=07%3A00%3A15%3A00&tSpan=00%3A24%3A00%3A00&tSpan=00%3A00%3A06%3A00&X-API-KEY=ILoveCutyMals)

 
```
{
  "PolicyId": "b84d709a29b5f2f0f79d48941df55d3e5823a1ecc290a6091d1f6841",
  "ReportDate": "2022-02-11T14:18:50.9200008+00:00",
  "TimeSpans": [
    {
      "TimeSpan": "60.00:05:00",
      "VolumeLoveLace": 549848494910531800,
      "VolumeNFTs": 621754
    },
    {
      "TimeSpan": "30.00:05:00",
      "VolumeLoveLace": 485133365140004860,
      "VolumeNFTs": 470175
    },
    {
      "TimeSpan": "7.00:15:00",
      "VolumeLoveLace": 71902742269153190,
      "VolumeNFTs": 69971
    },
    {
      "TimeSpan": "00:06:00",
      "VolumeLoveLace": 0,
      "VolumeNFTs": 0
    }
  ]
}
```

`VolumeLoveLace` is the amount of ADA in Lovelace which sent with the NFT to another wallet
`VolumeNFTs` is the amount of NFTS in their Quantity which were sent to another wallet
## Integration examples
### Javascript fetch number of NFTs in existence
The following is a codesnippet we use to identify how much tokens are minted already. We use the MATM (MultiAssetTransactionsMint) for that, it would also be possible via the MATO (MultiAssetTransactionsOut) table. We use the MATM because that table is smaller than the MATO table, and has faster response time. Downside of using MATM is that it only gives you the count of MINT transactions for a token, if you burned some or use a quantitiy different from one, you end up having wrong results.

```
async function GetNumberMinted() {
  let url = "https://mainnet.cutymals.com/odata/MultiAssetTransactionsMint?policyInHex=b84d709a29b5f2f0f79d48941df55d3e5823a1ecc290a6091d1f6841&X-API-KEY=ILoveCutyMals&%24top=0&%24count=true";
  return fetch(url ,{method: 'GET', headers: {
    "Accept": "application/json"
  }})
  .then(res =>{if(res.ok) return res.json(); })
  .then(data => return data['@odata.count']})
  .catch(err => content = "ERROR");
}
```
1. Get the data from the API
2. Check if status is OK
3. Parse to JSON
4. Select the count value and return it

Note: If you burned tokens or have more than one quantity per Asset, you might want to use the MATO table instead, because it shows you always how much NFTs are in existence.

### Javascript fetch owner of NFT

The following is a codesnippet we use to identify who is the owner the NFT shown on our page.
You can use that in the same way for your NFT project, to identify the current owner of an NFT.

Note: Thas just a snippet, to show the possibilities. It does not follow any style guide.
```
async function FindCurrentOwner()
  {
  let url = `https://mainnet.cutymals.com/odata/MultiAssetTransactionsOut?policyInHex=b84d709a29b5f2f0f79d48941df55d3e5823a1ecc290a6091d1f6841&nameInUTF8=${animal.assetName}&unspentOnly=true&X-API-KEY=ILoveCutyMals&%24top=1&%24select=TxOut&%24count=true&%24expand=TxOut`;

  await fetch(url ,{method: 'GET', headers: {
    "Accept": "application/json"
  }})
  .then(res =>{if(res.ok) return res.json(); })
  .then(data => 
  {
      return data.value[0].TxOut.Address; 
  })
  .catch(err => content = "ERROR");
}
```
1. Get the data from the API
2. Check if status is OK
3. Parse to JSON
4. Select the first value array, select the address from the TxOut
5. Return the value



## IPFS Use-Cases
### Retrieve a NFT picture from IPFS
You can retrieve a picture which is stored in IPFS via the following call

[https://mainnet.cutymals.com/api/Ipfs/QmTL6PY3sxcGwQz77SQbnx1YFnNtaPM2XzrpuX6Z2eRBHr?X-API-KEY=ILoveCutyMals](https://mainnet.cutymals.com/api/Ipfs/QmTL6PY3sxcGwQz77SQbnx1YFnNtaPM2XzrpuX6Z2eRBHr?X-API-KEY=ILoveCutyMals)

Parameters Explained:
- ipfsHash=QmTL6PY3sxcGwQz77SQbnx1YFnNtaPM2XzrpuX6Z2eRBHr (the IPFS file which will be retrieved, replace with your IPFS file)
- Returns image via FileStream

*Supported Values*: bmp,gif,ico,jpeg,jpg,png,psd,tiff

*Non-Supported*: Returns 404 for all other values

### Embed NFT from IPFS in social media
If a picture is shared in social media a preview thumnail is generated. This is done via open graph meta tags.

The following call returns the IPFS image embedded into HTML and includes this meta tags.

[https://mainnet.cutymals.com/api/Ipfs/Share/QmTL6PY3sxcGwQz77SQbnx1YFnNtaPM2XzrpuX6Z2eRBHr](https://mainnet.cutymals.com/api/Ipfs/Share/QmTL6PY3sxcGwQz77SQbnx1YFnNtaPM2XzrpuX6Z2eRBHr)

Parameters Explained:
- ipfsHash=QmTL6PY3sxcGwQz77SQbnx1YFnNtaPM2XzrpuX6Z2eRBHr (the IPFS file which will be retrieved, replace with your IPFS file)
- No API Key is necessary for the /api/Ipfs/Share endpoint
- Returns image embedded into HTML


*Supported Values*: bmp,gif,ico,jpeg,jpg,png,psd,tiff

*Non-Supported*: Returns 404 for all other values


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
### I get a 500 Internal Server error or timeout
If a lot of action happens on the blockchain this data is also being written in the underlying database. 
It can happen from time to time that the database has to reorder some indices, due to a lot of inserts recently.
E.g. this happens mainly on the MultiAssetTranstionsOutput tables at the moment.
Best you can do is to retry the query some minutes later.

We are still investigating how we can improve that.

Question not answered ?
[Contact us on Twitter!](https://twitter.com/CutymalsCom)
