[![Build status](https://dev.azure.com/brightmetricsazure/BrightmetricsWeb/_apis/build/status/BrightmetricsWeb-CI)](https://dev.azure.com/brightmetricsazure/BrightmetricsWeb/_build/latest?definitionId=2)

# Brightmetrics Development

This wiki contains documentation around the development and debugging of the portions of Brightmetrics contained in this repository.

## Main components

There are three separate solutions that share many of the same core projects and together represent the main components of Brightmetrics-hosted infrastructure.

See [[main build instructions]] for prerequisites and how to build them together.

### The main web solution (BrightMetricsWeb)
- The [[web application]] itself, including UI frontend and API backend
- The [[Cloud Agent]] (CloudAgentWebJob) - dispatches summary update tasks run via the cloud agent to the appropriate queue, and for PureCloud also runs requests off the cloud agent queue.
- [[PureCloud RealTime]] - Runs the logical real time agents for each PureCloud Realtime customer instance.
- [[Powershell Commands]] - Powershell Cmdlets to perform maintenance, modifications, and other low-level access to application data structures
- [[Unit Tests]] for some of the above

### The background worker (BrightMetricsWorker)
- [[Data importer]] - responsible for loading periodic summary updates in to the SQL Azure databases
- [[Email report poller]] - responsible for listening for requests to generate reports (also for export, not strictly email)
- [[Schedule checker]] - Checks for scheduled emails, builds email report tasks from them, and dispatches them to the EmailReportPoller
- Other background non-interactive services (e.g. posting subscription events to Segment)

### The maintenance helper (BrightMetricsHelper)

The distinction from the BrightMetricsWorker solution is that these are mainly things that run once a day and require privileged access.
Activating/Expiring trials, Renewing/Expiring subscriptions, invoicing customers and partners.  Also handles password resets.

For testing, see [[Helper Runner]]

## Other components

### The Brightmetrics agent (BrightmetricsAgent)
- The on-premise [[Agent Service]] itself, which handles connectivity between the customer's server and Brightmetrics, running tasks on demand, and running the persistent real-time monitoring.
- The [[Agent Installer]] that installs and updates the agent service.
- The [[Agent Updater]] that checks for and launches updates.
- The [[Agent Diagnostics]] that the customer can run to troubleshoot connectivity between the agent and Brightmetrics.
- Plugins for connecting to various data sources:
  - [[SqlModule]] for connecting to arbitrary MySQL, SQLite, or MSSQL databases.
  - [[C2GProcessor]] for processing data from ECC's CCIR (aka c2g) database in to a more consumable representation in SQLite.
  - [[MiCCPlugin]] for processing data from MiCC's database.
- The [[Realtime Hubs]] that agents and browsers connect to to relay data real time data.

### Amazon Connect agent
- SQS event driven agent that is ran in a AWS Lambda.
- Each detail/summary request is set to the SQS, and then handled in batches by the Lambda function
- Lives in it's own repository [brightmetrics/amazonconnect](https://github.com/brightmetrics/amazonconnect)

### DataSpec files for all data sources

Describing all the available facts and dimensions for each data source, and for SQL data sources the queries to generate summary and detail data

See [[testing dataspecs]]

