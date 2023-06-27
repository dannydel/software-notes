#dependencyinjection

## Helpful Links
[Dependency Injection - Docs](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-7.0)

## Overview
A dependency injection is an object that another object depends on. Usually these necessary objects are passed in to objects that require it to execute correctly. 

Dependency Injection solves the following problems with
- The use of an interface or base class to abstract the dependency implementation.
- Registration of the dependency in a service container. ASP.NET Core provides a built-in service container, [IServiceProvider](https://learn.microsoft.com/en-us/dotnet/api/system.iserviceprovider). Services are typically registered in the app's `Program.cs` file.
- _Injection_ of the service into the constructor of the class where it's used. The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.

### Implementing Dependency Injection

Lets say for example you have a Blazor page:
```C#
//UserPage.razor

@page '/User/{userId}'

<label for="fullname"> Full Name </label>
<input id="fullname" name="fullname" type="text" value="@User.FullName" />

<input type="checbox" value="@User.IsHappy" />

@code{
	[Parameter]
	public int UserId {get;set;}

	protected override void OnInitialized()
	{
		// we need to use UserId to get User from the service
	}
}

```

We would need to instantiate the service within the **Program.cs** of this application like so:

```C#
//Program.cs

var builder = WebApplication.CreateBuilder(args);

...

...

builder.Services.AddRazorPages();

builder.Services.AddScoped<IUserService, UserService>();

var app = builder.Build();

```

As you can see in the 

```
builder.Services.AddScoped<IUserService,UserService>();
```

we are telling the application to set up this service for a lifetime of each request. (See [[#Scoped]])

We registered the UserService successfully and now we can do the following with our Razor page. 

```C# - UserPage
//UserPage.razor

@page '/User/{userId}'
@inject IUserService UserService

<div id="error">
 @_error
</div>

<label for="fullname"> Full Name </label>
<input id="fullname" name="fullname" type="text" value="@_user.FullName" />

<input type="checbox" value="@_user.IsHappy" />

@code{
	[Parameter]
	public int UserId {get;set;}

	private User _user;
	private string? _error;

	protected override async Task OnInitializedAsync()
	{
		if(UserId == 0)
		{
			_error = "No User Id!";
		}

		_user = await UserService.GetUserById(UserId);
		
	}
}
```


### Design Services for dependency injection

When designing services for dependency injection:
- Avoid stateful, static classes and members. Avoid creating global state by designing apps to use singleton services instead.
- Avoid direct instantiation of dependent classes within services. Direct instantiation couples the code to a particular implementation.
- Make services small, well-factored, and easily tested.
- Try to follow the Single Responsibility Principle ([[SRP - Single Responsibility Principle]])

## Dependency Injection Lifetimes
[Microsoft Docs - Dependency Injection Lifetimes](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection#scoped)

#### Scoped - Once Per Request
#scoped

>Scoped lifetime services are created once per request.

##### When to use a scoped service or a good example of one?
- AddDbContext is by default a ***scoped*** service.
- Use a scoped service when a web application needs a lifetime that is created once per client request (or connection).

#### Singleton - Shared among subsequent requests
#singleton

>Singleton lifetime services are created the first time they are requested ( or when ConfigureServices is run if you specify an instance there) and then every subsequent request will use the same instance.

##### When to use a singleton service or a good example of one?
- Logging Services tend to be singleton since they do need to be use over and over again throughout a requests lifetime. It would be wasted resources if it was scoped or transient. 
- State management is also a good example of when to use a Singleton service.

#### Transient - New Service per request.
#transient

>Transient lifetime services are created each time they are requested. The lifetime works best for lightweight, stateless services.

##### When to use a transient service?
- Mainly this is used for if you don't want to expend extra resources keeping services alive for a lifetime. Transient services get spun up and disposed every time they are called. Reducing the risk of memory leaks or needed memory.