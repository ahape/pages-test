# Cloud Agent

## Prerequisites

* [Fiddler](https://www.telerik.com/download/fiddler) must be running when the CloudAgent project is started, as in debug builds the Fiddler proxy address is configured at startup.  Make sure HTTPS capture is enabled.
* Azure Storage Emulator must be running.  This should be installed as part of the Azure development tools with Visual Studio, but it must be launched \(it will run in the background but stop if the computer is restarted\).

## Configuration

### Environment variables

Before running the CloudAgentWebJob project locally, environment variables must be set to direct the WebJobs SDK to use the local emulation environment. Configure the following system environment variables:

* AzureWebJobsEnv: `Development`
* AzureWebJobsDashboard: `UseDevelopmentStorage=true`
* AzureWebJobsStorage: `UseDevelopmentStorage=true`

The computer does not need to be restarted after updating the environment variables, but Visual Studio must be relaunched to pick up the changes.

### Misc.

* In CloudAgentWebJob -&gt; `App.config`, you will also need to set the `AgentQueueName` key to `cloudagentbeta` \(although this is just temporary\):

  ```text
  <appSettings>
    <add key="AgentQueueName" value="cloudagentbeta" />
  </appSettings>
  ```

* Additionally, to run locally, you will need to set both BrightmetricsWebUI AND CloudAgentWebJob as startup projects. You can set multiple startup projects in VS by right clicking on the solution -&gt; "Set Startup Projects...".

