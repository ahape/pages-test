Choose a location for keeping the powershell commands.  For example, I have a folder `C:\bin` in which I keep miscellaneous utilities and have as part of my `PATH` environment variable.  I keep the powershell utilities in `C:\bin\PowershellCommands`, and powershell scripts in `C:\bin` so that they are in the PATH.  I will use these examples below.

Either run the (re)build.cmd script as documented in [[main build instructions]], or in Visual Studio build the PowershellCommands project in the BrightMetricsWeb solution.

In a command prompt, go the PowerShellCommands folder and run
```
xcopy /e bin\Debug c:\bin\PowershellCommands
```
and
```
copy /y Console.ps1 c:\bin\bmutil.ps1
```

The copy of the Powershell script (the .ps1 file) will need to be edited to reference the path to the PowershellCommands location.

It will also be necessary to create a file named `devSettings.config` in the `%APPDATA%` folder (`$env:appdata` in powershell) with the following content:
```
<appSettings>
  <add key="CompanyDataCachePath" value="C:\work\companies.dat" />
  <add key="BillingTokenURL" value="https://login.microsoftonline.com/33225117-2f1e-4551-a9a4-469816d4539f/oauth2/v2.0/token" />
  <add key="BillingBaseURL" value="https://brightmetrics-billing.azurewebsites.net" />
</appSettings>
```
where the `CompanyDataCachePath` can likewise be any path you want.  I happen to have a folder `c:\work` in which I keep random working files.

`BillingTokenURL` and `BillingBaseURL` are for billing system support.  If the billing system is being run locally in debug mode, BillingBaseURL can be altered to point to the local instance.

Finally, ensure that per [[main build instructions]] the ConfigData certificate has been installed.

Now whenever you want to run powershell commands just run `bmutil` (or `c:\bin\bmutil.ps1` if it is not in your PATH).

The first time, and occasionally, you will be prompted to authenticate to Azure Active Directory.  If you use the Brightmetrics Admin site, it is the same credentials.

When switching branches, it may be cumbersome to run through the copy/paste steps above to get the latest Powershell commands. Instead, you can just add [this cmdlet](https://github.com/brightmetrics/support/blob/main/Useful%20Powershell%20Scripts/update-local-cmdlets.ps1) to the folder you run your cmdlets from. Then, anytime you need to get the latest changes, just run `.\update-local-cmdlets.ps1`.

## Tips and warnings
- First and foremost: the powershell cmdlets give essentially direct access to the application data structures with little constraints.  **Be very cautious**, as it is completely possible to delete things or make unwanted changes.
- After an operation or sequence of operations, run the `Close-DataSession` cmdlet to close the session and ensure any changes are written back to the database and loaded objects are removed from memory.  If you are prompted to save changes when you don't think you made any, choose No!
- You can obviously look through the project to find which commands are available and how to use them, but you can also use `Get-Help commandname` to get usage help on any command, and `Get-Command -Module PowershellCommands` to get a list of all of them.
- If you are going to run a cmdlet that updates a data object in our database, you'll need to include in your powershell utilities (e.g. `C:\bin\PowershellCommands`) the file `hibernate.cfg.xml`. That file is not currently hosted anywhere, so you'll have to message a team member for it.