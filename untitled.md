# Agent Service

## Building

The agent service manages the MiVoice Connect \(aka ShoreTel\) real-time support, and so uses the ShoreTel Multi-Line COM Control to interface to the ShoreTel TAPI provider. In order to build the agent you will need to register the COM control in the system.

1. Open an administrator command prompt
2. Go to the `Dependencies` folder at the root of the working tree.
3. Run `C:\windows\SysWOW64\regsvr32.exe STMLControl.dll` to register the COM control.  The SysWOW64 is necessary because it needs to get registered in the 32-bit COM context, as the agent is set to run 32-bit.

## Running

The agent looks for registry keys from the agent installation when it runs, namely the identity GUID and the username/password credentials. Usually you will need the agent to run with the identity of one of our existing agents, to take over what it would normally be doing. In that case you can copy the registry keys from that system. You will need to stop the agent service on that system anyway so that they aren't both running at the same time.

Alternatively, if the agent is not being run for a specific data source, or if it is a new installation, just install the agent normally \(see the [Reinstalling the Brightmetrics Agent](https://brightmetrics.zendesk.com/hc/en-us/articles/210661243) help center article\), and then uninstall or stop the service when you want to run the agent under debugging.

To debug in Visual Studio, the `AgentService2` project can be set as the startup project, and the debug properties set to pass `debug` as the only command line argument.

