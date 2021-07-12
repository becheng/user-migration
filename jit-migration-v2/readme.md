# Azure AD B2C: Just in time user migration v2 (forked)
This is repo is a fork of the [azure-ad-b2c/user-migration](https://github.com/azure-ad-b2c/user-migration) repo to provide proof-of-concepts (PoCs) of just-in-time user migration using i) a sign-up and sign-in user flow for a single application and ii) a sign-up user flow with multiple app selections (similar to the 'My Apps Portal' experience).

- Both PoCs were derived from the *jit-migration-v2* code of the *user-migration* repo.  Refer to its [readme](https://github.com/azure-ad-b2c/user-migration/tree/master/jit-migration-v2) for the full explanation of the code and the B2C custom policy, including the flow diagrams that illustrate the JIT user migration logic and **instructions and setting up your environment to run the sample**.
- Single application JIT user migration PoC conducted using the policies and source-code within *jit-migration-v2* folder.  To run, compile source-code, deploy to an Azure app service, update B2C policies with your B2C tenant, update the REST API domains and upload to the B2C tenant.  See section below on notes to get source-code deployed.
- For the Sign-up to multiple apps JIT user migration flow PoC refer ot the contents of the *poc-wireframes-based* within the *policy* folder.  To run, replace policies with your B2C tenant, update REST API domains and upload to your B2C tenant. Note: the same source code deployed for the single app PoC will work for this PoC.
**Mappings**

   | APP    | userController | Azure Storage Table |
   |--------|----------------|---------------------| 
   | App1   | userMigrationController  | UserMigrationIdentities
   | App2   | userMigrationController2 | UserMigrationIdentities2
   | App3   | userMigrationController3 | UserMigrationIdentities3
  
## Notes to deploying 'source-code' to an Azure App Service

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
3. Added the following publish path to `"appService.deploySubpath"` in .vscode > settings.json : 
    ```
    jit-migration-v2\\source-code\\AADB2C.JITUserMigration\\bin\\Release\\netcoreapp2.1\\win-x86\\publish
    ```
4. Deployed contents of the above "publish" folder to a "Windows" AppService with a Runtime stack of "DotNetCore 2.1" where my domain was *b2cjitreg-appwin.azurewebsites.net*

5. Executed the a GET request against the respected migration table on PostMan to populate the test users using the *TestController* 
   ```
   http://b2cjitreg-appwin.azurewebsites.net/api/Test/PopulateMigrationTable
   ``` 


6. Logged onto the site's Kudu console and manually ran: 
   ```
   C:\home\site\wwwroot>.\AADB2C.JITUserMigration.exe
   ```

Note: To run on local host issue the following command: 
   ```
   C:\[repo-path]\user-migration\jit-migration-v2\source-code\AADB2C.JITUserMigration>dotnet run --project .\AADB2C.JITUserMigration.csproj
   ```
 
**IMPORTANT REMINDER** - Reissue the storage SAS key for demos!  If you are getting a 500 server error, it's most likely your Azure storage SAS key to access the *migrationtbl* has expired.






