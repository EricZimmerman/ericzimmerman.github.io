<p align="center">
  <img src="https://ericzimmerman.github.io/logoSmall.jpg">
</p>

## TL;DR

1. **READ** the [Requirements and troubleshooting](https://ericzimmerman.github.io/#!index.md#requirements-and-troubleshooting) section!!
2. Use [Get-ZimmermanTools](https://download.ericzimmermanstools.com/Get-ZimmermanTools.zip) to download all programs at once and keep your tool set current
    - Use **-Dest** to control where the tools ends up, else things end up in same directory as the script (recommended!)
    - Use **-NetVersion** to control which flavor of tool you get: 4 for .net 4.6.2 and 6 for .net 6, or 9 for .net 9 (recommended!)
    - The default is still net6 to allow people time to transition to .net 9
3. All **GUI tools** will be updated to use .net 9 only but the legacy version will be kept in place as well (just not updated anymore)
4. All **CLI tools** will continue to be built for both .net 4.6.2, .net 6 and .net 9 (for now)
5. **NOTE** net6 GUI tools will NOT be updated anymore as it is EOL. Only net9 GUIs will be updated from this point forward. NOTE THE VERSION # DIFFERENCES

## Contribute/support opportunities

[![Donate](https://ericzimmerman.github.io/Quarter16.png)](https://github.com/sponsors/EricZimmerman) **[GitHub Sponsors](https://github.com/sponsors/EricZimmerman)**

[![Donate](https://ericzimmerman.github.io/Quarter16.png)](https://paypal.me/ericrzimmerman) **[PayPal](https://paypal.me/ericrzimmerman)**

## Forensic tools

|Name | <span style="display: inline-block; width:150px">Version (.net 4 &vert; 6 &vert; 9)</span> | Purpose | 
|--|--|--
| AmcacheParser | [1.5.2](https://download.ericzimmermanstools.com/AmcacheParser.zip) &vert; [1.5.2](https://download.ericzimmermanstools.com/net6/AmcacheParser.zip) &vert; [1.5.2](https://download.ericzimmermanstools.com/net9/AmcacheParser.zip)| Amcache.hve parser with lots of extra features. Handles locked files
| AppCompatCacheParser | [1.5.1](https://download.ericzimmermanstools.com/AppCompatCacheParser.zip) &vert; [1.5.1](https://download.ericzimmermanstools.com/net6/AppCompatCacheParser.zip) &vert; [1.5.1](https://download.ericzimmermanstools.com/net9/AppCompatCacheParser.zip)| AppCompatCache aka ShimCache parser. Handles locked files
| bstrings | [1.5.3](https://download.ericzimmermanstools.com/bstrings.zip) &vert; [1.5.3](https://download.ericzimmermanstools.com/net6/bstrings.zip) &vert; [1.5.3](https://download.ericzimmermanstools.com/net9/bstrings.zip)| Find them strings yo. Built in regex patterns. Handles locked files
| EvtxECmd | [1.5.1](https://download.ericzimmermanstools.com/EvtxECmd.zip) &vert; [1.5.1](https://download.ericzimmermanstools.com/net6/EvtxECmd.zip) &vert; [1.5.1](https://download.ericzimmermanstools.com/net9/EvtxECmd.zip)| Event log (evtx) parser with standardized CSV, XML, and json output! Custom maps, locked file support, and more!
| EZViewer | - &vert; [2.0.0](https://download.ericzimmermanstools.com/net6/EZViewer.zip) &vert; [2.1.0](https://download.ericzimmermanstools.com/net9/EZViewer.zip) | Standalone, zero dependency viewer for .doc, .docx, .xls, .xlsx, .txt, .log, .rtf, .otd, .htm, .html, .mht, .csv, and .pdf. Any non-supported files are shown in a hex editor (with data interpreter!)
| Hasher | [2.1.0](https://download.ericzimmermanstools.com/hasher.zip) &vert; - &vert; -| Hash all the things
| JLECmd | [1.5.1](https://download.ericzimmermanstools.com/JLECmd.zip) &vert; [1.5.1](https://download.ericzimmermanstools.com/net6/JLECmd.zip) &vert; [1.5.1](https://download.ericzimmermanstools.com/net9/JLECmd.zip)| Jump List parser
| JumpList Explorer | - &vert; [2.0.0](https://download.ericzimmermanstools.com/net6/JumpListExplorer.zip) &vert; [2.1.0](https://download.ericzimmermanstools.com/net9/JumpListExplorer.zip) | GUI based Jump List viewer 
| LECmd  | [1.5.1](https://download.ericzimmermanstools.com/LECmd.zip) &vert; [1.5.1](https://download.ericzimmermanstools.com/net6/LECmd.zip) &vert; [1.5.1](https://download.ericzimmermanstools.com/net9/LECmd.zip)| Parse lnk files
| MFTECmd |[1.3.0](https://download.ericzimmermanstools.com/MFTECmd.zip) &vert; [1.3.0](https://download.ericzimmermanstools.com/net6/MFTECmd.zip) &vert; [1.3.0](https://download.ericzimmermanstools.com/net9/MFTECmd.zip)| $MFT, $Boot, $J, $SDS, $I30, and $LogFile (coming soon) parser. Handles locked files
| MFTExplorer | - &vert; [2.0.0](https://download.ericzimmermanstools.com/net6/MFTExplorer.zip) &vert; [2.1.0](https://download.ericzimmermanstools.com/net9/MFTExplorer.zip)| Graphical $MFT viewer
| PECmd | [1.5.1](https://download.ericzimmermanstools.com/PECmd.zip) &vert; [1.5.1](https://download.ericzimmermanstools.com/net6/PECmd.zip) &vert; [1.5.1](https://download.ericzimmermanstools.com/net9/PECmd.zip)| Prefetch parser
| RBCmd | [1.6.1](https://download.ericzimmermanstools.com/RBCmd.zip) &vert; [1.6.1](https://download.ericzimmermanstools.com/net6/RBCmd.zip) &vert; [1.6.1](https://download.ericzimmermanstools.com/net9/RBCmd.zip)| Recycle Bin artifact (INFO2/$I) parser
| RecentFileCacheParser | [1.5.1](https://download.ericzimmermanstools.com/RecentFileCacheParser.zip) &vert; [1.5.1](https://download.ericzimmermanstools.com/net6/RecentFileCacheParser.zip) &vert; [1.5.1](https://download.ericzimmermanstools.com/net9/RecentFileCacheParser.zip)| RecentFileCache parser
| RECmd | [2.1.0](https://download.ericzimmermanstools.com/RECmd.zip) &vert; [2.1.0](https://download.ericzimmermanstools.com/net6/RECmd.zip) &vert; [2.1.0](https://download.ericzimmermanstools.com/net9/RECmd.zip) | Powerful command line Registry tool searching, multi-hive support, plugins, and more
| Registry Explorer | - &vert; [2.0.0](https://download.ericzimmermanstools.com/net6/RegistryExplorer.zip) &vert; [2.1.0](https://download.ericzimmermanstools.com/net9/RegistryExplorer.zip)| Registry viewer with searching, multi-hive support, plugins, and more. Handles locked files
| RLA | [2.1.0](https://download.ericzimmermanstools.com/rla.zip) &vert; [2.1.0](https://download.ericzimmermanstools.com/net6/rla.zip) &vert; [2.1.0](https://download.ericzimmermanstools.com/net9/rla.zip)| Replay transaction logs and update Registry hives so they are no longer dirty. Useful when tools do not know how to handle transaction logs
| SDB Explorer |  - &vert; [2.0.0](https://download.ericzimmermanstools.com/net6/SDBExplorer.zip) &vert; [2.1.0](https://download.ericzimmermanstools.com/net9/SDBExplorer.zip)| Shim database GUI
| SBECmd | [2.1.0](https://download.ericzimmermanstools.com/SBECmd.zip) &vert; [2.1.0](https://download.ericzimmermanstools.com/net6/SBECmd.zip) &vert; [2.1.0](https://download.ericzimmermanstools.com/net9/SBECmd.zip) | ShellBags Explorer, command line edition, for exporting shellbag data
| ShellBags Explorer | - &vert; [2.0.0](https://download.ericzimmermanstools.com/net6/ShellBagsExplorer.zip) &vert; [2.1.0](https://download.ericzimmermanstools.com/net9/ShellBagsExplorer.zip) | GUI for browsing shellbags data. Handles locked files
| SQLECmd | [1.1.0](https://download.ericzimmermanstools.com/SQLECmd.zip) &vert; [1.1.0](https://download.ericzimmermanstools.com/net6/SQLECmd.zip) &vert; [1.1.0](https://download.ericzimmermanstools.com/net9/SQLECmd.zip) | Find and process SQLite files according to your needs with maps!
| SrumECmd | [1.0.0](https://download.ericzimmermanstools.com/SrumECmd.zip) &vert; [1.0.0](https://download.ericzimmermanstools.com/net6/SrumECmd.zip) &vert; [1.0.0](https://download.ericzimmermanstools.com/net9/SrumECmd.zip) | Process SRUDB.dat and (optionally) SOFTWARE hive for network, process, and energy info!
| SumECmd | [1.0.0](https://download.ericzimmermanstools.com/SumECmd.zip) &vert; [1.0.0](https://download.ericzimmermanstools.com/net6/SumECmd.zip) &vert; [1.0.0](https://download.ericzimmermanstools.com/net9/SumECmd.zip) | Process Microsoft User Access Logs found under 'C:\Windows\System32\LogFiles\SUM'
| Timeline Explorer | - &vert; [2.0.0.1](https://download.ericzimmermanstools.com/net6/TimelineExplorer.zip) &vert; [2.1.0](https://download.ericzimmermanstools.com/net9/TimelineExplorer.zip) | View CSV and Excel files, filter, group, sort, etc. with ease
| VSCMount | - &vert; [1.5.0](https://download.ericzimmermanstools.com/net6/VSCMount.zip) &vert; [1.5.0](https://download.ericzimmermanstools.com/net9/VSCMount.zip) | Mount all VSCs on a drive letter to a given mount point
| WxTCmd | [1.1.0](https://download.ericzimmermanstools.com/WxTCmd.zip) &vert; [1.1.0](https://download.ericzimmermanstools.com/net6/WxTCmd.zip) &vert; [1.1.0](https://download.ericzimmermanstools.com/net9/WxTCmd.zip) | Windows 10 Timeline database parser

***

## Other tools

|Name  |<span style="display: inline-block; width:150px">Version (.net 4 &vert; 6 &vert; 9)</span> | Purpose
|--|--|--
| Get-ZimmermanTools | [NA](https://download.ericzimmermanstools.com/Get-ZimmermanTools.zip) | PowerShell script to auto discover and update everything above.
| iisGeoLocate | [2.2.0](https://download.ericzimmermanstools.com/iisGeolocate.zip) &vert; [2.2.0](https://download.ericzimmermanstools.com/net6/iisGeolocate.zip) &vert; [2.2.0](https://download.ericzimmermanstools.com/net9/iisGeolocate.zip) | Geolocate IP addresses found in IIS logs, extracts unique IPs, records bad data from logs
| KAPE | [NA](https://learn.duffandphelps.com/kape?utm_campaign=2019_cyberitbn-KAPE-launch&utm_source=kroll&utm_medium=referral&utm_term=kape-gui-blogpost) | Kroll Artifact Parser/Extractor: Flexible, high speed collection of files as well as processing of files. Many many features
| TimeApp | [NA](https://download.ericzimmermanstools.com/TimeApp.zip) &vert; na &vert; na | A simple app that shows current time (local and UTC) and optionally, public IP address. Great for testing
| XWFIM | [NA](https://download.ericzimmermanstools.com/XWFIM.zip)  &vert; na &vert; na | X-Ways Forensics installation manager

***

## Other files

|Name  |Version| Purpose
|--|--|--
| Change log | [NA](https://download.ericzimmermanstools.com/ChangeLog.txt)| 

***

## Requirements and troubleshooting

 - .net 4 software requires at least [Microsoft .net 4.6.2](https://dotnet.microsoft.com/en-us/download/dotnet-framework/net462) or newer! You will get errors running these without at least 4.6.2. When in doubt, install it!
 - .net 6 software requires at least [Microsoft .net 6](https://dotnet.microsoft.com/en-us/download/dotnet/6.0) or newer! You will get errors running these without at least .net 6. When in doubt, install it! NOTE: Make sure you get the **Desktop** runtime if you plan on running any of
 the GUI programs
 - .net 9 software requires at least [Microsoft .net 9](https://dotnet.microsoft.com/en-us/download/dotnet/9.0) or newer! You will get errors running these without at least .net 9. When in doubt, install it! NOTE: Make sure you get the **Desktop** runtime if you plan on running any of
 the GUI programs
 - **DO NOT RUN ANYTHING FOUND HERE FROM 'C:\PROGRAM FILES' DIRECTORY** (unless you run them as administrator)!
 - **DO NOT USE WINDOWS TO EXTRACT THINGS.** Use 7-Zip or WinRAR as Windows will block the DLLs!
 - All software is digitally signed. Once you verify the signature as coming from me, any anti-virus hits are false positives. When in doubt, download the files directly from here!
 - **If you get DPI scaling issues, make a shortcut (or directly against the exe), edit the properties, then click Compatibility. Under Change high DPI settings, check Override high DPI scaling behavior at bottom and choose System, then click OK out of the dialog**

***

Open Source Development funding and support provided by the following contributors: [SANS Institute](http://sans.org/) and [SANS DFIR](http://dfir.sans.org/).
