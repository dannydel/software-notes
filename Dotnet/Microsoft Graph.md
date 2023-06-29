

## API setup
`Controller`

```csharp
[ApiController]
[Authorize]
[Route("api/[controller]")]
public class MicrosoftGraphController : ControllerBase
{
	private MicrosoftGraphProcessor _processor;

	public MicrosoftGraphController(MicrosoftGraphProcessor processor)
	{
		_processor = processor;
	}

	[HttpGet("groups")]
	public async Task<IActionResult> GetGroups() => Ok(await _processor.GetGroups());

	[HttpGet("groups/{id}")]
	public async Task<IActionResult> GetGroup(string id) => Ok(await _processor.GetGroup(id));

	[HttpGet("groups/{id}/members")]
	public async Task<IActionResult> GetMembers(string id) => Ok(await _processor.GetMembers(id));

	[HttpPost("groups/{id}/member")]
	public async Task<IActionResult> AddMember(string id, string memberId) => Ok(await _processor.AddMember(id, memberId));

	[HttpDelete("groups/{groupId}/members/{memberId}")]
	public async Task<IActionResult> RemoveMember(string groupId, string memberId) => Ok(await _processor.RemoveMember(groupId, memberId));

	[HttpGet("users")]
	public async Task<IActionResult> GetUsers() => Ok(await _processor.GetUsers());

	/// <summary>
	/// Gets Graph User by either Id or Email (UserPrincipalName)
	/// </summary>
	/// <param name="identifier"></param>
	/// <returns></returns>
	[HttpGet("users/{identifier}")]
	public async Task<IActionResult> GetMSGraphUser(string identifier = "") => Ok(await _processor.GetMSGraphUser(identifier));

	[HttpGet("b2cusers/{identifier}")]
	[ProducesDefaultResponseType(typeof(string))]
	public async Task<ActionResult<User>> GetB2CUser(string identifier = "") => Ok(await _processor.GetB2CUser(identifier));
}
```

`Processor`

