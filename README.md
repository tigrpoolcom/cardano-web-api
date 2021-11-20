
# Cardano Public API brought you by CutyMals.com
- [Cardano Public API brought you by CutyMals.com](#cardano-public-api-brought-you-by-cutymalscom)
  - [What can i do with it ?](#what-can-i-do-with-it-)
  - [What is OData ?](#what-is-odata-)
  - [NFT Use-Cases](#nft-use-cases)
    - [Count all SpaceBudz ever minted](#count-all-spacebudz-ever-minted)
    - [Count all SpaceBudz in existence](#count-all-spacebudz-in-existence)
  - [API Tokens](#api-tokens)
    - [API Key Headers and API Key Parameter](#api-key-headers-and-api-key-parameter)
    - [Public API-Key](#public-api-key)
    - [Private API-Key](#private-api-key)
## What can i do with it ?
Retrieve cardano blockchain data in a flexible ODATA Syntax. It allows you to get the data you need for your application with no hassle and maximum flexibility.Start building your application today, don't deal with infrastructure. We prepared examples queries for you. Example Queries
The Api allows you to:
- `$filter` results based on predicates like greater than, equals, ...
- `$count` the number of results for your query
- `$select` the return values you need,
- `$orderby` values within your request
- `$top` limit the amount of returned results
- `$skip` navigate through the returned results
- `$expand` navigation properties of related tables
Fast as Lightning directly executed on the database. 

[Go To Public Cardano Web API](https://cutymals.com/swagger/index.html)

## What is OData ?
Here are the various parameters that can be used together with API URI in request.

    $expand
    $filter
    $inlinecount
    $orderby
    $select
    $skip
    $top

Here are the examples of some the most used parameter with web API URI.

    http://localhost/api/Employees?$expand=DeptId
    http://localhost/api/Employees?$filter=Id ge 5 and Price le 15
    http://localhost/api/Employees?$inlinecount=allpages
    http://localhost/api/Employees?$orderby=Id desc
    http://localhost/api/Employees?$select=EmployeeName, Salary
    http://localhost/api/Employees?$skip=10
    http://localhost/api/Employees?$top=5

## NFT Use-Cases
### Count all SpaceBudz ever minted
[-> Result](https://cutymals.com/odata/MultiAssetTransactionsMint?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&X-API-KEY=ILoveCutyMals&%24count=true)
```
https://cutymals.com/odata/MultiAssetTransactionsMint?policyInHex=d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc&X-API-KEY=ILoveCutyMals&%24count=true
```
You will see that the count is greater than 10000 pcs. The reason for that is that the MultiAssetTransactionsMint API shows you all mints that took place. On Cardano you can mint and burn tokens, but each is a mint event.
If you mint 150 tokens, then burn 50 tokens (each of those in one transaction) you will then see 200 mint transaction.

If you instead want to know the existing circulation read the next.

### Count all SpaceBudz in existence
[-> Result]()
```

```

to be added

## API Tokens 
### API Key Headers and API Key Parameter
To request data from the API you have to use the X-API-KEY parameter.
Our API Allows you to either set it as authentication header:
```
curl -X 'GET' \ 'https://cutymals.com/api/CardanoSyncStatus' \
  -H 'X-API-KEY: ILoveCutyMals'
```
or set it as a GET parameter:
```
curl -X 'GET' \  'https://cutymals.com/api/CardanoSyncStatus?X-API-KEY=ILoveCutyMals' \
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

