# LINQ-Queries
This contains samples how to write complex LINQ queries &amp; generated SQLs

1. Left outer join with aggregate function and other columns.
```SQL
SELECT [e].[EmployeeId], [e].[FirstName], [e].[LastName], COALESCE(SUM([e0].[Incentive]), 0) AS [Sum]
FROM [Employee] AS [e]
LEFT JOIN [EmployeeIncentive] AS [e0] ON [e].[EmployeeId] = [e0].[EmployeeId]
GROUP BY [e].[EmployeeId], [e].[FirstName], [e].[LastName]
```
```csharp
from employee in _employeeDbContext.Employees
join incentive in _employeeDbContext.EmployeeIncentives
on employee.EmployeeId equals incentive.EmployeeId into j1
from j2 in j1.DefaultIfEmpty()
//let f = employee.FirstName + employee.LastName
let x = new { employee.EmployeeId, employee.FirstName, employee.LastName, j2.Incentive }

group x by new { x.EmployeeId, x.FirstName, x.LastName } into grouped
select new {
    grouped.Key,
    Sum = grouped.Sum(x => x.Incentive)
};
```
