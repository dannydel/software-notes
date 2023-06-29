## Caching Service

The code below is a daily cache for Census data, mainly the FIPs numbers for states and counties.

Example Code:

```csharp
using System.Timers;

using Timer = System.Timers.Timer;

namespace Amres.API.Services;

public class DailyCacheService : IHostedService
{
	private readonly object _lockObject = new object();
	private readonly IServiceScopeFactory _serviceScopeFactory;
	private readonly ILogger<DailyCacheService> _logger;
	private Timer? _dailyCensusTimer;

	public DailyCacheService(IServiceScopeFactory serviceScopeFactory, ILogger<DailyCacheService> logger)
	{
		_serviceScopeFactory = serviceScopeFactory;
		_logger = logger;
	}

	public Task StartAsync(CancellationToken cancellationToken)
	{
		_logger.LogInformation("{Service} is running.", nameof(DailyCacheService));
		
		_dailyCensusTimer = new Timer(TimeSpan.FromDays(1).TotalMilliseconds);
		_dailyCensusTimer.Elapsed += GatherStateCountyFips;

		//-- Gather on first run.
		GatherStateCountyFips(null, null);
		
		_dailyCensusTimer.Start();

		return Task.CompletedTask;
	}

	public Task StopAsync(CancellationToken cancellationToken)
	{
		return Task.CompletedTask;
	}

	private async void GatherStateCountyFips(object? obj, ElapsedEventArgs? args)
	{
		_logger.LogInformation("{Service} is gathering state and counties from Census API.", nameof(DailyCacheService));
		
		try
		{
			using (var scope = _serviceScopeFactory.CreateScope())
			{
				var censusProcessor = scope.ServiceProvider.GetService<CensusProcessor>();
				censusProcessor!.FipsCache = new();

				var response = await censusProcessor.GetStateAndCountyFips();

				if (response == null)
					return;

				foreach(var item in response.StateCountyFips)
				{
					censusProcessor.FipsCache!.Add(item);
				}
			}
		}
		catch (Exception exception)
		{
			_logger.LogError("{Service} had error occurr gathering state and county fips." + $"\\r{exception.Message}", nameof(DailyCacheService));
			throw exception ?? new Exception("Error occurred gathering state and county fips.");
		}
	}
}
```

You will just need to do one more thing to actually have this running: Add it to the `Program.cs`

```csharp
builder.Services.AddHostedService<DailyCacheService>();
```


# Stateful Service
[How to add a Stateful Service](https://github.com/dannydel/BlazorUtils)