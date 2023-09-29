``` JSON
"profiles": {  
  "Project.Web": {  
    "type": "coreclr",  
    "commandName": "Project",  
    "launchBrowser": false,  
    "inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}",  
    "applicationUrl": "http://localhost:5000",  
    "environmentVariables": {  
      "DOTNET_ENVIRONMENT": "Development"  
    }  
  }
```

While having many issues with debugging and building with MAUI/Blazor here are the current launchSettings.json I have for projects
- inspectURI is not important here
- What is important :
	- type - coreclr
	- applicationUrl
