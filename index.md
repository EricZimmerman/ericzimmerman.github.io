<p align="center">
  <img src="https://ericzimmerman.github.io/logoSmall.jpg">
</p>


## TL;DR

1. **READ** the [Requirements and troubleshooting](https://ericzimmerman.github.io/#!index.md#Requirements_and_troubleshooting) section!!
2. Use [Get-ZimmermanTools](https://f001.backblazeb2.com/file/EricZimmermanTools/Get-ZimmermanTools.zip) to download all programs at once and keep your toolset current
    - Use **-Dest** to control where the tools ends up, else things end up in same directory as the script (recommended!)
    - Use **-NetVersion** to control which flavor of tool you get: 4 for net 4.6.2 and 6 for net 6 (recommended!)
3. All **GUI tools** will be updated to use .net 6 only but the legacy version will be kept in place as well (just not updated anymore)
4. All **CLI tools** will continue to be built for both net 4.6.2 and net 6

## Contribute/support opportunities

[![Donate](https://ericzimmerman.github.io/Quarter16.png)](https://github.com/sponsors/EricZimmerman) **[GitHub Sponsors](https://github.com/sponsors/EricZimmerman)**

[![Donate](https://ericzimmerman.github.io/Quarter16.png)](https://paypal.me/ericrzimmerman) **[Paypal](https://paypal.me/ericrzimmerman)**

[![Donate](https://ericzimmerman.github.io/Quarter16.png)](https://www.patreon.com/ericzimmerman) **[Patreon](https://www.patreon.com/ericzimmerman)**



## Forensic tools

Need everything at once? Here are ALL the tools as a single [ZIP](https://f001.backblazeb2.com/file/EricZimmermanTools/All.zip)

|Name | <span style="display: inline-block; width:150px">Version (.net 4 &vert; 6)</span> | Purpose | 
|--|--|--
| AmcacheParser | [1.4.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/AmcacheParser.zip) &vert; na | Amcache.hve parser with lots of extra features. Handles locked files
| AppCompatCacheParser | [1.5.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/AppCompatCacheParser.zip) &vert; [1.5.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/net6/AppCompatCacheParser.zip) | AppCompatCache aka ShimCache parser. Handles locked files
| bstrings | [1.5.2.0](https://f001.backblazeb2.com/file/EricZimmermanTools/bstrings.zip) &vert; [1.5.2.0](https://f001.backblazeb2.com/file/EricZimmermanTools/net6/bstrings.zip) | Find them strings yo. Built in regex patterns. Handles locked files
| EZViewer | [1.0.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/EZViewer.zip) &vert; na | Standalone, zero dependency viewer for .doc, .docx, .xls, .xlsx, .txt, .log, .rtf, .otd, .htm, .html, .mht, .csv, and .pdf. Any non-supported files are shown in a hex editor (with data interpreter!)
| Evtx Explorer/EvtxECmd | [0.6.5.0](https://f001.backblazeb2.com/file/EricZimmermanTools/EvtxExplorer.zip) &vert; na | Event log (evtx) parser with standardized CSV, XML, and json output! Custom maps, locked file support, and more!
| Hasher | [1.9.3.0](https://f001.backblazeb2.com/file/EricZimmermanTools/hasher.zip) &vert; na| Hash all the things
| JLECmd | [1.4.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/JLECmd.zip) &vert; na| Jump List parser
| JumpList Explorer | [1.4.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/JumpListExplorer.zip) &vert; na | GUI based Jump List viewer 
| LECmd  | [1.4.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/LECmd.zip) &vert; na| Parse lnk files
| MFTECmd |[0.5.2.0](https://f001.backblazeb2.com/file/EricZimmermanTools/MFTECmd.zip) &vert; [0.5.2.0](https://f001.backblazeb2.com/file/EricZimmermanTools/net6/MFTECmd.zip)| $MFT, $Boot, $J, $SDS, and $LogFile (coming soon) parser. Handles locked files
| MFTExplorer | [0.5.1.0](https://f001.backblazeb2.com/file/EricZimmermanTools/MFTExplorer.zip) &vert; na | Graphical $MFT viewer
| PECmd  | [1.4.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/PECmd.zip) &vert; na| Prefetch parser
| RBCmd  | [0.5.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/RBCmd.zip) &vert; na| Recycle Bin artifact (INFO2/$I) parser
| RecentFileCacheParser | [1.0.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/RecentFileCacheParser.zip) &vert; na| RecentFileCache parser
| Registry Explorer/RECmd |  [1.6.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/RegistryExplorer_RECmd.zip) &vert; na | Registry viewer with searching, multi-hive support, plugins, and more. Handles locked files
| SDB Explorer |  [1.0.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/SDBExplorer.zip) &vert; na | Shim database GUI
| ShellBags Explorer | [1.4.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/ShellBagsExplorer.zip) &vert; na| GUI for browsing shellbags data. Handles locked files
| SQLECmd | [0.5.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/SQLECmd.zip) &vert; na| Find and process SQLite files according to your needs with maps!
| SumECmd | [0.5.2.0](https://f001.backblazeb2.com/file/EricZimmermanTools/SumECmd.zip) &vert; [0.5.2.0](https://f001.backblazeb2.com/file/EricZimmermanTools/net6/SumECmd.zip) | Process Microsoft User Access Logs found under 'C:\Windows\System32\LogFiles\SUM'
| SrumECmd | [0.5.1.0](https://f001.backblazeb2.com/file/EricZimmermanTools/SrumECmd.zip) &vert; [0.5.1.0](https://f001.backblazeb2.com/file/EricZimmermanTools/net6/SrumECmd.zip) | Process SRUDB.dat and (optionally) SOFTWARE hive for network, process, and energy info!
| Timeline Explorer | [1.3.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/TimelineExplorer.zip) &vert; na| View CSV and Excel files, filter, group, sort, etc. with ease
| VSCMount |[1.0.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/VSCMount.zip) &vert; na| Mount all VSCs on a drive letter to a given mount point
| WxTCmd | [0.6.0.0](https://f001.backblazeb2.com/file/EricZimmermanTools/WxTCmd.zip) &vert; na| Windows 10 Timeline database parser

***

## Other tools

|Name  |Version| Purpose
|--|--|--
| KAPE | [NA](https://learn.duffandphelps.com/kape?utm_campaign=2019_cyberitbn-KAPE-launch&utm_source=kroll&utm_medium=referral&utm_term=kape-gui-blogpost) | Kroll Artifact Parser/Extractor: Flexible, high speed collection of files as well as processing of files. Many many features
| iisGeoLocate | [2.0.0.2](https://f001.backblazeb2.com/file/EricZimmermanTools/iisGeolocate.zip) &vert; na | Geolocate IP addresses found in IIS logs, extracts unique IPs, records bad data from logs
| TimeApp | [NA](https://f001.backblazeb2.com/file/EricZimmermanTools/TimeApp.zip) &vert; na| A simple app that shows current time (local and UTC) and optionally, public IP address. Great for testing
| XWFIM | [NA](https://f001.backblazeb2.com/file/EricZimmermanTools/XWFIM.zip)  &vert; na | X-Ways Forensics installation manager
| Get-ZimmermanTools | [NA](https://f001.backblazeb2.com/file/EricZimmermanTools/Get-ZimmermanTools.zip) | PowerShell script to auto discover and update everything above.



***


## Other files

|Name  |Version| Purpose
|--|--|--
| Change log | [NA](https://f001.backblazeb2.com/file/EricZimmermanTools/ChangeLog.txt)| 




***
## Requirements and troubleshooting

 - .net 4 software requires at least [Microsoft .net 4.6.2](https://dotnet.microsoft.com/en-us/download/dotnet-framework/net462) or newer! You will get errors running these without at least 4.6.2. When in doubt, install it!
 - .net 6 software requires at least [Microsoft .net 6](https://dotnet.microsoft.com/en-us/download/dotnet/6.0) or newer! You will get errors running these without at least 6. When in doubt, install it!
 - **DO NOT RUN ANYTHING FOUND HERE FROM 'C:\PROGRAM FILES' DIRECTORY** (unless you run them as administrator)!
 - **DO NOT USE WINDOWS TO EXTRACT THINGS.** Use 7-Zip or Winrar as Windows will block the DLLs!
 - All software is digitally signed. Once you verify the signature as coming from me, any anti-virus hits are false positives. When in doubt, download the files directly from here!
 - **If you get DPI scaling issues, make a shortcut (or directly against the exe), edit the properties, then click Compatibility. Under Change high DPI settings, check Override high DPI scaling behavior at bottom and choose System, then click OK out of the dialog**

***


Open Source Development funding and support provided by the following contributors: [SANS Institute](http://sans.org/) and [SANS DFIR](http://dfir.sans.org/).
