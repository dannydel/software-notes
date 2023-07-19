# Entity Framework

## Resources/Cool Posts
[Entity Framework Features I wish I knew Earlier](https://timdeschryver.dev/blog/entity-framework-features-i-wish-i-knew-earlier?utm_source=csharpdigest&utm_medium&utm_campaign=1682#autoinclude)
- In this blog post it goes over things like:
  - AutoInclude
  - Single/Split Queries
  - HasQueryFilter
  - ShadowProperties and more!


# EF 7.0

## Interceptors
[EF Core Interceptors](https://learn.microsoft.com/en-us/ef/core/logging-events-diagnostics/interceptors)

Mainly used as a way to intercept a message between the LINQ command and the query engine executing the SQL query. An example of this is creating an interceptor that will turn "Robust Plan" on in SQL by tagging it after the `WHERE` LINQ statement.


