# How I got my environment working...

1. Changed .vscode/settings.json with the following
   a. Made the following changes to the .csproj file:
   
    Original:
    ```
    <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
    </PropertyGroup>
    ``` 
   
   Changed:
   ```
   <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win-x86</RuntimeIdentifier>
        <PublishWithAspNetCoreTargetManifest>false</PublishWithAspNetCoreTargetManifest>
   </PropertyGroup>
   ```
   
2. Ran the following in terminal (to publish):
   ```
   C:\[repo-path]\user-migration\jit-migration-v2\source-code\AADB2C.JITUserMigration>dotnet publish -c Release --self-contained false
   ```
3. Added the following publish path to `"appService.deploySubpath"` in .vscode > settings.json : `"jit-migration-v2\\source-code\\AADB2C.JITUserMigration\\bin\\Release\\netcoreapp2.1\\win-x86\\publish"`

4. Deployed contents of the above "publish" folder to a "Windows" AppService with a Runtime stack of "DotNetCore 2.1" where my domain is "b2cjitreg-appwin.azurewebsites.net"

5. Executed GET request on the `http://b2cjitreg-appwin.azurewebsites.net/api/Test/PopulateMigrationTable` using PostMan to populate the test users the `TestController`

6. Logged onto the site's Kudu console and manually ran: `C:\home\site\wwwroot>.\AADB2C.JITUserMigration.exe`

## IMPORTANT REMINDER - REISSUE THE STORAGE SAS KEY FOR DEMOS - IF YOU GET A 500 SERVER ERROR, ITS MOST LIKELY EXPIRED.

## Notes
- to run on local host issue the following command: `C:\[repo-path]\user-migration\jit-migration-v2\source-code\AADB2C.JITUserMigration>dotnet run --project .\AADB2C.JITUserMigration.csproj`








