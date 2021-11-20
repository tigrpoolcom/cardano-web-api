# CutyMals is cute animals on blockchain!
## Public Cardano Web API
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

## OData
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

## Example Queries 