```csharp
/// <summary>
/// Provides access to microsoft graph services and API.
/// </summary>
public class MicrosoftGraphProcessor : BaseProcessor
{
	private readonly IConfiguration _configuration;
	private GraphServiceClient? _internalClient;
	private GraphServiceClient? _externalClient;

	/// <summary>
	/// Constructor
	/// </summary>
	/// <param name="configuration"></param>
	/// <param name="logger"></param>
	public MicrosoftGraphProcessor(IConfiguration configuration, ILogger<MicrosoftGraphProcessor> logger) : base(logger)
	{
		_configuration = configuration;
		CreateB2CClient();
	}

	/// <summary>
	/// Authenticates access to Microsoft Graph.
	/// </summary>
	/// <returns></returns>
	public void Authenticate()
	{
		var scopes = new[] { "<https://graph.microsoft.com/.default>" };
		var tenantId = _configuration["MicrosoftGraphConfig:TenantId"];
		var clientId = _configuration["MicrosoftGraphConfig:ApplicationId"];
		var clientSecret = _configuration["MicrosoftGraphConfig:ApplicationSecret"];

		var options = new TokenCredentialOptions
		{
			AuthorityHost = AzureAuthorityHosts.AzurePublicCloud
		};

		var clientSecretCredential = new ClientSecretCredential(
			tenantId, clientId, clientSecret, options);

		_internalClient = new GraphServiceClient(clientSecretCredential, scopes);
	}

	public void CreateB2CClient()
	{
		var scopes = new[] { "<https://graph.microsoft.com/.default>" };
		var tenantId = _configuration["MicrosoftGraphB2CConfig:TenantId"];
		var clientId = _configuration["MicrosoftGraphB2CConfig:ApplicationId"];
		var clientSecret = _configuration["MicrosoftGraphB2CConfig:ApplicationSecret"];

		var options = new TokenCredentialOptions
		{
			AuthorityHost = AzureAuthorityHosts.AzurePublicCloud
		};

		var clientSecretCredential = new ClientSecretCredential(
			tenantId, clientId, clientSecret, options);

		_externalClient = new GraphServiceClient(clientSecretCredential, scopes);

		_externalClient.BaseUrl = "<https://graph.microsoft.com/beta/>";
	}

	/// <summary>
	/// Gets all Groups.
	/// </summary>
	/// <returns></returns>
	public async Task<List<Group>> GetGroups()
	{
		Authenticate();

		var groups  = await _internalClient!.Groups.Request().GetAsync();

		return groups.ToList();
	}

	//-- test id 03a98aa0-5935-4e9e-9b29-82dcf6cb1264
	public async Task<Group> GetGroup(string id)
	{
		Authenticate();

		//var filter = $"id eq '{id}'";
		var group = await _internalClient!.Groups[id].Request().GetAsync();

		return group;
	}

	/// <summary>
	/// Gets all Members in a Group.
	/// </summary>
	/// <param name="groupId"></param>
	/// <returns></returns>
	public async Task<List<DirectoryObject>> GetMembers(string groupId)
	{
		Authenticate();

		var members = await _internalClient!.Groups[groupId].Members.Request().GetAsync();

		return members.ToList();
	}

	/// <summary>
	/// Adds Member to Group.
	/// </summary>
	/// <param name="groupId"></param>
	/// <param name="memberId"></param>
	/// <param name="email"></param>
	/// <returns></returns>
	public async Task<bool> AddMember(string groupId, string memberId = "", string email = "")
	{
		Authenticate();
		DirectoryObject directoryObject = new()
		{
			Id = memberId
		};

		if (memberId == string.Empty && email != string.Empty)
		{
			var user = await GetUserByEmail(email);

			directoryObject.Id = user?.Id ?? "";
		}

		if (directoryObject.Id == null || directoryObject.Id == string.Empty)
			return false;

		await _internalClient!.Groups[groupId].Members.References.Request().AddAsync(directoryObject);

		return true;
	}

	/// <summary>
	/// Removes Member from a Group.
	/// </summary>
	/// <param name="groupId"></param>
	/// <param name="memberId"></param>
	/// <param name="email"></param>
	/// <returns></returns>
	public async Task<bool> RemoveMember(string groupId, string memberId = "", string email = "")
	{
		Authenticate();

		DirectoryObject directoryObject = new()
		{
			Id = memberId
		};

		if (memberId == string.Empty && email != string.Empty)
		{
			var user = await GetUserByEmail(email);

			directoryObject.Id = user?.Id ?? "";
		}

		if (directoryObject.Id == null || directoryObject.Id == string.Empty)
			return false;

		await _internalClient!.Groups[groupId].Members[memberId].Reference.Request().DeleteAsync();

		return true;
	}

	/// <summary>
	/// Gets list of Users from graph.
	/// </summary>
	/// <returns>IGraphServiceUsersCollectionPage</returns>
	public async Task<IGraphServiceUsersCollectionPage> GetUsers()
	{
		Authenticate();

		return await _internalClient!.Users.Request().GetAsync();
	}

	/// <summary>
	/// Gets User from Graph.
	/// </summary>
	/// <param name="identifier"></param>
	/// <returns>Graph User</returns>
	public async Task<User?> GetMSGraphUser(string identifier)
	{
		Authenticate();

		if (identifier.Contains('@'))
		{
			return await GetUserByEmail(identifier);
		}

		return await GetUserById(identifier);
	}

	public async Task<User?> GetB2CUser(string identifier)
	{
		if (identifier.Contains('@'))
		{
			return await GetB2CUserByEmail(identifier);
		}

		return await GetB2CUserById(identifier);
	}

	public async Task<bool> RegisterUser(User companyUser)
	{
		User user = new()
		{
			DisplayName = $"{companyUser.FirstName} {companyUser.LastName}",
			GivenName = companyUser.FirstName,
			Surname = companyUser.LastName,
			Identities = new List<ObjectIdentity>()
			{
				new()
				{
					SignInType = "emailAddress",
					Issuer = "company.onmicrosoft.com",
					IssuerAssignedId = amresUser.Email
				}
			},
			PasswordProfile = new()
			{
				Password = companyUser.Password,
				ForceChangePasswordNextSignIn = false
			},
			PasswordPolicies = "DisablePasswordExpiration"
		};

		try
		{
			_ = await _externalClient!.Users
				.Request()
				.AddAsync(user);
		}
		catch
		{
			return false;
		}

		return true;
	}

	public async Task<User> RegisterUser(string firstName, string lastName, string email, string password)
	{
		var existingUser = await _externalClient!.Users.Request().Filter($"mail eq '{email}'").GetAsync();

		if(existingUser.Any())
		{
			return existingUser.First();
		}

		User user = new()
		{
			DisplayName = $"{firstName} {lastName}",
			GivenName = firstName,
			Surname = lastName,
			Identities = new List<ObjectIdentity>()
				{
					new()
					{
						SignInType = "emailAddress",
						Issuer = "amreslending.onmicrosoft.com",
						IssuerAssignedId = email
					}
				},
			PasswordProfile = new()
			{
				Password = password,
				ForceChangePasswordNextSignIn = true
			},
			PasswordPolicies = "DisablePasswordExpiration"
		};

		var graphUser = await _externalClient.Users
			.Request()
			.AddAsync(user);

		return graphUser;
	}

	private string GeneratePassword(int length)
	{
		string lowercaseChars = "abcdefghijkmnopqrstuvwxyz";
		string uppercaseChars = "ABCDEFGHJKLMNOPQRSTUVWXYZ";
		string digits = "0123456789";
		string symbols = "!@#$%&*?";

		Random random = new();
		List<char> chars = new();

		void RandomInsert(string source)
		{
			char c = source[random.Next(0, source.Length)];
			chars.Insert(random.Next(0, chars.Count), c);
		}

		// Make sure at least one from each category is added
		RandomInsert(lowercaseChars);
		RandomInsert(uppercaseChars);
		RandomInsert(digits);
		RandomInsert(symbols);

		// Concatenate the string to make a single selection string
		string groupedSelection = lowercaseChars + uppercaseChars;

		for (int i = chars.Count; i < length - 1; i++)
		{
			RandomInsert(groupedSelection);
		}

		return lowercaseChars[random.Next(0, lowercaseChars.Length)].ToString() + "#" + new string(chars.ToArray());
	}

	private async Task<User> GetB2CUserById(string id) => await _externalClient!.Users[id].Request().GetAsync();

	private async Task<User?> GetB2CUserByEmail(string email)
	{
		var users = await _externalClient!.Users.Request().Filter($" Mail eq '{email}'").GetAsync();

		return users.FirstOrDefault();
	}

	private async Task<User> GetUserById(string id) => await _internalClient!.Users[id].Request().GetAsync();

	private async Task<User?> GetUserByEmail(string email)
	{
		var users = await _internalClient!.Users.Request().Filter($" UserPrincipalName eq '{email}'").GetAsync();

		return users.FirstOrDefault();
	}
}
```
