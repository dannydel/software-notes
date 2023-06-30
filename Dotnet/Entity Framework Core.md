# EF 7.0

## Interceptors
[EF Core Interceptors](https://learn.microsoft.com/en-us/ef/core/logging-events-diagnostics/interceptors)

Mainly used as a way to intercept a message between the LINQ command and the query engine executing the SQL query. An example of this is creating an interceptor that will turn "Robust Plan" on in SQL by tagging it after the `WHERE` LINQ statement.
