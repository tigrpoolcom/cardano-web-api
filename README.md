# CutyMals is cute animals on blockchain!
## Cardano Web API
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
