[Dotnet Docs - ErrorBoundary Blazor Documentation](https://learn.microsoft.com/en-us/aspnet/core/blazor/fundamentals/handle-errors?view=aspnetcore-7.0)

Creating an error boundary is very simple within the Blazor .Net space.
Here are the steps to implement Error Boundary:

1. Go to MainLayout.razor and locate where the @Body is. 
2. Now surround the body with the ErrorBoundary component like below:
```C# Razor
<ErrorBoundary @ref="errorBoundary">
	@Body
</ErrorBoundary>

```

3. Next add a private nullable ErrorBoundary variable and name it like the above ref: "errorBoundary".
```C# Razor
@code{
	private ErrorBoundary? errorBoundary;

	protected override void OnParametersSet()
	{
		errorBoundary?.Recover();
	}
}

```
4. Next add in the OnParametersSet hook and set the errorboundary to recover. See Above:
5. Next we will want to open the Error.cshtml and Error.cshtml.cs pages.
6. Navigate to the .cs page and notice the following code:
```C# 
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
[IgnoreAntiforgeryToken]
public class ErrorModel : PageModel
{
	public string? RequestId { get; set; }

	public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

	private readonly ILogger<ErrorModel> _logger;

	public ErrorModel(ILogger<ErrorModel> logger)
	{
		_logger = logger;
	}

	public void OnGet()
	{
		RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
	}
}

```

7. Lets break this down and note what each thing is doing in this code.
	1. Response caching reduces the number of requests a client or proxy makes to a web server. Response caching also reduces the amount of work the web server performs to generate a response. Response caching is set in headers. Since this page is only for errors we aren't worried about caching right now.
	2. IgnoreAntiforgeryToken - this attribute is telling the routing system we don't have to be logged in to see this page, this page can be shown to anyone given the received an error.
	3. The code within the ErrorModel class is just setup in case we want to do logging around the error boundary being run.