# ShoreTel Realtime

## Running

ShoreTel \(aka MiVoice Connect, or MiVC\) Real Time can only be run from a ShoreTel application server \(aka Distributed Voicemail Server, or DVS\) or the Headquarters \(HQ\) server. However, it is possible to install the ShoreTel Server TAPI Provider on a workstation for local debugging. For doing so, refer to the ShoreTel installation guide section for installing the TAPI provider on a Windows Terminal Server. A VPN connection to the ShoreTel HQ server is required.

## Debugging

The Real Time component writes a log file to C:\Windows\ServiceProfiles\LocalService\AppData\Local\Temp\brightmetricsagentservicert.txt \(or whatever "%TEMP%\brightmetricsagentservicert.txt" points to\).

It is possible to capture debug logs from the ShoreTel TAPI COM SDK by creating a registry value named `TapiDebugLevel` under the key `HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Brightmetrics\Agent` set to the string value `medium`.

That will create files in `C:\Windows\ServiceProfiles\LocalService\AppData\Local\Temp` named `brightmetricsagentservicetapi_MONDAY.txt`, etc \(one for each day of the week\).

The possible values for TapiDebugLevel are `low`, `medium`, and `high`. They can get quite large at high if there is a lot of call volume, so that's not recommended, unless it is the only way to get the level of detail needed for troubleshooting.

