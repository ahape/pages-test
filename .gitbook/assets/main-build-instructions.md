# Building the web application and related components

The main components (BrightmetricsWeb, BrightmetricsWorker, and BrighmetricsHelper) can be built upon checkout by running `rebuild.cmd` in the root of the working tree.  The prerequisites are:
- Visual Studio 2019 (any version) with the workloads:
  - ASP.NET and web development
  - Azure development
  - .NET desktop development
- Java runtime (1.8+)
- NodeJS and NPM
- Ensure a folder `C:\src\azure` exists and is writeable, for holding web app build artifacts

For incremental builds, `build.cmd` can be used instead, which skips updating NPM and nuget packages and runs a "build" of each solution rather than a "rebuild."  `rebuild.cmd` is only needed on initial checkout or when switching between major branches with changes to NPM or NuGet packages.

Note that Visual Studio 2017 should work as well, but `rebuild15.cmd` needs to be run instead.  In addition, some projects may need to have their C# language version set to "latest" for VS2017 to enable the required language features, if they have not been built with VS2017 before.

Note that in order to run powershell cmdlets within the build script, you must have an [execution policy](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.1) that allows the running of scripts.

## Visual Studio extensions
The following Visual Studio extensions are recommended or required
- [SonarLint](https://www.sonarlint.org/visualstudio/) for code analysis (required)
- [CodeMaid](http://www.codemaid.net/) for helpful formatting and navigation features (recommended)

# Installing ConfigData certificate

In order to run most of the projects, they need to connect to our configuration data stored in KeyVault, which is authenticated by client certificate.

1. Obtain the current ConfigData certificate
1. Run `mmc.exe`.
1. Choose File -> Add/Remove Snap-in.
1. Choose "Certificates` and click Add.
1. Choose "Computer account" and click Next.
1. Choose "Local computer" and click Finish.
1. Click OK to close the "Add or Remove Snap-ins" dialog.
1. Expand Certificates -> Personal -> Certificates.
1. Right-click Certificates and choose "All tasks" -> "Import".  Click Next.
1. Locate and open the certificate file.  It may be necessary to change the open dialog's file type to "Personal Information Exchange (*.pfx)".
1. Click Next in the File to Import step.
1. In the Private Key Protection step, enter the password for the certificate and ensure the only checked box is "Include all extended properties".  Click Next.
1. In the Certificate Store step, ensure "Place all certificates in the following store" is selected and set to "Personal".  Click Next.
1. Click Finish.  The certificate should now be imported.
1. Right-click the imported certificate and choose "All Tasks" -> "Manage Private Keys".
1. Click Add and add your user account.  Check the "Read" allow permission.
1. If you will be running the Brightmetrics web application in a full IIS installation, you will also need to click Add again and add "IIS AppPool\BrightmetricsWebApp" (or whatever is the name of your application pool).
1. The MMC console can now be closed.  It will prompt the "Save console settings", but you can click "No", it doesn't have anything to do with saving changes to the certificate.