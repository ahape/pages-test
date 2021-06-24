# Agent installer

There are two versions of the agent installer:
- [InnoSetup](https://jrsoftware.org/isinfo.php) is the older installer application we used and will be needed if any further InnoSetup-based installer packages need to be produced.
- MSI uses the [WiX Toolset](https://wixtoolset.org/) to produce a standard Microsoft Installer package.  To build in Visual Studio both the WiX Toolset and the Visual Studio Extensions are needed.

In either case, the Brightmetrics code signing certificate is needed.  If you need to build packages for deployment, contact your manager to get the current code signing certificate.

If you only need to test running the Brightmetrics Agent, you can unload this project from the solution, and can skip installing the WiX Toolset.