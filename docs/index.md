---
hide:
  - navigation
---

# Home

<p align="center">
  <img src="https://ericzimmerman.github.io/pics/logoSmall.jpg">
</p>

## TL;DR

1. **READ** the [Requirements and troubleshooting](https://ericzimmerman.github.io/#requirements-and-troubleshooting) section!!
2. Use [Get-ZimmermanTools](https://download.ericzimmermanstools.com/Get-ZimmermanTools.zip) to download all programs at once and keep your tool set current
    - Use **-Dest** to control where the tools ends up, else things end up in same directory as the script (recommended!)
    - Use **-NetVersion** to control which flavor of tool you get: `4` for .net 4.7.2 or `10` for .net 10 (recommended!), or `0` for all versions. Default is `10`

!!! note

    - **GUI tools** are .net 10 only, except Hasher and TimeApp, which are also built for .net 4.
    - **CLI tools** are built for both .net 4.7.2 and .net 10, except AmcacheParser, SQLECmd, VSCMount and WxTCmd, which are .net 10 only.


## Contribute/support opportunities

[![Donate](https://ericzimmerman.github.io/pics/Quarter16.png)](https://github.com/sponsors/EricZimmerman) **[GitHub Sponsors](https://github.com/sponsors/EricZimmerman)**

[![Donate](https://ericzimmerman.github.io/pics/Quarter16.png)](https://paypal.me/ericrzimmerman) **[PayPal](https://paypal.me/ericrzimmerman)**

## Forensic tools

|Name | <span style="display: inline-block; width:150px">Version (.net 4 &vert; 10)</span> | Purpose | 
|--|--|--
| AmcacheParser | - &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/AmcacheParser.zip) | Amcache.hve parser with lots of extra features, including the Windows 11 26H2 Amcache databases. Handles locked files
| AppCompatCacheParser | [2026.10.0](https://download.ericzimmermanstools.com/AppCompatCacheParser.zip) &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/AppCompatCacheParser.zip) | AppCompatCache aka ShimCache parser. Handles locked files
| bstrings | [2026.10.0](https://download.ericzimmermanstools.com/bstrings.zip) &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/bstrings.zip) | Find them strings yo. Built in regex patterns. Handles locked files
| EvtxECmd | [2026.10.1](https://download.ericzimmermanstools.com/EvtxECmd.zip) &vert; [2026.10.1](https://download.ericzimmermanstools.com/net/EvtxECmd.zip) | Event log (evtx) parser with standardized CSV, XML, and json output! Custom maps, locked file support, and more!
| EZViewer | - &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/EZViewer.zip) | Standalone, zero dependency viewer for .doc, .docx, .xls, .xlsx, .txt, .log, .rtf, .otd, .htm, .html, .mht, .csv, and .pdf. Any non-supported files are shown in a hex editor (with data interpreter!)
| Hasher | [2026.10.0](https://download.ericzimmermanstools.com/hasher.zip) &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/hasher.zip) | Hash all the things
| JLECmd | [2026.10.2](https://download.ericzimmermanstools.com/JLECmd.zip) &vert; [2026.10.2](https://download.ericzimmermanstools.com/net/JLECmd.zip) | Jump List parser
| JumpList Explorer | - &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/JumpListExplorer.zip) | GUI based Jump List viewer
| LECmd | [2026.10.1](https://download.ericzimmermanstools.com/LECmd.zip) &vert; [2026.10.1](https://download.ericzimmermanstools.com/net/LECmd.zip) | Parse lnk files
| MFTECmd | [2026.10.2](https://download.ericzimmermanstools.com/MFTECmd.zip) &vert; [2026.10.2](https://download.ericzimmermanstools.com/net/MFTECmd.zip) | $MFT, $Boot, $J, $SDS, $I30, and $LogFile parser. Handles locked files
| MFTExplorer | - &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/MFTExplorer.zip) | Graphical $MFT viewer
| PECmd | [2026.10.2](https://download.ericzimmermanstools.com/PECmd.zip) &vert; [2026.10.2](https://download.ericzimmermanstools.com/net/PECmd.zip) | Prefetch parser
| RBCmd | [2026.10.0](https://download.ericzimmermanstools.com/RBCmd.zip) &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/RBCmd.zip) | Recycle Bin artifact (INFO2/$I) parser
| RecentFileCacheParser | [2026.10.1](https://download.ericzimmermanstools.com/RecentFileCacheParser.zip) &vert; [2026.10.1](https://download.ericzimmermanstools.com/net/RecentFileCacheParser.zip) | RecentFileCache parser
| RECmd | [2026.10.0](https://download.ericzimmermanstools.com/RECmd.zip) &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/RECmd.zip) | Powerful command line Registry tool searching, multi-hive support, plugins, and more
| Registry Explorer | - &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/RegistryExplorer.zip) | Registry viewer with searching, multi-hive support, plugins, and more. Handles locked files
| RLA | [2026.10.0](https://download.ericzimmermanstools.com/rla.zip) &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/rla.zip) | Replay transaction logs and update Registry hives so they are no longer dirty. Useful when tools do not know how to handle transaction logs
| SBECmd | [2026.10.0](https://download.ericzimmermanstools.com/SBECmd.zip) &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/SBECmd.zip) | ShellBags Explorer, command line edition, for exporting shellbag data
| SDB Explorer | - &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/SDBExplorer.zip) | Shim database GUI
| ShellBags Explorer | - &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/ShellBagsExplorer.zip) | GUI for browsing shellbags data. Handles locked files
| SQLECmd | - &vert; [2026.10.1](https://download.ericzimmermanstools.com/net/SQLECmd.zip) | Find and process SQLite files according to your needs with maps!
| SrumECmd | [2026.10.0](https://download.ericzimmermanstools.com/SrumECmd.zip) &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/SrumECmd.zip) | Process SRUDB.dat and (optionally) SOFTWARE hive for network, process, and energy info!
| SumECmd | [2026.10.0](https://download.ericzimmermanstools.com/SumECmd.zip) &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/SumECmd.zip) | Process Microsoft User Access Logs found under 'C:\Windows\System32\LogFiles\SUM'
| Timeline Explorer | - &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/TimelineExplorer.zip) | View CSV and Excel files, filter, group, sort, etc. with ease
| VSCMount | - &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/VSCMount.zip) | Mount all VSCs on a drive letter to a given mount point
| WxTCmd | - &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/WxTCmd.zip) | Windows 10/11 Timeline database parser

## Other tools

|Name  |<span style="display: inline-block; width:150px">Version (.net 4 &vert; 10)</span> | Purpose
|--|--|--
| Get-ZimmermanTools | [NA](https://download.ericzimmermanstools.com/Get-ZimmermanTools.zip) | PowerShell script to auto discover and update everything above.
| iisGeoLocate | [2026.10.0](https://download.ericzimmermanstools.com/iisGeolocate.zip) &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/iisGeolocate.zip) | Geolocate IP addresses found in IIS logs, extracts unique IPs, records bad data from logs
| KAPE | [NA](https://kape.zip) | Kroll Artifact Parser/Extractor: Flexible, high speed collection of files as well as processing of files. Many many features
| TimeApp | [2026.10.0](https://download.ericzimmermanstools.com/TimeApp.zip) &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/TimeApp.zip) | Shows the current time (local and UTC) to the microsecond, a stopwatch, a countdown, a timestamp converter (Unix, FILETIME, WebKit, Cocoa, OLE) and optionally, public IP addresses. Great for testing
| XWFIM | - &vert; [2026.10.0](https://download.ericzimmermanstools.com/net/XWFIM.zip) | X-Ways Forensics installation manager

## Other files

|Name  |Version| Purpose
|--|--|--
| Change log | [NA](https://download.ericzimmermanstools.com/ChangeLog.txt)| To log changes

***

## Requirements and troubleshooting
!!! warning

     - **DO NOT RUN ANYTHING FOUND HERE FROM 'C:\PROGRAM FILES' DIRECTORY** (unless you run them as administrator)!
     - **DO NOT USE WINDOWS TO EXTRACT THINGS.** Use 7-Zip or WinRAR as Windows will block the DLLs!

 - .net 4 software requires at least [Microsoft .net 4.7.2](https://dotnet.microsoft.com/en-us/download/dotnet-framework/net472) or newer (TimeApp: [.net 4.8](https://dotnet.microsoft.com/en-us/download/dotnet-framework/net48))! You will get errors running these without it. When in doubt, install it!
 - .net 10 software requires at least [Microsoft .net 10](https://dotnet.microsoft.com/en-us/download/dotnet/10.0) or newer! You will get errors running these without at least .net 10. When in doubt, install it! Be sure sure you get the **Desktop** runtime if you plan on running any of the GUI programs
 - All software is digitally signed. Once you verify the signature as coming from me, any anti-virus hits are false positives. When in doubt, download the files directly from here!
 

!!! tip

    The GUI programs scale themselves for each monitor (Per-Monitor V2 DPI awareness): they should look sharp and be the right size on any display, also when moved between monitors with different scaling. If you set **Override high DPI scaling behavior** on a shortcut or exe for an older version (Properties, Compatibility, Change high DPI settings), clear it: it now makes them blurry

***

Open Source Development funding and support provided by the following contributors: [SANS Institute](http://sans.org/) and [SANS DFIR](http://dfir.sans.org/).
