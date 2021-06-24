# MiCC Plugin

## Building

The MiCC plugin handles processing the summary and detail requests to gather data from the MiCC SQL database and workflow files. In order to build and package the MiCC plugin, you must do the following:

1. Build the MiCCPlugin project in the BrightmetricsAgent solution.
2. Open PowerShell and navigate to the root directory of the BrightmetricsAgent solution
3. Run `bmutil.ps1` \(or `Console.ps1`, see [PowerShell docs](https://github.com/brightmetrics/projects/wiki/Powershell-Commands) for any further clarification\) to load the assemblies and cmdlets
4. Run the plugin package cmdlet: `.\Package-MiCCPlugin.ps1`
   * This command will grab the debug binary of the plugin you just built previously, and upload them to azure blob storage as a single package.  It auto-increments the package number for every build.
5. Take note of the version number the logs show was packaged and uploaded.

## Deploying

You can either deploy the MiCC plugin to be used as the current version for all MiCC DCGs, or you can specify a specific version of the plugin to run for a specific MiCC DCG. To do so, do the following:

1. Open PowerShell
2. Run bmutil.ps1/Console.ps1 to load the assemblies and cmdlets
3. Get the DCG for the MiCC instance: `$dcg = Get-DataConnectionGroup -Id XXXXXXX`
4. Update the plugin version \(Choose one\)
   * Company specific plugin
     * `$dcg.Connections[0].InstancePlugin = "MiCCPlugin-X.X.XXX"` 
     * Updates the plugin for the loaded data connection group.  Used for testing for a specific company/DCG, or releasing a specific version to a particular company
   * Release plugin
     * **WARNING**: This updates for **all** companies on the main release cycle
     * **WARNING**: If there are any additional fields added to the data spec in this release, you must update the released data spec as well that matches this plugin version
     * `$dcg.get_Connections()[0].get_Connection().set_Plugin('MiCCPlugin-X.X.XXX')`
     * Updates the current released plugin version.  The release plugin version is the one used by DCGs that do not have a specific version specified for them
5. Save the changes by flushing the session: `Close-DataSession`

## Running

The MiCCPlugin looks for a local installation of the Mitel Contact Center client tools. Those are always present on the server, but will need to be installed locally to run and test locally.

The [Client Component Pack](https://andrewgaskill.s3.us-east-1.amazonaws.com/Mitel%20Client%20Component%20Pack.exe) packaged from our installation \(and so matching our version\) can be downloaded and installed.

### Notes on installing the Client Component Pack

* When prompted, the enterprise server IP address should be set to `13.57.236.125`.

## Debugging

When debugging, you will need the pre-reqs required from the Running section above. To debug a given agent task, you can set the MiCC Plugin as your startup project, and set the following as your arguments.

```text
<DSI ID> MiCC.DataRequest debug "Data Source=C:\work\mydata\micc.sqlite;BaseSchemaName=dbo" "C:\work\micc_detail.json"
```

* DSI ID: One of the data source instance IDs for the MiCC data source your data is against.
* Data Source: the path to the empty micc sqlite file that's in the same directory as the data files.  If you want to connect directly to a MiCC database, then you can use a normal MS SQL connection string to that database.
* micc\_detail.json: A file containing the full XML of the task to run.

For the task file, an easy way to get those contents is to run the report/query you want, then look in the agent logs for the corresponding log of the task it ran. Then copy it all directly to your file.

### Downloading customer data

You can run the cmdlet `get-miccdata` to download the entirety of a customer's data to be able to debug using it.

```text
get-miccdata -agentid <agent ID> -dsiid <DSI ID> -datadir 'c:\work\mydata'
```

Once complete, you can then deflate and copy the config snapshot files \(located in the agent ID subdirectory\) to your local `C:\BrightmetricsData\` directory.

### Build versions

See \[\[MiCC Plugin Builds\]\] for which builds of the plugin correspond to which branches/commits

