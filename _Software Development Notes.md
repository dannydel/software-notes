A full overview and all notation on different topics of software development.

🏛️[[Architecture Overview]]
📦[[Micro Services]]

## Dotnet

💉[[Dependency Injection]]


# Resources
⚒️[[Resources]]



## RestServices Project 
Example of `RestService` Project:

```csharp
<Project Sdk="Microsoft.NET.Sdk">

	<PropertyGroup>
		<TargetFramework>net7.0</TargetFramework>
		<ImplicitUsings>enable</ImplicitUsings>
		<VersionPrefix>1.0.12</VersionPrefix>

		<APIGeneratorProjectPath>..\\..\\API\\Amres.API</APIGeneratorProjectPath>
		<APIGeneratorClassName>AmresRestService</APIGeneratorClassName>
	</PropertyGroup>

	<ItemGroup>
		<PackageReference Include="Amres.APICallGeneration.Build" Version="2.5.2" />
		<PackageReference Include="Amres.APICallGeneration.Dependencies" Version="1.1.0" />
		<PackageReference Include="Microsoft.Graph" Version="4.54.0" />
	</ItemGroup>

	<ItemGroup>
		<Reference Include="Amres.Base.Models">
			<HintPath>$(PkgAmres_WebPortal_Models)\\lib\\net7.0\\Amres.Base.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Amres.Base.Models\\bin\\Debug\\net7.0\\Amres.Base.Models.dll')" Update="Amres.Base.Models">
			<HintPath>..\\..\\Data Models\\Amres.Base.Models\\bin\\Debug\\net7.0\\Amres.Base.Models.dll</HintPath>
		</Reference>
	</ItemGroup>

	<ItemGroup>
		<PackageReference Include="Amres.WebPortal.Models" Version="*" ExcludeAssets="Compile" GeneratePathProperty="true" />

		<Reference Include="Amres.WebPortal.Models">
			<HintPath>$(PkgAmres_WebPortal_Models)\\lib\\net7.0\\Amres.WebPortal.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Amres.WebPortal.Models\\bin\\Debug\\net7.0\\Amres.WebPortal.Models.dll')" Update="Amres.WebPortal.Models">
			<HintPath>..\\..\\Data Models\\Amres.WebPortal.Models\\bin\\Debug\\net7.0\\Amres.WebPortal.Models.dll</HintPath>
		</Reference>
	</ItemGroup>

	<ItemGroup>
		<Reference Include="Amres.Loannex.Models">
			<HintPath>$(PkgAmres_WebPortal_Models)\\lib\\net7.0\\Amres.Loannex.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Amres.Loannex.Models\\bin\\Debug\\net7.0\\Amres.Loannex.Models.dll')" Update="Amres.Loannex.Models">
			<HintPath>..\\..\\Data Models\\Amres.Loannex.Models\\bin\\Debug\\net7.0\\Amres.Loannex.Models.dll</HintPath>
		</Reference>
	</ItemGroup>

	<ItemGroup>
		<Reference Include="Amres.Mismo.Models">
			<HintPath>$(PkgAmres_WebPortal_Models)\\lib\\net7.0\\Amres.Mismo.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Amres.Mismo.Models\\bin\\Debug\\net7.0\\Amres.Mismo.Models.dll')" Update="Amres.Mismo.Models">
			<HintPath>..\\..\\Data Models\\Amres.Mismo.Models\\bin\\Debug\\net7.0\\Amres.Mismo.Models.dll</HintPath>
		</Reference>
	</ItemGroup>

	<ItemGroup>
		<Reference Include="Amres.Byte.Models">
			<HintPath>$(PkgAmres_WebPortal_Models)\\lib\\net7.0\\Amres.Byte.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Amres.Byte.Models\\bin\\Debug\\net7.0\\Amres.Byte.Models.dll')" Update="Amres.Byte.Models">
			<HintPath>..\\..\\Data Models\\Amres.Byte.Models\\bin\\Debug\\net7.0\\Amres.Byte.Models.dll</HintPath>
		</Reference>
	</ItemGroup>

	<ItemGroup>
		<Reference Include="Amres.Census.Models">
			<HintPath>$(PkgAmres_WebPortal_Models)\\lib\\net7.0\\Amres.Census.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Amres.Census.Models\\bin\\Debug\\net7.0\\Amres.Census.Models.dll')" Update="Amres.Census.Models">
			<HintPath>..\\..\\Data Models\\Amres.Census.Models\\bin\\Debug\\net7.0\\Amres.Census.Models.dll</HintPath>
		</Reference>
	</ItemGroup>

	<ItemGroup>
		<Reference Include="Amres.ShortTermPricing.Models">
			<HintPath>$(PkgAmres_WebPortal_Models)\\lib\\net7.0\\Amres.ShortTermPricing.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Amres.ShortTermPricing.Models\\bin\\Debug\\net7.0\\Amres.ShortTermPricing.Models.dll')" Update="Amres.ShortTermPricing.Models">
			<HintPath>..\\..\\Data Models\\Amres.ShortTermPricing.Models\\bin\\Debug\\net7.0\\Amres.ShortTermPricing.Models.dll</HintPath>
		</Reference>
	</ItemGroup>

	<ItemGroup>
		<Reference Include="Amres.Polly.Models">
			<HintPath>$(PkgAmres_WebPortal_Models)\\lib\\net7.0\\Amres.Polly.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Amres.Polly.Models\\bin\\Debug\\net7.0\\Amres.Polly.Models.dll')" Update="Amres.Polly.Models">
			<HintPath>..\\..\\Data Models\\Amres.Polly.Models\\bin\\Debug\\net7.0\\Amres.Polly.Models.dll</HintPath>
		</Reference>
	</ItemGroup>
</Project>
```

Also needs to be installed on the API project that was created. These would have the controllers needed to generate off of.

You will need to also set up Nuget packages.