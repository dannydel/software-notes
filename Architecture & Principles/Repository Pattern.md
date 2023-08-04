[Repository Pattern - Erik Bergman (Medium)](https://medium.com/@pererikbergman/repository-design-pattern-e28c0f3e4a30)
[Microsoft - Designing the Infrastructure Persistence Layer](https://learn.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)
[Code Maze - ASP.NET Core Web API - Repository Pattern](https://code-maze.com/net-core-web-development-part4/)

The pattern that states abstracting the data layer away so that you can be data storage agnostic. Making CRUD functionality very reusable and isn't determinate on if you are using a Relational Database or a Non Relational Database. 

### Opinion 
A lot of people disagree with this notion but [[Entity Framework Core]] is very much a Repository Pattern in itself. Some people believe that you need another layer of abstraction on top of that to keep developers safe from using some methods and functions within, making safe patterns and a cleaner code base. But, in my belief, this is too restrictive and EF Core is completely able to allow developers access data without fear of obnoxious code overuse. The main idea behind a Repository Pattern is really just a way to separate the concern of the data layer and allow the Repo to determine how and what it is connecting to as well as showing developers the necessary CRUD functions required to do the job.
