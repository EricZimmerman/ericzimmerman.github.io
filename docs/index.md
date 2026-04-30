<p align="center">
  <img src="https://ericzimmerman.github.io/logoSmall.jpg">
</p>

## TL;DR

1. **READ** the [Requirements and troubleshooting](https://ericzimmerman.github.io/#!index.md#requirements-and-troubleshooting) section!!
2. Use [Get-ZimmermanTools](https://download.ericzimmermanstools.com/Get-ZimmermanTools.zip) to download all programs at once and keep your tool set current
    - Use **-Dest** to control where the tools ends up, else things end up in same directory as the script (recommended!)
    - Use **-NetVersion** to control which flavor of tool you get: 4 for .net 4.7.2 or 9 for .net 9 (recommended!), or 0 for all versions. Default is 9
3. All **GUI tools** will be updated to use .net 9 only but the legacy net4 version will be kept in place as well (just not updated anymore)
4. All **CLI tools** will continue to be built for both .net 4.7.2 and .net 9 (for now)

**NOTE** Only net9 GUIs will be updated from this point forward. NOTE THE VERSION # DIFFERENCES

## Contribute/support opportunities

[![Donate](https://ericzimmerman.github.io/Quarter16.png)](https://github.com/sponsors/EricZimmerman) **[GitHub Sponsors](https://github.com/sponsors/EricZimmerman)**

[![Donate](https://ericzimmerman.github.io/Quarter16.png)](https://paypal.me/ericrzimmerman) **[PayPal](https://paypal.me/ericrzimmerman)**

## Forensic tools

|Name | <span style="display: inline-block; width:150px">Version (.net 4 &vert; 9)</span> | Purpose | 
|--|--|--
| AmcacheParser | [2026.5.0](https://download.ericzimmermanstools.com/AmcacheParser.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/AmcacheParser.zip)| Amcache.hve parser with lots of extra features. Handles locked files
| AppCompatCacheParser | [2026.5.0](https://download.ericzimmermanstools.com/AppCompatCacheParser.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/AppCompatCacheParser.zip)| AppCompatCache aka ShimCache parser. Handles locked files
| bstrings | [2026.5.0](https://download.ericzimmermanstools.com/bstrings.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/bstrings.zip)| Find them strings yo. Built in regex patterns. Handles locked files
| EvtxECmd | [2026.5.0](https://download.ericzimmermanstools.com/EvtxECmd.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/EvtxECmd.zip)| Event log (evtx) parser with standardized CSV, XML, and json output! Custom maps, locked file support, and more!
| EZViewer | - &vert; [2026.5.0](https://download.ericzimmermanstools.com/net9/EZViewer.zip) | Standalone, zero dependency viewer for .doc, .docx, .xls, .xlsx, .txt, .log, .rtf, .otd, .htm, .html, .mht, .csv, and .pdf. Any non-supported files are shown in a hex editor (with data interpreter!)
| Hasher | [2026.5.0](https://download.ericzimmermanstools.com/hasher.zip) &vert; - | Hash all the things
| JLECmd | [2026.5.0](https://download.ericzimmermanstools.com/JLECmd.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/JLECmd.zip)| Jump List parser
| JumpList Explorer | - &vert; [2026.5.0](https://download.ericzimmermanstools.com/net9/JumpListExplorer.zip) | GUI based Jump List viewer 
| LECmd  | [2026.5.0](https://download.ericzimmermanstools.com/LECmd.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/LECmd.zip)| Parse lnk files
| MFTECmd |[2026.5.0](https://download.ericzimmermanstools.com/MFTECmd.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/MFTECmd.zip)| $MFT, $Boot, $J, $SDS, $I30, and $LogFile (coming soon) parser. Handles locked files
| MFTExplorer | - &vert; [2026.5.0](https://download.ericzimmermanstools.com/net9/MFTExplorer.zip)| Graphical $MFT viewer
| PECmd | [2026.5.0](https://download.ericzimmermanstools.com/PECmd.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/PECmd.zip)| Prefetch parser
| RBCmd | [2026.5.0](https://download.ericzimmermanstools.com/RBCmd.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/RBCmd.zip)| Recycle Bin artifact (INFO2/$I) parser
| RecentFileCacheParser | [2026.5.0](https://download.ericzimmermanstools.com/RecentFileCacheParser.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/RecentFileCacheParser.zip)| RecentFileCache parser
| RECmd | [2026.5.0](https://download.ericzimmermanstools.com/RECmd.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/RECmd.zip) | Powerful command line Registry tool searching, multi-hive support, plugins, and more
| Registry Explorer | - &vert; [2026.5.0](https://download.ericzimmermanstools.com/net9/RegistryExplorer.zip)| Registry viewer with searching, multi-hive support, plugins, and more. Handles locked files
| RLA | [2026.5.0](https://download.ericzimmermanstools.com/rla.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/rla.zip)| Replay transaction logs and update Registry hives so they are no longer dirty. Useful when tools do not know how to handle transaction logs
| SDB Explorer |  - &vert; [2026.5.0](https://download.ericzimmermanstools.com/net9/SDBExplorer.zip)| Shim database GUI
| SBECmd | [2026.5.0](https://download.ericzimmermanstools.com/SBECmd.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/SBECmd.zip) | ShellBags Explorer, command line edition, for exporting shellbag data
| ShellBags Explorer | -  &vert; [2026.5.0](https://download.ericzimmermanstools.com/net9/ShellBagsExplorer.zip) | GUI for browsing shellbags data. Handles locked files
| SQLECmd | - &vert; [2026.5.0](https://download.ericzimmermanstools.com/net9/SQLECmd.zip) | Find and process SQLite files according to your needs with maps!
| SrumECmd | [2026.5.0](https://download.ericzimmermanstools.com/SrumECmd.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/SrumECmd.zip) | Process SRUDB.dat and (optionally) SOFTWARE hive for network, process, and energy info!
| SumECmd | [2026.5.0](https://download.ericzimmermanstools.com/SumECmd.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/SumECmd.zip) | Process Microsoft User Access Logs found under 'C:\Windows\System32\LogFiles\SUM'
| Timeline Explorer | - &vert; [2026.5.0](https://download.ericzimmermanstools.com/net9/TimelineExplorer.zip) | View CSV and Excel files, filter, group, sort, etc. with ease
| VSCMount | - &vert; [2026.5.0](https://download.ericzimmermanstools.com/net9/VSCMount.zip) | Mount all VSCs on a drive letter to a given mount point
| WxTCmd | - &vert; [2026.5.0](https://download.ericzimmermanstools.com/net9/WxTCmd.zip) | Windows 10 Timeline database parser

***

## Other tools

|Name  |<span style="display: inline-block; width:150px">Version (.net 4 &vert; 9)</span> | Purpose
|--|--|--
| Get-ZimmermanTools | [NA](https://download.ericzimmermanstools.com/Get-ZimmermanTools.zip) | PowerShell script to auto discover and update everything above.
| iisGeoLocate | [2026.5.0](https://download.ericzimmermanstools.com/iisGeolocate.zip) &vert;  [2026.5.0](https://download.ericzimmermanstools.com/net9/iisGeolocate.zip) | Geolocate IP addresses found in IIS logs, extracts unique IPs, records bad data from logs
| KAPE | [NA](https://kape/zip) | Kroll Artifact Parser/Extractor: Flexible, high speed collection of files as well as processing of files. Many many features
| TimeApp | [2026.5.0](https://download.ericzimmermanstools.com/TimeApp.zip) &vert; na | A simple app that shows current time (local and UTC) and optionally, public IP address. Great for testing
| XWFIM | [NA](https://download.ericzimmermanstools.com/XWFIM.zip) &vert; na | X-Ways Forensics installation manager

***

## Other files

|Name  |Version| Purpose
|--|--|--
| Change log | [NA](https://download.ericzimmermanstools.com/ChangeLog.txt)| 

***

## Requirements and troubleshooting

 - .net 4 software requires at least [Microsoft .net 4.7.2](https://dotnet.microsoft.com/en-us/download/dotnet-framework/net472) or newer! You will get errors running these without at least 4.7.2. When in doubt, install it!
 - .net 9 software requires at least [Microsoft .net 9](https://dotnet.microsoft.com/en-us/download/dotnet/9.0) or newer! You will get errors running these without at least .net 9. When in doubt, install it! NOTE: Make sure you get the **Desktop** runtime if you plan on running any of the GUI programs
 - **DO NOT RUN ANYTHING FOUND HERE FROM 'C:\PROGRAM FILES' DIRECTORY** (unless you run them as administrator)!
 - **DO NOT USE WINDOWS TO EXTRACT THINGS.** Use 7-Zip or WinRAR as Windows will block the DLLs!
 - All software is digitally signed. Once you verify the signature as coming from me, any anti-virus hits are false positives. When in doubt, download the files directly from here!
 - **If you get DPI scaling issues, make a shortcut (or directly against the exe), edit the properties, then click Compatibility. Under Change high DPI settings, check Override high DPI scaling behavior at bottom and choose System, then click OK out of the dialog**

***

Open Source Development funding and support provided by the following contributors: [SANS Institute](http://sans.org/) and [SANS DFIR](http://dfir.sans.org/).
