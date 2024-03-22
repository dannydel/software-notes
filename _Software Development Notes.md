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

		<APIGeneratorProjectPath>..\\..\\API\\Test.API</APIGeneratorProjectPath>
		<APIGeneratorClassName>TestRestService</APIGeneratorClassName>
	</PropertyGroup>

	<ItemGroup>
		<PackageReference Include="Test.APICallGeneration.Build" Version="2.5.2" />
		<PackageReference Include="Test.APICallGeneration.Dependencies" Version="1.1.0" />
		<PackageReference Include="Microsoft.Graph" Version="4.54.0" />
	</ItemGroup>

	<ItemGroup>
		<Reference Include="Test.Base.Models">
			<HintPath>$(PkgTest_WebPortal_Models)\\lib\\net7.0\\Test.Base.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Test.Base.Models\\bin\\Debug\\net7.0\\Test.Base.Models.dll')" Update="AmrTestes.Base.Models">
			<HintPath>..\\..\\Data Models\\Test.Base.Models\\bin\\Debug\\net7.0\\Test.Base.Models.dll</HintPath>
		</Reference>
	</ItemGroup>

	<ItemGroup>
		<PackageReference Include="Test.WebPortal.Models" Version="*" ExcludeAssets="Compile" GeneratePathProperty="true" />

		<Reference Include="Test.WebPortal.Models">
			<HintPath>$(PkgTest_WebPortal_Models)\\lib\\net7.0\\Test.WebPortal.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Test.WebPortal.Models\\bin\\Debug\\net7.0\\Test.WebPortal.Models.dll')" Update="Test.WebPortal.Models">
			<HintPath>..\\..\\Data Models\\Test.WebPortal.Models\\bin\\Debug\\net7.0\\Test.WebPortal.Models.dll</HintPath>
		</Reference>
	</ItemGroup>

	<ItemGroup>
		<Reference Include="Test.Loannex.Models">
			<HintPath>$(PkgAmres_WebPortal_Models)\\lib\\net7.0\\Test.Loannex.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Test.Loannex.Models\\bin\\Debug\\net7.0\\Test.Loannex.Models.dll')" Update="Test.Loannex.Models">
			<HintPath>..\\..\\Data Models\\Test.Loannex.Models\\bin\\Debug\\net7.0\\AmTestres.Loannex.Models.dll</HintPath>
		</Reference>
	</ItemGroup>

	<ItemGroup>
		<Reference Include="Amres.Mismo.Models">
			<HintPath>$(PkgTest_WebPortal_Models)\\lib\\net7.0\\Test.Mismo.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Test.Mismo.Models\\bin\\Debug\\net7.0\\Test.Mismo.Models.dll')" Update="Test.Mismo.Models">
			<HintPath>..\\..\\Data Models\\Test.Mismo.Models\\bin\\Debug\\net7.0\\Test.Mismo.Models.dll</HintPath>
		</Reference>
	</ItemGroup>

	<ItemGroup>
		<Reference Include="Test.Byte.Models">
			<HintPath>$(PkgTest_WebPortal_Models)\\lib\\net7.0\\Test.Byte.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Test.Byte.Models\\bin\\Debug\\net7.0\\Test.Byte.Models.dll')" Update="Test.Byte.Models">
			<HintPath>..\\..\\Data Models\\Test.Byte.Models\\bin\\Debug\\net7.0\\Test.Byte.Models.dll</HintPath>
		</Reference>
	</ItemGroup>

	<ItemGroup>
		<Reference Include="Test.Census.Models">
			<HintPath>$(PkgTest_WebPortal_Models)\\lib\\net7.0\\Test.Census.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Test.Census.Models\\bin\\Debug\\net7.0\\Test.Census.Models.dll')" Update="Test.Census.Models">
			<HintPath>..\\..\\Data Models\\Test.Census.Models\\bin\\Debug\\net7.0\\Test.Census.Models.dll</HintPath>
		</Reference>
	</ItemGroup>

	<ItemGroup>
		<Reference Include="Test.ShortTermPricing.Models">
			<HintPath>$(PkgTest_WebPortal_Models)\\lib\\net7.0\\Test.ShortTermPricing.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Test.ShortTermPricing.Models\\bin\\Debug\\net7.0\\Test.ShortTermPricing.Models.dll')" Update="Test.ShortTermPricing.Models">
			<HintPath>..\\..\\Data Models\\Test.ShortTermPricing.Models\\bin\\Debug\\net7.0\\Test.ShortTermPricing.Models.dll</HintPath>
		</Reference>
	</ItemGroup>

	<ItemGroup>
		<Reference Include="Test.Polly.Models">
			<HintPath>$(PkgATest_WebPortal_Models)\\lib\\net7.0\\Test.Polly.Models.dll</HintPath>
		</Reference>
		<Reference Condition="'$(Configuration)'=='DEBUG' AND Exists('..\\..\\Data Models\\Test.Polly.Models\\bin\\Debug\\net7.0\\Test.Polly.Models.dll')" Update="Test.Polly.Models">
			<HintPath>..\\..\\Data Models\\Amres.Polly.Models\\bin\\Debug\\net7.0\\Test.Polly.Models.dll</HintPath>
		</Reference>
	</ItemGroup>
</Project>
```

Also needs to be installed on the API project that was created. These would have the controllers needed to generate off of.

You will need to also set up Nuget packages.