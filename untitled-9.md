# SMDR Processor Service

The processor service is a small utility that is installed within network access to the MiVB server. It listens to the SMDR and ACD Event streams and logs the data to a LocalDB for processing by the MIVB Plugin.

## System Requirements

* .NET Framework 4
* Microsoft SQL Server Local DB 2014 or newer
  * For 2017, make sure this KB is installed to fix invalid path issues for DB creation: [https://support.microsoft.com/en-us/topic/kb4101464-cumulative-update-6-for-sql-server-2017-3050a2f3-72b3-83c7-0a5d-bfd5e4bcedb6](https://support.microsoft.com/en-us/topic/kb4101464-cumulative-update-6-for-sql-server-2017-3050a2f3-72b3-83c7-0a5d-bfd5e4bcedb6)
* Network access to MiVB Server
  * TCP Access to the following ports:
    * SMDR Events: 1752
    * ACD Events: 15373
* Same Timezone as MiVB Server

## Installation

* Install all pre-requisites
  * .NET Framework 4 - [http://www.microsoft.com/en-us/download/details.aspx?id=17718](http://www.microsoft.com/en-us/download/details.aspx?id=17718)
  * Microsoft SQL Server Local DB - [https://brightmetricsstorage.blob.core.windows.net/publicfiles/SqlLocalDB\_2014\_x64.msi](https://brightmetricsstorage.blob.core.windows.net/publicfiles/SqlLocalDB_2014_x64.msi)
* Download the SMDR Processor Setup
  * [https://brightmetricsstorage.blob.core.windows.net/publicfiles/BrightmetricsAgent/BrightmetricsSMDRProcessorSetup.msi](https://brightmetricsstorage.blob.core.windows.net/publicfiles/BrightmetricsAgent/BrightmetricsSMDRProcessorSetup.msi)
* Run the SMDR Processor setup program
  * When prompted for the Server Address, enter the IP Address to your MiVB Server

## Development Notes

* The `CurrentSchemaVersion` constant for the SMDR and ACD event classes should only be incremented when there is a change in the schema of the incoming SMDR/ACD event from the MiVB server.  Actions needed due to changes to the schema of the classes themselves should be handled by the EF Migration functionality.
* When changing the schema of any of the DB Context classes, you should create a migration.  This can be accomplished by opening the **Package Manager Console** for the **MiVBCommon Project** and running `Add-Migration <name of migration>`.  This will create a migration based on the changes you made in relation to your current LocalDB instance.  Try naming the migration related to the changes that you made.
  * Make sure you have ran the SMDR Processor at least once without your changes to populate your localDB with the current schema.

## Debugging

To get the current SMDR buffer for a MiVB server \(up to 20,000 records\), run the following command via the Maintenance Commands in the Web UI: `LOGSYS READ SMDR INTO SMDREVENTS ALL`

You can then SSH or FTP into the MiVB server and navigate to `/db/cc/` and copy the file `SMDREVENTS.1`

### Testing locally

The one hard requirement for any kind of local debugging is that you have at least SQL Server Express 2014 installed. This is to insure that you have Sql LocalDB installed and available for use.

To test, you can manually debug it with a test data file via adding to the debug command line arguments: `debug debugdata <path to data file>`

This data file is a simple text file with each line being one event. You can mix ACD and SMDR events in the same data file. For a sample SMDR data file, you can look in the SMDRProcessorTest proj in the TestData folder.

To test with actual data, you just need to specify the `debug` portion in the command line arguments. As well, you will need to have the following set up locally:

1\) \#\#\# VPN Access to SMDR Server This requires downloading/running a small utility to set up the VPN connection, and a certificate to be created for you. Talk to Andrew to get this set up.

2\) \#\#\# Registry entry for server address ![image](https://user-images.githubusercontent.com/2438810/108547799-cf680780-729f-11eb-83bb-b94c1a000769.png)

3\) \#\#\# SIP phone utilities You will need one or more SIP phone utilities to test calling back and forth to generate some data. Two such programs are [Linphone](https://www.linphone.org/releases/) and [MicroSIP](https://www.microsip.org/downloads).

* **Linphone**

  You need to add an account with the following properties \(substituting `214` with your own user extension if warranted\) ![image](https://user-images.githubusercontent.com/2438810/108548231-659c2d80-72a0-11eb-81a6-47e4389533f9.png)

* **MicroSIP**

  ![image](https://user-images.githubusercontent.com/2438810/108548378-9b411680-72a0-11eb-8672-6245391acf57.png)

Once they are set up, you can try calling from one to the other, and seeing the events being processed by the SMDR Processer.

