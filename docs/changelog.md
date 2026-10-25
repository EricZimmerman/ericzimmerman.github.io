---
hide:
  - navigation
---

# 📜 ChangeLog

User-facing changes to Eric Zimmerman's tools, newest first. Get the tools from the [home page](index.md), or all at
once (and kept current) with [Get-ZimmermanTools](https://download.ericzimmermanstools.com/Get-ZimmermanTools.zip).

⌨️ command line tool &nbsp; 🖥️ GUI &nbsp; 📚 library &nbsp; 📜 script &nbsp; ⚡ speed or memory

## 🚀 2026.10 (October 2026)

!!! note "In this release"

    All tools now target .NET 10 (LTS) instead of .NET 9 (AmcacheParser, VSCMount and WxTCmd only .NET 10), and return exit code 1 when the tool fails (invalid arguments, a missing input path, output that can't be written, files or folders it can't read) and 0 otherwise: damaged files are reported but are not a failure.

    New in the October updates: --co in every command line tool except VSCMount saves all console messages, with times and levels, to &lt;yyyyMMddHHmmss&gt;\_&lt;Tool&gt;\_&lt;version&gt;\_ConsoleLog.txt in the output directory (SBECmd: on by default, replacing !SBECmd\_Messages.txt; Issues #259).

| Program | Version | Program | Version | Program | Version |
|--|--|--|--|--|--|
| AmcacheParser | 2026.10.1 | LECmd | 2026.10.3 | SDB Explorer | 2026.10.0 |
| AppCompatCacheParser | 2026.10.1 | MFTECmd | 2026.10.5 | ShellBags Explorer | 2026.10.1 |
| bstrings | 2026.10.1 | PECmd | 2026.10.3 | SQLECmd | 2026.10.2 |
| EvtxECmd | 2026.10.2 | RBCmd | 2026.10.1 | SrumECmd | 2026.10.1 |
| EZViewer | 2026.10.0 | RECmd | 2026.10.2 | SumECmd | 2026.10.1 |
| Hasher | 2026.10.0 | rla | 2026.10.1 | VSCMount | 2026.10.0 |
| iisGeolocate | 2026.10.1 | RecentFileCacheParser | 2026.10.2 | WxTCmd | 2026.10.1 |
| JLECmd | 2026.10.4 | Registry Explorer | 2026.10.1 | | |
| JumpList Explorer | 2026.10.1 | SBECmd | 2026.10.2 | | |

### 📚 Registry

*The library used by RECmd and rla, Registry Explorer, ShellBags Explorer and SBECmd, AmcacheParser, AppCompatCacheParser and the RECmd plugins; they get these with their next release*

- ⚡ About 2x faster with a sixth of the allocations and less memory (153 MB SOFTWARE hive: 4.5 s → 2.3 s, 13.2 GB → 2.1 GB allocated, 1.22 GB → 1.19 GB peak)
- Transaction logs are applied as Windows applies them: only the entries from the log's sequence number on, in order. Older entries left in a log were applied over newer data, which could leave a hive that would not load (for example an Amcache that failed with "The given key was not present in the dictionary")
- Logs that grow the hive are applied (#24); both logs are used in the right order, and a hive whose header is damaged gets it back from the log
- Old format transaction logs (Windows 8 and earlier) are applied (they were ignored)
- Hives replayed from their logs are byte for byte what yarp produces, for every dirty hive tested
- More deleted keys and values are recovered, such as ones whose data runs past the end of the list they were left in (#20): 693 more keys over the test hives, including a deleted USB storage device key in the Lone Wolf scenario's SYSTEM hive
- Value names made only of characters such as Japanese, Chinese or Cyrillic are read (they showed "(Unable to determine name)"; PR #37 by mnrkbys)
- One byte key and value names are read as Windows reads them (bytes 0x80-0x9F were turned into letters such as Ÿ)
- Values with data stored in the record show as many bytes as they have: SAM's RID values have none (they showed 4 bytes), device property INT16 values 2
- Damaged and truncated hives are read as far as they can be instead of failing: missing cells, keys listed twice, lists pointing back at a parent key (which crashed with a stack overflow), impossible cell and bin sizes. Every hive in the test set now loads (9 failed before)
- Key lookups ignore case as Windows does
- VSMIDK and BCD-Template hives are recognized
- A hive whose header time stamp is not a valid FILETIME loads (its last written time is 1601-01-01); it failed with "Not a valid Win32 FileTime" (2026.10.1)

### 📚 ERZHelpers

*Raw reads of locked files and Volume Shadow Copies, used by AmcacheParser, AppCompatCacheParser, EvtxECmd, MFTECmd, PECmd, RECmd and rla, Registry Explorer, ShellBags Explorer and SBECmd, SQLECmd; they get these with their next release*

- --vss works again: listing and mounting Volume Shadow Copies failed with "Unable to load DLL 'WmiLight.Native.dll'" (EvtxECmd, MFTECmd, PECmd and RECmd 2026.5)
- Mounting VSCs only removes the links of an earlier mount from the mount folder; a folder with anything else in it is left alone with an error (it was deleted with everything in it)
- A locked file that can't be read (a bad path, a VSC that is gone, a damaged volume) is reported and the other files are still read; it stopped the whole list
- VSC creation times use the UTC offset WMI gives; a VSC on a volume that WMI doesn't list no longer stops the run, and volumes mounted in a folder no longer match a drive letter
- A VSC that fails to mount says why, and a failed mount can no longer be reported as mounted

### 📚 ExtensionBlocks

*The shell items and extension blocks used by LECmd, JLECmd and JumpList Explorer (through Lnk and JumpList), SBECmd and ShellBags Explorer, and the RECmd plugins with shell items (LastVisitedPidlMRU, OpenSavePidlMRU, RecentDocs, Taskband) in RECmd and Registry Explorer*

- A root folder item of type 0x2E with extension blocks after its GUID (a known folder such as Downloads, with 0x48 at offset 3) is read with its folder name and times; it failed with "Index and count must refer to a location within the buffer" (2026.10.8)
- Zip contents items with data in bytes 4-9 are read (they were unknown items of types such as 0x6C, 0x7F and 0x8A) (2026.10.8)
- Checked against every item of the ShellBags test corpus (68,598 shell items) and every shell item of the LNK and jump list test files (169,919, unchanged)

### ⌨️ AmcacheParser

- Windows 11 26H1/26H2 Amcache SQLite databases (Inventory\*.db next to Amcache.hve, which replace its Root\\Inventory\* keys) are read: programs, files, shortcuts, devices and drivers with the same logic and CSVs as the hive (\_Databases in their names, every database column included), and every other database (InventoryInstaller\*, InventoryMiscellaneous\*, Mare, DeviceCensus, ...) and their Metadata tables as CSVs of their own (FILETIMEs such as lastFullSync and LastScanTime shown as times)
- -f can be Amcache.hve, the folder it's in, or one of the databases; the console says which kinds of Amcache data were found
- Databases are read from temporary copies (with their -wal and -journal), so nothing is written next to the evidence; damaged databases are reported and the newest copy of a row is used
- Old format entries (Root\\File, Root\\Programs) in hives upgraded from Windows 8 are written to \_OldFormat CSVs (they were left out; Issues #203)
- Programs and drivers with a date that is not valid (Discord writes 53/16/2024; some MSI dates hold two dates) are kept with that date empty (they were left out with an error)
- New columns at the end of the new format CSVs: Publisher, Sha256, KeyName, AppxPackageFullName and AppxPackageRelativeId for files; KeyName, UserSid and MsiInstallDate for programs; target path, AUMID and program id for shortcuts; DeviceState, install dates, filters, ExtendedInfs, DeviceInterfaceClasses and LocationPaths for PnP devices; ClassGuid, DriverPackageStrongName, FlightIds, RecoveryIds and IsActive for driver packages
- Deleted keys are recovered from hives that are in use too; old format (.LOG) transaction logs are used, and logs are found whatever their case; a hive whose header checksum is wrong is treated as dirty when it has logs
- Uses Registry 2026.10.0 (see Registry above): a hive in the test set that would not open is read, and logs are replayed as Windows does
- .NET 10 only (the .NET Framework build is gone; SQLite needs a native library)
- The SQLite library it needs for the Windows 11 Inventory\*.db databases ships as files next to it: e\_sqlite3.dll (Windows x64), libe\_sqlite3.so (Linux x64) and libe\_sqlite3.dylib (macOS Apple silicon); `dotnet AmcacheParser.dll` works on Linux and macOS
- New --temp: the temporary copies of the Inventory\*.db databases go to a Temp folder next to AmcacheParser (or the folder given with --temp) instead of the user's %TEMP%; if it can't be written to, AmcacheParser stops and asks for --temp
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments, a missing input path, a hive or database it can't access or read, output or a --temp folder that can't be written, a dirty hive without its transaction logs); damaged hives and databases, and entries that could not be parsed, are reported and exit 0 (it was always 0)
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_AmcacheParser\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the --csv directory (Issues #259); --debug and --trace show the time of each message again (they showed the --dt date) (2026.10.1)

### ⌨️ AppCompatCacheParser

- Windows 10/11 Executed is read from the entry's data records (tag 0x100) instead of "the last 4 bytes are 1", which missed 157 and wrongly flagged 102 of 9,316 entries in test hives (#6)
- Windows XP: only the entries in use, most recently updated first (all 96 slots were written, including empty ones)
- New FromSlack column (last): entries recovered from the slack of the value's cell that are no longer in the cache (Windows 8 to 11)
- -t sorts by last modified time (it did nothing)
- Windows 7 and Vista/2008 values are read with the right 32/64 bit layout when the hive's architecture value is missing or wrong
- A damaged entry is reported and the rest of the cache is still read; Windows 10/11 entries with less than 4 bytes of data no longer stop parsing
- A control set without a cache no longer repeats the previous control set's entries; control sets above 9 are found
- Transaction logs of a hive that is in use are replayed (they were read as empty); log files are found whatever their case
- Values that aren't a cache (for example a cut off image) give a clear message instead of an exception
- No CSV with only a header is left when no cache is found
- Uses Registry 2026.10.0 (see Registry above): transaction logs replayed as Windows does, damaged hives that would not open (such as one in the test set) are read
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments or a -c ControlSet that doesn't exist, a missing input file, a hive it can't access or read, a dirty hive without its transaction logs, a CSV that can't be written); damaged hives and cache entries are reported and exit 0, as does a hive without a cache (it was always 0)
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_AppCompatCacheParser\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the --csv directory (Issues #259) (2026.10.1)

### ⌨️ bstrings

- ⚡ About 1.6x faster with about a third less memory (152 MB library: 1.50 s → 0.95 s, 1.27 GB → 0.81 GB peak); console output about 3x faster
- Strings that cross chunk boundaries are found whole instead of as fragments, so output no longer depends on -b
- Exact offsets for Unicode strings, and for --off with trimmed whitespace or multi-byte code pages
- Find UTF-16LE strings that start at odd byte offsets
- Regex filters match only the string, not the offset suffix (anchored patterns such as b64, sid, cc and zip failed with --off)
- Fix regex filters matching across spaces ("foo bar" matched "foobar"), and the url3986 and reg\_path patterns
- A string that matches several filters is written once
- Validate --cp code pages; honor -x when it equals -m
- -f and -d work when input is redirected (#17)
- Directory scans skip inaccessible folders, empty files and reparse points instead of stopping
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments, a missing input path, a file or a folder under -d it can't read, an -o file that can't be written); any file content gives 0 (it was always 0)
- Folders under -d that can't be read are reported ("Unable to read folder &lt;path&gt;: &lt;reason&gt;") and the rest is still processed; they were skipped silently
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_bstrings\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the folder of the -o file (Issues #259) (2026.10.1)

### ⌨️ EvtxECmd

- ⚡ About 2x faster overall (1,039 logs: 589 s → 300 s); large logs 2.4-4x faster (128 MB Security log: 16.1 s → 6.6 s), and maps load faster (a small log takes 0.24 s instead of 0.39 s)
- JSON time stamps are ISO 8601 UTC with a Z (2024-01-02T03:04:05.6780000Z, as JLECmd writes them); they ended in +00:00
- Forwarded events (ForwardedEvents and other Windows Event Forwarding logs) are read; they gave no records (#15)
- New event log parser, written to MS-EVEN6 and checked against the Rust evtx parser: about 331,000 more records read over 1,039 test logs (1,283,606 → 1,614,510), the same counts as that parser except for one chunk it drops
- New RenderingInfo column (last CSV column): the message and the level, task, opcode and keyword names that forwarded events carry from the source computer (PR #262)
- &amp;, &lt; and &gt; in values are no longer written as &amp;amp;, &amp;lt; and &amp;gt; in Payload, PayloadData and the other map columns
- Left-to-right marks in values (for example in dates) are kept instead of becoming ", "
- Character references, entity references, CDATA sections and processing instructions in events are supported (#208)
- Fix values: Real64 arrays, size\_t as hex, SIDs read by their subauthority count, and invalid timestamps shown as hex
- A damaged record or chunk is reported and skipped, and the rest of the log is still read; errors are listed under the right record number
- Fix the crash when two maps had the same event id and channel but different providers (#241)
- New maps for OpenSSH/Admin events 1 and 2 (#253), and System Kernel-Power 105 (power source changes, battery capacity)
- Map Values can be XPath expressions that calculate a value, e.g. a battery percentage with round(... div ... \* 100) (#99); a Value that isn't valid XPath is reported when maps load instead of failing every record that uses it
- A log that is in use and can't be read raw is skipped instead of ending the run
- -d skips reparse points
- --vss works again (VSCs failed to list with "Unable to load DLL 'WmiLight.Native.dll'"), and when it finishes only its own VSC links are removed from C:\\\_\_\_\_vssMount, then the folder if it is empty (it was deleted with everything in it)
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments, a missing input path, output that can't be written, event logs or folders it can't open or read); damaged event logs and files that aren't event logs are reported and exit 0 (it was always 0)
- An in-use event log whose raw read fails is reported and skipped; it stopped the run
- Folders under -d that can't be read are reported ("Unable to read folder &lt;path&gt;: &lt;reason&gt;") and the rest is still processed; they were skipped silently
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_EvtxECmd\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the output directory (the first of --csv, --json, --xml given) (Issues #259); --debug and --trace show the time of each message again (they showed the --dt date) (2026.10.2)

### 🖥️ EZViewer

- ⚡ PDF files are no longer kept open (locked) after another file is opened, and viewers of earlier files no longer stay in memory
- ⚡ Images are released when the next file is opened (each image's bitmap stayed in memory: ten 8000x6000 images used about 1.1 GB more, now about 0.1 GB)
- ⚡ Starts about 5x faster (3.5 s → 0.7 s) and uses about a third of the memory (484 → 149 MB): the program is one .NET single-file exe (with Extensions.yaml next to it) instead of being packed with Costura
- The hex view shows a multiple of 8 bytes per row (8, 16, 24, ...), never odd counts such as 15
- Now .NET 10 with DevExpress 26.1; sharp and correctly sized on each monitor
- Starts again: Extensions.yaml was invalid (a tab) and had no image list, so the program exited at start. A missing or invalid Extensions.yaml now gets a warning and the built-in lists
- Images display reliably (including animated GIFs and multi-page TIFFs); .ps1, .log, .yaml and .xml open as plain text; .tsv opens as a spreadsheet
- The "Open with EZViewer" context menu works when EZViewer is in a folder with spaces in its path, and adding or removing it no longer crashes
- The window opens where it was left on monitors left of or above the main one; a file that can't be read, or dropping something that isn't a file, no longer crashes
- Help \| Supported formats lists the extensions in use; the version no longer shows +&lt;commit id&gt;
- An empty EZViewer.settings file no longer stops the program at start (it failed with a null reference error)
- Images, and files shown in the hex view, are read off the UI thread: the window keeps responding while a big image loads
- The Hex view window is made when first opened and lets go of the file's bytes when closed (it kept a copy of the last file for as long as the program ran)
- The Hex view window shows a file again if it changed since it was last shown (it kept showing the old bytes)
- The Hex view window's saved size and place are no longer replaced by the defaults when it wasn't opened
- Error reports and logged exceptions include source file and line numbers

### 🖥️ Hasher

- ⚡ Faster: each file is read once for all the selected hashes (it was read once per algorithm); every other hash value is unchanged (checked on 468 files, including eMule chunk boundaries)
- ⚡ Hashing many small files is much faster and uses far less memory (100,000 small files: 28-34 s → 3-4 s; memory allocated 206 GB → 0.3-0.4 GB; peak working set 1.2-1.5 GB → 0.4 GB)
- ⚡ Starts about 5x faster (3.7 s → 0.8 s) and uses about a third of the memory (513 → 165 MB): the program is one .NET single-file exe (no .config or other files next to it) instead of being packed with Costura
- Whirlpool values are correct: the old code gave wrong values for about a third of files (151 of 465 test files; for example the standard "The quick brown fox jumps over the lazy dog" vector). Whirlpool hashes made with earlier versions may not match
- Now .NET 10 only (there is no .NET Framework build any more) with DevExpress 26.1; sharp and correctly sized on each monitor; the version no longer shows +&lt;commit id&gt;
- Cancel stops at once, also in the middle of a large file; results are added to the grid in batches (the window stays responsive with many files), a file that can't be read is reported with its error, and a dropped folder with a subfolder that can't be read is hashed except that subfolder (nothing in it was hashed)
- Hasher.ini is kept next to the program and used by both the main window and Options (they could read different files); the encrypted osTriage export works (it failed creating the zip); Help → Website opens <https://ericzimmerman.github.io/> (Forums opened a forum that no longer exists)
- File &gt; Save results &gt; to CSV: comma separated, with values quoted where needed (EricZimmerman/Issues#255)
- Fixed: the file being hashed when Cancel was clicked was added to the results as a row of empty hashes (and counted as hashed)
- The window stays responsive while hashing many small files (it did not respond for the whole run) and while a big folder is listed (it hung until the list was done); Cancel works while listing
- At most 200 errors are listed in the status history, then one line with the total (a folder of unreadable files listed every one)
- Saving results (text, CSV, Excel, osTriage, and the auto-save) shows a wait form, and the menus and buttons are disabled until it is done; text, CSV and osTriage files are written in the background, so the window keeps responding (Excel exports still pause the window while the grid is exported, behind the wait form). The file contents are unchanged
- A failed auto-save is also shown in a message box (it was only a line in the status history)
- Error reports and logged exceptions include source file and line numbers

### 📜 Get-ZimmermanTools

- ⚡ Much faster: up to 6 downloads at a time, one request per file (an unchanged file costs one small request instead of two), and faster unzipping; works in Windows PowerShell 5.1 and PowerShell 7
- Gets the .NET 10 builds: -NetVersion 10 is the default (9 still works and means 10), and they are saved to &lt;Dest&gt;\\net (a warning says when an old net9 folder can be deleted). Everything is downloaded again once
- A program that can't be reached no longer makes the script retry it forever
- -Sync runs even when everything is already current (it was skipped)
- Checks for a newer version of itself first: if there is one, it replaces the script and stops, saying to run it again (-NoUpdate skips the check)

### ⌨️ iisGeolocate

- ⚡ About 9x faster with a fifth of the memory (197 MB log: 11.8 s → 1.3 s, 619 MB → 112 MB); nothing is written next to the program any more (tmp.txt)
- The log CSVs have a header row (the field names from the log plus GeoCity and GeoCountry); 2026.5 wrote none
- Logs that crashed it are read: a log whose fields have no c-ip, rows before the first #Fields line, and a log that goes back to an earlier field list after a restart
- Values separated by more than one space and quoted values (IIS Advanced Logging, other W3C writers) stay in their columns; they shifted columns, and a user name could be looked up as the client IP
- IIS 6 logs no longer get an extra empty column (from the space at the end of their #Fields line)
- All private, shared (CGNAT), link local, multicast, documentation and reserved IPv4 and IPv6 ranges are skipped (only 127.0.0.1, ::1, 10.x and 192.168.x were); IPv4 mapped IPv6 addresses are looked up as IPv4
- Logs with the same name in different folders (W3SVC1, W3SVC2, ...) each get their own CSV; the second overwrote the first
- BadDataRows\_REVIEW\_ME.txt says which file and line each bad row is from and why (wrong number of values, no #Fields line before it); rows with the wrong number of values were written to the CSV with their values in the wrong columns
- !UniqueIPs.csv is replaced, not overwritten in place (a longer older file left its end behind)
- New --db option to use a MaxMind database from anywhere; Country and Enterprise databases work too
- Logs in use by IIS can be read; \*.LOG files are found on Linux; folders that can't be read are skipped
- CSVs always use commas (they used the system's list separator, such as ; on German systems)
- A summary at the end: rows, bad rows, and how many addresses were geolocated, not found, not public or not an IP address
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments, a missing folder, no usable database, output that can't be written, logs or folders it can't read); damaged logs are still listed under Failed files and exit 0 (it was always 0, even after a crash)
- Folders under -d that can't be read are reported ("Unable to read folder &lt;path&gt;: &lt;reason&gt;") and the rest is still processed; they were skipped silently (the .NET 10 build)
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_iisGeolocate\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the --csv directory (Issues #259) (2026.10.1)

### 🖥️ JumpList Explorer

- ⚡ Loaded jump lists use about a fifth less memory: the raw jump list file is no longer kept (619 MB to 491 MB with 107 MB of jump lists loaded)
- ⚡ Selecting a jump list is faster and allocates less: its rows (targets, times) are built once when it's loaded, off the UI thread, and its root was shown twice per selection (now once)
- ⚡ Starts about 4x faster (4.2 s → 1.1 s) and uses less than half the memory (478 → 200 MB): the program is one .NET single-file exe (nothing else next to it) instead of being packed with Costura
- Now .NET 10, with JumpList 2026.10.8 and Lnk 2026.10.9 (see JLECmd and LECmd above for what they read better)
- "Details as string" no longer swaps the target's modified and accessed times (Issues #239)
- No longer crashes on a LNK whose network share information is damaged (the tree was left half built), or on LNK files with named property sheets (the details window failed to open)
- Entries whose LNK is missing are listed (with the DestList path) instead of left out, and no longer show the previous entry's details
- Parse errors are shown in the properties; files that are empty or not jump lists are reported instead of skipped
- The console extra data shows the right popup attributes, buffer size and FE code page
- Custom jump list targets use the network or local path when there is no target ID list (and no longer have a double backslash); LNK numbers continue across categories
- Files load without freezing the window and no longer stay open (locked) after loading; the grid of LNKs keeps its column widths and sorting
- A damaged Settings.xml no longer leaves the program unusable; settings are kept next to the program; the version no longer shows +&lt;commit id&gt;
- Sharp and correctly sized on each monitor (Per-Monitor DPI awareness)
- Loading files that are already loaded is a single pass (was a search and a remove per file)
- The same file loaded with a different case (e.g. `C:\X.automaticDestinations-ms` and `c:\x.automaticdestinations-ms`) is no longer two rows; a file given twice is read once
- Progress while loading ("Loading N files: M read...", every 25 files)
- The load errors box shows the first 50 errors and "... and N more"; all of them are written to `JumpListExplorer_LoadErrors.log` next to the program (never to the temp folder: if the program's folder can't be written to, the box says the log could not be written). It listed every one and could be taller than the screen
- Property rows are disposed when the properties panel is cleared (they were only removed); the LNK details window fills its property store and Vista ID lists in one repaint
- Reads the same new shell items as JLECmd (Windows 11 archive folders, document drive items, 0x3D folders, OneDrive and Creative Cloud Files GUIDs named)
- Error reports and logged exceptions include source file and line numbers
- Uses JumpList 2026.10.9, Lnk 2026.10.10 and ExtensionBlocks 2026.10.8 (see ExtensionBlocks above) (2026.10.1)

### ⌨️ JLECmd

- ⚡ Uses about a fifth less memory on large sets of jump lists (JumpList 2026.10.6 no longer keeps each raw file while the results are written); the output is unchanged
- --appIds works again (AppIds loaded from the file were discarded)
- Unknown AppIds show "Unknown AppId" instead of a blank description; AppId lookups ignore case
- Custom destination categories after the first get their names (e.g. "Frequent") and types
- MacAddress and creation time are blank for droids that are not time based (they showed garbage values)
- Damaged or truncated jump lists keep every entry that can be read, instead of failing the whole file
- Jump lists with 4096 byte sectors (version 4) and large jump lists (about 24 MB and up) load
- Custom destination shortcuts no longer include the next entry's data
- Pinned destination count is filled in
- New --user option: adds the user name to the CSV (a User column) and JSON output (PR #8)
- JSON dates are ISO 8601 UTC (yyyy-MM-ddTHH:mm:ss.fffffffZ) instead of /Date(ms)/ (PR #7)
- All the LNK parsing fixes listed under LECmd (Unicode volume labels, long arguments, network paths, shell items)
- Uses Lnk 2026.10.4 and JumpList 2026.10.2: shortcuts damaged by a text mode copy are reported as such (see LECmd)
- TargetIDAbsolutePath has folders' real names where Windows also stores a localized name for them (This PC\\C:\\Users\\e\\Desktop\\..., not This PC\\C:\\@shell32.dll,-21813\\e\\@shell32.dll,-21769\\...; 900 of 28,087 rows over the test jump lists); the JSON also has the long names, 0xbeef000b blocks and the shell items in property values (JumpList 2026.10.3, Lnk 2026.10.5, ExtensionBlocks 2026.10.2)
- With --fd, jump lists with a named property (AutoList) are no longer dropped (777 rows over the test jump lists), and the long name of users files folder (CFSF) items is shown; LNKs whose last item is a CD burning item have its path and MFT entry (it was read as a control panel item with a garbled path)
- Uses JumpList 2026.10.8, Lnk 2026.10.9, ExtensionBlocks 2026.10.7 and GuidMapping 2026.10.2 (the same new shell items as LECmd, and 0x36 items say "Shell Type: File")
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments, a missing input path, output that can't be written, jump lists or folders under -d it can't read); damaged jump lists are reported and exit 0 (it was always 0)
- Folders under -d that can't be read are reported ("Unable to read folder &lt;path&gt;: &lt;reason&gt;") and the rest is still processed; they were skipped silently
- Uses JumpList 2026.10.9, Lnk 2026.10.10 and ExtensionBlocks 2026.10.8: 0x2E root folder items with extension blocks and zip contents with data in bytes 4-9 are read (see ExtensionBlocks above) (2026.10.3)
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_JLECmd\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the output directory (the first of --csv, --json, --html given) (Issues #259); --debug and --trace show the time of each message again (they showed the --dt date) (2026.10.4)

### ⌨️ LECmd

- ⚡ About 27% faster shortcut parsing with less memory
- JSON time stamps (also with --pretty) are ISO 8601 UTC with a Z (2024-01-02T03:04:05.6780000Z, as JLECmd writes them); they ended in +00:00
- New columns in CSV, JSON, XML and HTML: Name, IconLocation, ShowWindow, EnvironmentVariables, VolumeDroid, FileDroid, VolumeDroidBirth, FileDroidBirth, Anomalies
- Show damaged shell items, parse problems and anomalies (data after the end of the shortcut, out of range timestamps, whitespace padded arguments, undefined show commands)
- Fix crash on undefined ShowCommand values such as the BITTER APT shortcuts (#12); readable reasons for damaged shell items (#8)
- Name and Arguments are no longer cut at 260 characters (which also misaligned everything after them)
- Fix swapped target modified/accessed labels, Shim data blocks read as known folder blocks, only the first shell item of Vista+ ID lists being parsed
- Unicode volume labels, correct network share paths (Unicode offsets, device and provider names), no bogus Unicode local path
- Root folder items such as Web Folders and All Control Panel Items are parsed instead of reported as corrupt
- Console data blocks: all 16 colors, fill attributes, font details; header hot keys with combined modifiers
- Tracker MAC address and creation time only for time based droids
- One bad shortcut no longer aborts the whole CSV/JSON/HTML export, and a display error no longer marks a parsed file as failed
- XML file names include a hash of the source path (same-named shortcuts from different folders overwrote each other)
- Relative --html paths work (they discarded every export)
- Directory scans no longer treat folders and empty files as failed files, and don't follow reparse points
- JSON output replaces an existing file instead of leaving old data at its end
- Shortcuts damaged by a text mode copy (a CR byte added before every LF byte, as an ASCII mode FTP transfer does) are reported as such instead of only as corrupt (Issues #43)
- TargetIDAbsolutePath has folders' real names where Windows also stores a localized name for them: This PC\\C:\\Program Files\\Process Hacker 2\\... instead of This PC\\C:\\@shell32.dll,-21781\\Process Hacker 2\\... (327 of 2,494 test shortcuts). The details (--all) show the long names, 0xbeef000b blocks (they were unknown), the shell items in property values, and FAT/CDFS file system hints (Lnk 2026.10.5, ExtensionBlocks 2026.10.2)
- Uses Lnk 2026.10.9, ExtensionBlocks 2026.10.7 and GuidMapping 2026.10.2: folders inside archives opened with Windows 11's archive support, document drive items and 0x3D folders in target ID lists are read, and OneDrive and Creative Cloud Files folders are named whatever their account
- The text of a 0x36 target ID item says "Shell Type: File" (it said Directory), matching its name
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments, a missing input path, output that can't be written, lnk files or folders under -d it can't read); damaged lnk files are reported and exit 0 (it was always 0)
- Folders under -d that can't be read are reported ("Unable to read folder &lt;path&gt;: &lt;reason&gt;") and the rest is still processed; they were skipped silently
- Uses Lnk 2026.10.10 and ExtensionBlocks 2026.10.8: 0x2E root folder items with extension blocks and zip contents with data in bytes 4-9 are read (see ExtensionBlocks above) (2026.10.2)
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_LECmd\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the output directory (the first of --csv, --json, --xml, --html given) (Issues #259); --debug and --trace show the time of each message again (they showed the --dt date) (2026.10.3)

### 🖥️ MFT Explorer

- Large $MFTs load in seconds instead of minutes, with the window responsive (every record lookup searched all records; a 196,230 record $MFT was still loading after 5 minutes, now about 8 s)
- ⚡ Faster Expand All (30-43% on the large corpus $MFTs) and folder expanding: expand buttons come from a set built while loading
- ⚡ Recursive listings (right click) are about 25-45% faster on large $MFTs; their rows come in the order they are stored instead of folders first, then by name (click a column header to sort)
- Scrolling and column best fit are quicker on large listings (rows are no longer measured for wrapped text; names never wrap)
- ⚡ Starts about 3x faster (4.5 s → 1.5 s) and uses about half the memory (439 → 209 MB): the program is one .NET single-file exe (nothing else next to it) instead of being packed with Costura
- Overview shows the file name, entry and parent entry (with sequence numbers), parent path, the $STANDARD\_INFORMATION and $FILE\_NAME times and the record's attributes; Details (always empty before) has the whole record; the hex view has 16 bytes per row
- The folder tree is built as you expand it, and folder listings (also recursive ones, with right click) are built in the background
- The hex view shows the record's own bytes for $MFTs with 4 KB records and for entries past 4,194,304 (it read entry number x 1024)
- Right click lists a folder recursively again (a new tree menu had taken it); the keyboard changes the listing; damaged $MFTs and files that are not $MFTs no longer crash it
- A damaged record's parse errors are shown in its overview, and the $MFT's counts in the title; MFTExplorer.exe &lt;file&gt; opens an $MFT
- Now .NET 10, DevExpress 26.1 and MFT 2026.10.3 ($MFTs over 2 GB, 48 bit entry numbers), sharp on high DPI monitors (Per-Monitor V2); About shows the version
- The window can no longer be made smaller than the size it opens at (it could be shrunk to nothing; EricZimmerman/Issues#257)
- A hard linked file is listed once per name it has in a folder, with that name (it was listed with all its names in every folder it is in, e.g. 4 rows for 2 names in one folder; folder counts and recursive listings were too large: Win10 $MFT, recursive root 615,071 rows before, 241,913 now)
- After listing a folder, Overview, Details and the hex view show the folder (they showed the listing's first file, e.g. $Extend for the root)
- Right click (expand all and list recursively) shows the wait form from the start (the window froze with nothing shown while the tree expanded)
- Selecting another folder cancels a listing still being built (they all ran to the end at the same time); the wait form is no longer closed by an older listing while a newer one runs
- The $MFT stays open while it is loaded (it was opened again for every selected row)
- Error reports and logged exceptions include source file and line numbers

### ⌨️ MFTECmd

- ⚡ New --lowmem option: about 30% less peak memory for about 1.4x the run time
- ⚡ Large $MFTs load about 1.8x faster (2.1M records: 10.4 s → 5.7 s), and a loaded $MFT takes about 20% less memory (2 GB $MFT: 5.2 GB → 4.7 GB peak)
- JSON time stamps ($MFT, $J and $LogFile) are ISO 8601 UTC with a Z (2024-01-02T03:04:05.6780000Z, as JLECmd writes them); they ended in +00:00
- Add $LogFile support: -f $LogFile exports every log record to CSV and JSON, decoding FILE records, created and deleted attributes, file name index entries (name, parent, timestamps, size), directory entry updates and $STANDARD\_INFORMATION timestamp changes (time stomping) with their previous values
- $LogFile: the target file and attribute of every operation, including index buffers, $UsnJrnl:$J and bitmaps, from the log's checkpoints; transaction grouping (TransactionFirstLsn); with -m, target and parent paths
- $J with -m $MFT: parent paths are as they were when each record was written, by rewinding the journal (#56), so paths through deleted, reused, renamed or moved directories resolve instead of showing PathUnknown
- Add v3 $J records (128 bit file references) and skip v4 records correctly (both used to lose the rest of the page)
- $J records after zeros in the middle of a page are no longer skipped; invalid $J timestamps no longer stop processing
- New --cutoff option: only $MFT records created or modified on or after a date (PR #46 idea)
- New ParseErrors column at the end of the $MFT CSV with each damaged record's problems; parse problems are summarized as warnings (details with --debug)
- $MFTs over 2 GB are supported
- Resident attribute lists are read (thousands of records per $MFT had empty attribute lists)
- Deleted records keep their attributes past the used size (more $DATA runs and names recovered)
- The $MFT CSV only has the ResidentDataBase64, ResidentDataHex and ResidentDataASCII columns with --ir (#58); without it they were always there and always empty
- One damaged record no longer ends the whole $MFT; fixed hangs on damaged records and directories that are each other's parent
- Fix NTFS 3.0 (Windows 2000) record entry numbers; fix 48 bit entry numbers (upper bits were multiplied by 2^24)
- $SDS: one row per security id (every entry was output twice), with a FromMirror column for mirror copies that differ
- $SDS and $MFT security descriptors: correct SIDs (padding was read as extra subauthorities, e.g. S-1-5-32-545-2020897377-...), ACEs of 256+ bytes, object ACEs, any descriptor part order, NULL ACLs
- $I30 and $O: damaged pages and entries are skipped instead of ending the file; $O returns the object ids of every page (only the last page's were returned)
- $Boot: cluster sizes over 64 KB
- --de shows each index root entry's own file and its parent
- $J --csv and --json together write both (JSON was skipped)
- With -m, --csvf/--jsonf name only the -f output (the $MFT export overwrote it)
- JSON output replaces an existing file instead of leaving old data at its end
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments, a missing file, a file it can't read, output that can't be written); damaged, empty or unknown files are reported and exit 0 (it was always 0)
- Warnings about invalid timestamps say where they are: the attribute ($STANDARD\_INFORMATION, $FILE\_NAME, index entry), the FILE record (entry-sequence) and its offset, or the $I30 offset or $LogFile LSN, with the raw value (MFT #22); they said only "Enable --debug for record information" (2026.10.4)
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_MFTECmd\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the output directory (the first of --csv, --json, --body given) (Issues #259); --debug and --trace show the time of each message again (they showed the --dt date) (2026.10.5)

### ⌨️ PECmd

- JSON time stamps are ISO 8601 UTC (2024-01-02T03:04:05.6780000Z, as JLECmd writes them) with their full precision; they used the --dt format, to the second by default
- Runs on Linux and macOS: Windows 10/11 prefetch files are decompressed without Windows (--vss and --ads are disabled there with a warning)
- New --ads option: scan alternate data streams for prefetch data (PR #17)
- Executable full paths in the exports, validated against the prefetch hash (PR #19)
- Fix file references in version 23 and later files being shifted by one entry
- Fix version 30 file metrics all being copies of the first entry
- Fix 48 bit MFT entry numbers in file references (the upper bits were multiplied by 2^24); fix a crash on 30 character executable names
- Show parse problems when a file doesn't parse completely
- --vss works again (VSCs failed to list with "Unable to load DLL 'WmiLight.Native.dll'"), and when it finishes only its own VSC links are removed from C:\\\_\_\_vssMount, then the folder if it is empty (it was deleted with everything in it)
- Error reports and logged exceptions include source file and line numbers
- A prefetch file with no last run times (run count 0) shows "Last run: (none)"; without -q it was reported as an error ("Sequence contains no elements")
- A file that doesn't parse completely is listed once under Failed files (it was listed twice, and the summary could say "Processed -2 out of 2 files")
- Exit code 1 when the tool fails (invalid arguments, a missing input path, output that can't be written, files or folders it can't read, including folders under -d that can't be read, VSC folders that can't be fully listed and folders or streams that can't be opened during --ads); damaged files are reported and exit 0 (it was always 0)
- Folders under -d that can't be read are reported ("Unable to read folder &lt;path&gt;: &lt;reason&gt;") and the rest is still processed; they were skipped silently
- -d --dedupe with a file that can't be read (for example locked by another process) reports it and goes on; it stopped the run with an unhandled exception
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_PECmd\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the output directory (the first of --csv, --json, --html given) (Issues #259); --debug and --trace show the time of each message again (they showed the --dt date) (2026.10.3)

### ⌨️ RBCmd

- -d finds files in the .NET Framework build (it found none)
- Contents of deleted folders ($R directories) are listed (they almost never were)
- INFO2: Windows 9x records (280 bytes) are read; new Removed column for records Windows has cleared (file restored or Recycle Bin emptied)
- Damaged $I and INFO2 files are reported with what could be read, instead of crashing
- Console times are UTC like the CSV; out of range timestamps are reported
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments, a missing input path, output that can't be written, files or folders it can't read); damaged $I and INFO2 files are still listed under Failed files and exit 0 (it was always 0)
- A $I or INFO2 file that can't be read (for example locked by another process) is reported as "Unable to read" instead of asking for the file to be sent in
- Folders under -d that can't be read are reported ("Unable to read folder &lt;path&gt;: &lt;reason&gt;") and the rest is still processed; they were skipped silently (the .NET Framework build logs them)
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_RBCmd\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the --csv directory (Issues #259); --debug and --trace show the time of each message again (they showed the --dt date) (2026.10.1)

### ⌨️ RECmd and rla

*And the RECmd plugins, also used by Registry Explorer*

- JSON time stamps are ISO 8601 UTC with a Z (2024-01-02T03:04:05.6780000Z, as JLECmd writes them); they used the --dt format
- --recover false is honored (deleted keys and values were always recovered); hives in use read raw now recover them too
- --sync and batch file errors no longer end the process abruptly
- Keys with the same value name twice no longer stop batch and --vn output for that key
- Keys without a last write time no longer fail --kn and JSON output
- --kn details show "(Deleted: True)" instead of a broken message template
- The help shows the version and examples again
- Uses Registry 2026.10.0 (see Registry above): over the test hives, DFIRBatch finds about 9% more rows (41,589 → 45,213) (logs found and replayed in full, damaged hives read, more deleted keys)
- --vss works again (VSCs failed to list with "Unable to load DLL 'WmiLight.Native.dll'"), and when it finishes only its own VSC links are removed from C:\\\_\_\_vssMount, then the folder if it is empty (it was deleted with everything in it)
- Old format (.LOG) transaction logs are used; log files are found whatever their case (on Linux, logs named differently from the hive were missed); empty logs are skipped
- A hive whose header checksum is wrong is treated as dirty when it has logs (Windows takes the header from the log)
- In -d, files that only have a hive's name (a script called system, say) are skipped quietly; errors name the hive
- Error reports and logged exceptions include source file and line numbers
- **rla**: --nop false (the default) saves each hive under its original path in --out again (#112): C:\\Windows\\System32\\config\\SAM becomes &lt;out&gt;\\C\\Windows\\System32\\config\\SAM
- **rla**: Hives with the same name no longer overwrite each other with --nop true: the second is saved as SAM\_2 (#112)
- **rla**: --cn finds the profile for Users, Documents and Settings and ServiceProfiles paths, in KAPE output (\\triage\\C\\Users\\...) and with / separators
- **rla**: Hives in use are copied from the raw read (copying them failed); transaction logs that can't be applied are reported (the hive was skipped without a word)
- **RECmd plugins**: Use Registry 2026.10.0, GuidMapping 2026.10.2 (more ETW providers named) and AppCompatCache 2026.10.1
- **RECmd plugins**: AppCompatCache: Executed (Windows 10/11 tag 0x100) and FromSlack columns, entries recovered from the value's slack, and the parse errors of the cache
- **RECmd plugins**: AppCompatFlags: CIT entries are listed (the plugin failed to load DiscUtils and found nothing)
- **RECmd plugins**: User accounts (SAM): groups created on the computer are listed with the built in ones (Issues #80), membership is matched by the account's full SID (user 500 was also put in groups of 1500), and SAM hives with a damaged group list their accounts
- **RECmd plugins**: New 7-Zip copy history and folder history (by Phill Moore) and extraction path history plugins (Issues #170)
- **RECmd plugins**: ETW: providers without an Enabled or EnableProperty value are listed
- **RECmd plugins**: SCSI and USBSTOR: devices without a name property (Windows 7 and earlier) are listed
- **RECmd plugins**: IconLayouts no longer crashes on Desktop keys without the value; damaged values are reported
- **RECmd plugins**: A value name used twice in a key no longer stops Amcache, Services, Windows App and other plugins for that key
- **RECmd plugins**: OpenSavePidlMRU and LastVisitedPidlMRU show folders' long names where Windows also stores a localized name (This PC\\C:\\Program Files\\..., not PROGRA~1); Pinned Taskbar Items (Taskband) and RecentDocs fill in the LNK name, which was empty for those items (RegistryPluginBase 2026.10.1, ExtensionBlocks 2026.10.2)
- **RECmd plugins**: OpenSavePidlMRU, LastVisitedPidlMRU and Pinned Taskbar Items read shell items with the classes ShellBags Explorer and SBECmd use (with LECmd's fixes): paths that were !!!Unable to determine value!!! or empty have their names, an unsupported item no longer repeats the folder before it in the path, MRU values with an item that can't be read are listed with that item as an error (they were left out), and more item types are read (20 Unsupported ShellID errors over the test hives are gone)
- **RECmd plugins**: OpenSavePidlMRU and LastVisitedPidlMRU read 0x3A file entries (a value that was lost is listed, garbled names are names) and name zip contents and network items as LECmd does
- **RECmd plugins**: DsrmAdminLogonBehaviour finds the value: its key path is ControlSet00\*\\Control\\Lsa (it was System\\CurrentControlSet\\Control\\Lsa, which no hive has) and its value DsrmAdminLogonBehavior (it looked for DsrmAdminLogonBehaviour); the British spelling is still accepted
- **RECmd plugins**: Use RegistryPluginBase 2026.10.4 (ExtensionBlocks 2026.10.7, GuidMapping 2026.10.2): the plugins with shell items (LastVisitedPidlMRU, OpenSavePidlMRU, Taskband) read folders inside archives (Windows 11), document drive items and 0x3D folders; OneDrive and Creative Cloud Files GUIDs are named
- **RECmd plugins**: Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments or batch file, a missing input path, a hive or folder it can't read, including subfolders under -d, a dirty hive without its transaction logs when --nl isn't given, output that can't be written, a plugin that throws or can't be loaded in batch mode); damaged hives are reported and exit 0 (it was always 0)
- **rla**: Exit code 1 when the tool fails (invalid arguments, a missing input path, a hive or folder it can't read, including subfolders under -d, a dirty hive without its transaction logs with --ca false, a hive that can't be saved); damaged hives are reported and exit 0 (it was always 0)
- Folders under -d that can't be read are reported ("Unable to read folder &lt;path&gt;: &lt;reason&gt;") and the rest is still processed; they were skipped silently
- **RECmd**: uses RegistryPluginBase 2026.10.6 and ExtensionBlocks 2026.10.8: the LastVisitedPidlMRU, OpenSavePidlMRU, RecentDocs and Taskband plugins read 0x2E root folder items with extension blocks and zip contents with data in bytes 4-9 (see ExtensionBlocks above) (2026.10.1)
- **RECmd**: New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_RECmd\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the output directory (the first of --csv, --json given, else the folder of the --saveTo file) (Issues #259); --debug and --trace show the time of each message again (they showed the --dt date) (2026.10.2)
- **rla**: New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_rla\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the --out directory (Issues #259) (2026.10.1)

### ⌨️ RecentFileCacheParser

- JSON time stamps are ISO 8601 UTC (2024-01-02T03:04:05.6780000Z, as JLECmd writes them) instead of \\/Date(milliseconds)\\/
- Checks the file's checksum and shows whether it is valid
- Damaged entries are reported and the readable ones kept; empty files no longer crash
- Show parse problems; fix the --pretty example in the help
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (a missing -f or input file, a file it can't read, a CSV or JSON export that can't be written); a damaged file, or a file that isn't a RecentFileCache.bcf, is reported and exits 0 (it was always 0)
- A file that can't be read (for example locked by another process) is reported as "Unable to read" instead of asking for the file to be sent in
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_RecentFileCacheParser\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the output directory (the first of --csv, --json given) (Issues #259) (2026.10.2)

### 🖥️ Registry Explorer

- ⚡ Large hives load faster and use less memory: the key tree is made as keys are expanded (the find panel, filters, incremental search and exports still search whole hives, after making the rest of the tree the first time), hives are read off the UI thread with a limit on how many load at once, and value data, slack and messages are shown without building every row's text up front
- ⚡ No longer slows down or grows in memory as keys are clicked: menus, tabs, hex viewers (which kept each byte as a separate object) and fonts were created and never released, and removed hives stayed in memory
- ⚡ Starts about 3x faster (4.9 s → 1.7 s) and uses about a third less memory (432 → 269 MB): the program is one .NET single-file exe (with only Plugins and Bookmarks next to it, no .config) instead of being packed with Costura
- ⚡ Hex views get that control's fixes: Shift+Up selected two lines instead of one, and GDI objects and memory were leaked on every repaint
- Now .NET 10, with Registry 2026.10.0 (see Registry above: transaction logs applied as Windows applies them, damaged hives read as far as they can be, more deleted keys and values), the current plugins and the shared shell item code (ExtensionBlocks)
- Hiding and unhiding keys works again (it saved the key's offset instead of its path)
- Clicking a message with no hive no longer ends with an unhandled exception; a value name that can't be read in a damaged hive no longer does either (Issues #246)
- Plugins show for values of deleted keys; selecting some deleted keys found nothing
- Transaction logs (.LOG1, .LOG2) opened along with their hive are skipped with a message instead of failing to load as hives
- Messages name the hive they are about
- Find: Cancel stops the whole search (it stopped only the last hive's), and results are added per hive instead of per hit
- Projects remember hives loaded dirty without replaying their logs and open them without asking again (Issues #84); hives can have a nickname, shown in the tree and saved in the project (Issues #44); older projects still open, and a project given on the command line opens as one
- MSIX (packaged) app hives are recognized: Helium\\User.dat (the app's HKCU), Helium\\UserClasses.dat (its HKCU\\Software\\Classes) and WindowsApps Registry.dat (the package's HKLM\\SOFTWARE) are labeled with their app, and bookmarks and .reg exports treat them as NTUSER, UsrClass and SOFTWARE (Issues #238)
- The version no longer shows +&lt;commit id&gt;
- Scales correctly on high resolution monitors and when moved between monitors (everything could become very small)
- Its plugins and shell items read the same new shell items as RECmd's plugins (ExtensionBlocks 2026.10.7, GuidMapping 2026.10.2, RegistryPluginBase 2026.10.4)
- The hex views (value hex viewer and Technical details) use the same hex control as the other GUIs (EZ.Hexbox): bytes per line follow the window width in steps of 8 (8, 16, 24...) instead of any count (13, 23...), so the line offsets are round (0x...0/0x...8)
- Error reports and logged exceptions include source file and line numbers, also in plugins
- Uses RegistryPluginBase 2026.10.6 and ExtensionBlocks 2026.10.8: the shell items in the LastVisitedPidlMRU, OpenSavePidlMRU, RecentDocs and Taskband plugins (see ExtensionBlocks above) (2026.10.1)

### ⌨️ SBECmd

- ⚡ Uses Registry 2026.10.0 (see Registry above), ExtensionBlocks 2026.10.7 and GuidMapping 2026.10.2; 1.3 to 3.2 times faster over the test hives
- ⚡ Loading a hive is about 15-20% faster (each bag's text built once instead of three times, one compiled pattern for extension block signatures)
- Folders and files with a Unicode name (accented names such as Modèles; shell item types 0x35, 0x36, 0x39, 0xB1, 0xB5) have their LastWriteTime, FirstInteracted/LastInteracted, NodeSlot and child bag count (they were empty or 0; Issues #237)
- CSV names have the user's name for hives under Users or Documents and Settings on Linux and macOS too (Issues #233)
- Bags that are recovered deleted copies no longer drop the rest of their key: in one test hive 127 more bags (a whole This PC branch) are listed
- Damaged shell items no longer hang SBECmd (negative sizes in file, CD burning, CFSF and other items looped forever) or end it (an assertion on extra data in some items ended the process); in a test with 154,200 damaged items none hang or crash
- A damaged extension block, property sheet or time no longer makes the whole shell item a parse error: the item is listed with what could be read (0x32 files whose size starts with 0x10, such as 250k.zip, were also taken for zip contents)
- Keys without MRUListEx, or with a slot listed twice, and bad NodeSlot values no longer lose their bags
- Old format (.LOG) transaction logs are used, and logs are found whatever their case; a hive whose header checksum is wrong is treated as dirty when it has logs
- --json writes a JSON file per hive (one object per line, the CSV's columns plus SourceFile, times in ISO 8601 UTC as in JLECmd), also with --csv, --jsonf, --dedupe and -l; it failed every hive when used alone and wrote nothing when used with --csv (Issues #249)
- A hive in use is read raw only on Windows, and files that only have a hive's name are skipped quietly
- Network locations without a description no longer show the first letter of their name as one (Tout le réseau (T) is Tout le réseau), and a shell item that can't be parsed is reported with the parser's reason
- Zip contents items are named by their name (the folder in the zip file was appended: ...\\GWD\_v2.8.zip\\GWD\_v2.8\\images/GWD\_v2.8/ is now ...\\GWD\_v2.8.zip\\GWD\_v2.8\\images), and network items by their location (Microsoft Windows Network, not Microsoft Windows Network (Microsoft Network)), as LECmd and JLECmd show them; the folder and the provider are in the details
- OneDrive and Creative Cloud Files folders are named whatever their account (GuidMapping 2026.10.2 maps their GUID families by prefix: 653 GUIDs that showed as Unmapped GUID), as are WPS Cloud and Tencent Weiyun
- Folders inside archives opened with Windows 11's archive support (zip, 7z, rar, tar...) are read whatever their bytes 2-3 (they were unknown types, or misread: some as an unmapped GUID made of the folder's name): the name, the path in the archive and the modified time
- Folders and files of a document drive namespace extension (types 0xFF and 0xFC with 0x55 at offset 3, Korean folder names, roots such as TB\_ROOT): the name, three times, and the folder's ID and parent ID (they were unknown types)
- Shell item type 0x3D (a folder like 0x31, with a code page short name) is read (it was an unknown type)
- The ShellType column says File for 0x36 items (files with a Unicode name, zip files with accented names for example); it said Directory
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments, a missing -d folder, a hive or folder it can't read, including subfolders under -d, a dirty hive without its transaction logs when --nl isn't given, output that can't be written); damaged hives are reported and exit 0, and finding nothing to process exits 0 (it was always 0)
- A hive in use that is read raw and has no BagMRU key is a warning, as when it is read normally; it was counted as a hive that could not be processed
- Folders under -d that can't be read are reported ("Unable to read folder &lt;path&gt;: &lt;reason&gt;") and the rest is still processed; they were skipped silently
- A --csv or --json that names an existing file stops with a message; it crashed with an unhandled exception
- 0x2E root folder items with extension blocks (a known folder such as Downloads) are read; they were parse errors ("Index and count must refer to a location within the buffer"). Zip contents with data in bytes 4-9 are read (they were unknown types). ExtensionBlocks 2026.10.8, see above (2026.10.1)
- The console messages are saved to &lt;yyyyMMddHHmmss&gt;\_SBECmd\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the --csv directory, else the --json one, a new file per run, instead of being added to !SBECmd\_Messages.txt; --co is on by default, --co false turns it off (Issues #259) (2026.10.2)

### 🖥️ SDB Explorer

- ⚡ The "please wait" window no longer stays open after some clicks or errors; the hex view no longer adds a new viewer (and memory) on every click
- ⚡ Large databases (sysmain.sdb has about 330,000 entries) open faster: tree nodes are made as they are expanded (filters and find still search everything), and the details of a top node stop at 10,000 lines (it built the whole subtree, about 330,000 lines, on each click)
- ⚡ Building the tree and the details text allocates less memory (the tag name and its display rules are worked out once per tag)
- ⚡ Starts about 4.5x faster (3.6 s → 0.8 s) and uses about a third of the memory (490 → 166 MB): the program is one .NET single-file exe (nothing else next to it) instead of being packed with Costura
- The hex view shows a multiple of 8 bytes per row (8, 16, 24, ...), never odd counts such as 15
- Now .NET 10 with DevExpress 26.1 and SDB 2026.10.0 (damaged databases load as far as they can be read, and their problems are shown); sharp and correctly sized on each monitor
- A damaged Settings.xml no longer makes closing crash; settings are kept next to the program
- Damaged or crafted databases no longer crash it (large tag ids and checksums, invalid times, lists with two names)
- The database is read without freezing the window; a file can be given on the command line; dumping strings to a folder that can't be written reports an error; the version no longer shows +&lt;commit id&gt;
- Opening a second database while one is still loading (Open, Ctrl+O, or a file on the command line) is no longer possible: two files could be read at once and mix up the string table and metrics. Open is disabled until the load ends
- The "please wait" form no longer flashes on every node selected; it is shown only for nodes with large subtrees
- Error reports and logged exceptions include source file and line numbers

### 🖥️ ShellBags Explorer

- ⚡ Faster on large hives: folders fill the grid in one update (a right click filled it twice), messages are added in batches (each one measured every column over every row), and details are shown once per click
- ⚡ Loading a hive is about 15-20% faster (same parser change as SBECmd)
- ⚡ Loading a hive no longer keeps the previous hive in memory (the last right-clicked node and the Details window's bag held it)
- ⚡ Starts more than twice as fast (4.5 s → 1.9 s) and uses about half the memory (535 → 254 MB): the program is one .NET single-file exe (nothing else next to it) instead of being packed with Costura
- The hex view shows a multiple of 8 bytes per row (8, 16, 24, ...), never odd counts such as 15
- Built on the new ShellBags library (see SBECmd above): Unicode name items with their times, damaged items listed with what could be read, no hangs on damaged items, more bags from deleted copies, old format logs (.LOG), and Registry 2026.10.0 (see Registry above)
- Now .NET 10
- Property sheets in the details name their properties (Guid: ..., Key: 10 ==&gt; Item Name Display, Value: ...), and paths in 0xbeef000b extension blocks keep their drive letter (the first character was cut off); the shell item code is now in the ExtensionBlocks package, shared with LECmd, JLECmd and the RECmd plugins
- The SBECmd fixes above; shell items found inside other items' property values have values where they were empty (0x00 property views), and no longer show Has been explored and # Child Bags
- The zip contents and network names above
- Hives load without the window freezing ("Not responding"), and a hive dropped during a load no longer starts a second one inside it
- Holding an arrow key in the tree no longer mixes the rows of two folders in the grid
- The "show parent nodes on filter" option no longer flips on every start; a damaged layout file no longer stops it from starting
- Exports: TSV is tab separated (it had commas), CSV export no longer replaces what the grid shows, and JSON export no longer writes each row's icon
- The hex view (HexEditorControl 2026.10.0) no longer leaks graphics resources while painting and no longer throws when closed; the data interpreter uses the date format from Options, keeps its position and always on top setting, and no longer clears the clipboard on arrow keys
- Fewer duplicate error reports (one per kind of problem per hive); the log is written next to the program; the version no longer shows +&lt;commit id&gt;
- Sharp and correctly sized on each monitor (Per-Monitor DPI awareness)
- A click in the tree builds the details once (they were built 2-3 times) and fills the grid once (it was filled twice on the first left click after a right click, after a load, and when a grid row's right click went to its folder)
- Messages with raw bytes keep the bytes and show them in hex when displayed (a hex string was kept per message); messages are queued straight from the loading thread
- The first display of a bag with MRU position 0 now shows "Last interacted" (it appeared only from the second display)
- Selecting the hive's root clears the Details window (it showed the first top-level bag)
- Collapse no longer shows the nodes a tree filter hid
- The filter status shows "0 of 0 rows visible" for a folder with no bags, and the parse summary of a hive with no bags shows 100% processed (both showed NaN)
- Export errors (CSV/TSV, Excel, JSON, messages) are reported in a message instead of an unhandled exception; JSON export no longer leaves the wait cursor on an error
- Options: the date/time format preview no longer throws while a format is typed, and Save refuses a format that can't be used (it broke every time shown, the summary and the exports); a bad format saved earlier falls back to the default
- Options: "Show hex in string" is applied on Save only (it applied immediately, even on Cancel)
- Names and paths containing &lt; or &amp; show correctly in the summary; HTML in message boxes is limited to the parse summary
- The Messages window is cleared when a hive starts loading, so it shows the messages of the hive shown (they piled up over every load until Clear was used)
- The Messages window opens at most once per load (closing it during a load no longer makes it reopen at the next message)
- Reads the same new shell items as SBECmd (Windows 11 archive folders, document drive items, 0x3D folders, OneDrive and Creative Cloud Files GUIDs named)
- The Details window shows "Last interacted" for wrapper items (0x35, 0x36, 0x39, 0xB1, 0xB5) at MRU position 0, as the grid column already did; 0x36 items show Shell Type File in the grid and the details
- Error reports and logged exceptions include source file and line numbers
- Same shell item fixes as SBECmd: 0x2E root folder items with extension blocks and zip contents with data in bytes 4-9 (ExtensionBlocks 2026.10.8) (2026.10.1)

### ⌨️ SQLECmd

- ⚡ One connection per database instead of one per map (faster)
- JSON date and time values are ISO 8601 UTC (2024-01-02T03:04:05.6780000Z, as JLECmd writes them) instead of \\/Date(milliseconds)\\/, which was also shifted by the time zone of the machine running SQLECmd
- New --ifn option: adds the database's file name to its CSV and blob file names (as in \_places.sqlite.csv), so databases processed with the same map can be told apart (#54)
- DATETIME values stored with a time zone (such as 2024-01-02T03:04:05Z) are no longer converted to the local time of the machine running SQLECmd in CSV and JSON output; databases whose path contains a ; open
- No longer writes SQLite.Interop.dll into the working directory (it couldn't delete it while loaded); the SQLite natives for Windows x64/x86, Linux and macOS are in the runtimes folder next to it and loaded from there. A SQLite.Interop.dll / libSQLite.Interop.so left by an older SQLECmd in a folder it was run from can be deleted
- A map whose IdentifyQuery fails no longer skips the remaining maps (or crashes with -f); unreadable databases are reported as such instead of "No maps found"
- Hunt mode no longer warns about every non-matching map and non-SQLite file (only with --debug)
- --dedupe and --sync close their files; unreadable unmatched databases no longer crash the summary
- --sync verifies TLS certificates (PR #101) and reports HTTP errors and a missing Maps folder
- Maps without queries fail validation instead of failing at run time; databases in use can be detected
- The evidence is no longer changed: databases were opened in place, which wrote the -wal into the database and deleted the -wal and -shm files. Each is now copied, with its -wal and -journal, to a Temp folder next to SQLECmd (or the folder given with the new --temp) and only the copy is read; if that folder can't be written to, SQLECmd stops and asks for --temp
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments or maps, a missing input path, output that can't be written, files or folders it can't read); databases that are damaged or not SQLite, and map queries that fail on a database, are reported and exit 0 (it was always 0)
- Files that can't be opened (access denied, locked) and folders under -d that can't be read are reported ("Unable to read folder &lt;path&gt;: &lt;reason&gt;", once per folder, then "Could not access all files"); they were skipped silently
- A --json file that can't be created stops with a message; it crashed with an unhandled exception
- A -d folder that doesn't exist exits 1 with a message; it crashed with an unhandled exception
- -d no longer logs "&lt;folder&gt; not found!" for every subfolder
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_SQLECmd\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the output directory (the first of --csv, --json given) (Issues #259); --debug and --trace show the time of each message again (they showed the --dt date) (2026.10.2)

### ⌨️ SrumECmd

- SRUDB.dat is read from a copy in a temporary folder: nothing next to it changes (it wrote log and checkpoint files there and replayed the logs into the database in place). It can be read-only
- Dirty databases are handled automatically: the transaction logs are replayed into the copy, and a copy that is still dirty is repaired with esentutl /p (and SrumECmd says so). Repairing by hand is no longer needed; 4 of the 9 DFIR Artifact Museum samples failed with "Access denied" and exported nothing
- A bad value, an app or user id missing from the id map, or an invalid time no longer loses a whole table, and a damaged id map entry or SOFTWARE hive profile no longer loses everything; the problems are listed at the end
- New CSVs for the Server 2022 tables, when present: ProcessorTime, SizeInBytes, TotalUsed (volume size and space used) and NetworkBytes, with what they measure (Total, a volume, a network adapter) in ExeInfo (Srum#15)
- AppTimelineProvider has the seconds in focus, in the foreground, with user, keyboard and mouse input, with the display required and with audio in and out, as new columns at the end (Srum#9)
- --dt also applies to the Timestamp columns; now .NET 10
- New --temp: the temporary copy of SRUDB.dat and its transaction logs goes to a Temp folder next to SrumECmd (or the folder given with --temp) instead of the user's %TEMP%; if it can't be written to, SrumECmd stops and asks for --temp
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments, a missing input path, output that can't be written, a database, a -r SOFTWARE hive or folders it can't read); a damaged database or SOFTWARE hive is reported and exits 0, and a -d folder without SRUDB.dat exits 0 (it was always 0)
- Folders under -d that can't be read are reported ("Unable to read folder &lt;path&gt;: &lt;reason&gt;") and the rest is still processed; they were skipped silently (the .NET 10 build)
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_SrumECmd\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the --csv directory (Issues #259) (2026.10.1)

### ⌨️ SumECmd

- The databases are read from copies in a temporary folder: nothing in the -d folder changes (it created edb.chk, edb.log and other files there). It can be read-only
- Dirty databases (as Current.mdb usually is) are handled automatically: the transaction logs are replayed into the copies, and a copy that is still dirty is repaired with esentutl /p (and SumECmd says so). Repairing by hand is no longer needed
- Current.mdb's day counts are dated in the year of its data; they were dated in the year of the computer running SumECmd (a 2020 server processed in 2026 got 2026 dates, and day 366 the next year)
- Day counts over 32,767 and OS build numbers over 32,767 were negative
- One database that can't be opened no longer loses the others (even the SystemIdentity.mdb data was not exported), a bad value no longer loses the rest of its table, and yearly databases not listed in SystemIdentity.mdb are read too; the problems are listed at the end
- ⚡ SystemIdentInfo has the host and domain name, manufacturer, model, serial numbers, last boot, time zone, processors and memory, as new columns at the end
- New --temp: the temporary copies of the UAL databases and their transaction logs go to a Temp folder next to SumECmd (or the folder given with --temp) instead of the user's %TEMP%; if it can't be written to, SumECmd stops and asks for --temp
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments, a missing -d folder, output that can't be written, a database it can't read); databases that are damaged or missing are reported and exit 0 (it was always 0)
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_SumECmd\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the --csv directory (Issues #259) (2026.10.1)

### 🖥️ TimeApp

- New timestamp converter tab: a number or hex bytes as Unix seconds/milliseconds/microseconds, Windows FILETIME, WebKit/Chrome, Mac (Cocoa) or OLE time, and the current time in each; double-click a time to copy it
- The stopwatch no longer jumps when the clock changes, goes on after Stop and Start, and resets while running; the countdown no longer falls behind, shows in the title, and beeps and flashes at zero
- Public IPs over HTTPS; now .NET Framework 4.8 (was 4.6.2), and also a .NET 10 build (sharp on high DPI monitors)
- The timestamp converter's current times update only while its tab is shown and the window is active (EricZimmerman/Issues#256)
- Unchecking "Get public IP" while the addresses were being looked up no longer fills them in (or shows an error) afterwards; checking it on and off quickly shows only the last lookup's result.
- The stopwatch's display isn't redrawn while another tab is shown or the window is minimized; it's up to date as soon as its tab is shown again.
- The .NET 10 version is one exe (only TimeApp.exe, no .config) instead of TimeApp.exe, TimeApp.dll and TimeApp.runtimeconfig.json
- Error reports and logged exceptions include source file and line numbers

### 🖥️ Timeline Explorer

- ⚡ Large CSVs load faster (tagged lines, list updates and debug messages were per row costs)
- ⚡ Closed tabs free their memory (each closed file stayed in memory until the program ended); faster with debug messages on, on super timelines and large files (column sizing), and in Find
- ⚡ Files use about half the memory once loaded (one string for a value repeated in many rows): MFTECmd $MFT 145 → 76 MB, $LogFile 358 → 188 MB, $J 102 → 29 MB, a 1.5M-row plaso L2T CSV 1726 → 786 MB
- ⚡ Generic CSVs load with half the allocations (rows go straight into the grid's row objects)
- ⚡ Plaso (L2T/psort) and mactime timelines load faster (no exception per row without a time, no lower-case copies of the description per row)
- ⚡ Sorting mactime's Meta column is faster (no exception per comparison)
- ⚡ Find is many times faster on big files (8-15x; a 1.5M-row super timeline with 21 terms: about 2.5 minutes → 10 seconds)
- ⚡ Starts about 4x faster (3.9 s → 1.0 s) and uses less than half the memory (556 → 249 MB): the program is one .NET single-file exe (with Plugins and Layouts next to it) instead of being packed with Costura
- The CSVs of the 2026.10 tools load with their plugins again (typed columns, layouts) instead of as generic CSVs: AmcacheParser, AppCompatCacheParser, EvtxECmd, LECmd, JLECmd (also with --user), MFTECmd ($MFT, $SDS), PECmd (and its timeline), RBCmd, SrumECmd (AppTimelineProvider) and WxTCmd; CSVs from older versions still load
- New plugins and layouts for MFTECmd's $LogFile CSV, SrumECmd's Server 2022 CSVs (ProcessorTime, SizeInBytes, TotalUsed, NetworkBytes) and SumECmd's VmInfo; SumECmd's SystemIdentInfo shows its new columns
- AmcacheParser's database table CSVs (Inventory\*, Mare\*, Metadata, DeviceCensus from Windows 11 26H2) open with their own plugins and layouts, and the old format's associated files and programs CSVs load with their plugins (they opened as generic CSVs: their columns were in another order than the plugins listed)
- A file with an empty value in a number or date column no longer fails to load (for example some Amcache DriverPackages and DeviceContainers CSVs), and header lines in another case load
- Other CSVs: the delimiter (comma, tab, pipe or semicolon) is detected from the header (contributed by JYYYeung); the Hayabusa and BrowsingHistoryView layouts are used (they were named so they were never found); the internal Microsoft MFT tool's tab separated files load (they never did)
- RECmd's per plugin CSVs (UserAssist, RecentDocs, ShellBags, ...) open with their own plugins and layouts instead of as generic CSVs
- New plugins and layouts for MFTECmd's $I30 CSV, RecentFileCacheParser's CSV, iisGeolocate's CSVs (the IIS 6 and 7-10 default fields and every standard field, with typed status, port and size columns and a Timestamp from date and time; other field lists still open as generic CSVs) and SQLECmd's ActivitiesCache CSVs (Activity, Activity\_PackageId, ActivityOperation); they opened as generic CSVs
- BrowsingHistoryView: Visit Time is shown as written in the file (local time of the machine it ran on, or UTC with /ShowTimeInGMT 1); it was converted again with the analyst's time zone (Issues #241)
- Files load in the background: the window stays responsive while a large file loads, and files opened or dropped meanwhile load after it
- Details pane (View \| Details pane): the selected row's fields and values, one per line, beside the grid (Issues #248)
- MFTECmd's $MFT CSV without --ir (2026.10.3 and later, no resident data columns) opens with its plugin and layout
- Copy (Ctrl+C) and Apply value as filter (Ctrl+Shift+F) work in the details pane too: copy a value or several lines from it, or filter the grid on a field's value (Issues #250)
- Global format rules (Tools \| Global format rules): conditional formatting for every file, for example tagged rows in yellow, applied to each file that has the rule's columns (Issues #144)
- Fixes: a default layout that failed to load was deleted; startup without a Plugins folder; search options showed each other's choice; tagging with the checkbox could record another row; the wrong tab selected after closing one; export dialogs showed no files; a folder on the command line; empty plaso descriptions; Find with an empty line; Open last closed keeps tags
- Now .NET 10 and DevExpress 26.1, sharp on high DPI monitors (Per-Monitor V2); the default layouts are the current ones
- The PECmd, RBCmd, JLECmd and SBECmd plugins are named as the tools are (they were PeCmd, RbCmd, JleCmd and SbeCmd); a first pinned column chosen for their files has to be chosen again
- Details pane: a long value (such as an EvtxECmd Payload) can be read to its end: click it to open it with its own scroll bar (the pane scrolled a whole value at a time; EricZimmerman/Issues#253)
- Plugins no longer build a debug message for every row when debug messages are off
- A generic CSV row starting with #TYPE is no longer dropped (only the first line is checked for PowerShell's #TYPE line)
- Tagging rows redraws only those rows (the whole view was re-sorted and re-filtered after each tag), unless Tag is sorted, grouped or filtered on
- Tagging in an Excel file says it isn't possible (it did nothing)
- Find (Tools \| Find) searches the open files in the background; the window no longer freezes
- Find names the Excel files it can't search (they were left out silently)
- Find's wait form no longer stays open after an error or a double-click outside a result row
- The details pane keeps up when an arrow key is held (it was rebuilt for every row) and always ends on the focused row
- The super timeline details window is only filled while it is shown (it was filled for every focused row, hidden or not)
- Opening a session while a file is loading no longer opens that file in the new session
- Closing a tab clears the super timeline details window of its row
- Opening a file with a layout no longer measures every column that the layout then resizes
- The cell contents window no longer leaks a font per font size change
- With debug messages on, the messages window stays fast at its size limit (it copied all 2M characters four times a second)
- A plaso L2T CSV whose header has "macb" in lower case loads (it failed on its first row); mactime, KAPE mini timeline, ForensicTimeline, psort, Sigcheck, Autoruns, Shimcache and PowerShell history CSVs also match their columns ignoring case
- Find matches text by character, ignoring case (ordinal). Upper/lower case of accented, German, Cyrillic and Greek letters still match; a letter + combining accent no longer matches the precomposed letter (e + U+0301 vs é), ligatures and compatibility signs no longer match their letters (U+FB01 'ﬁ' vs "fi", Kelvin sign vs K), and zero-width characters, soft hyphens, BOMs and NULs in the text are no longer skipped
- Error reports and logged exceptions include source file and line numbers, also in plugins

### ⌨️ VSCMount

- Only the vss\* links of an earlier run are removed from the mount folder (&lt;mp&gt;\_&lt;drive letter&gt;); a folder with anything else in it is left alone with an error. It was deleted with everything in it
- --help works without administrator rights
- Exit code 1 for invalid arguments, a drive that isn't ready, missing administrator rights, a mount folder that can't be used or a VSC that fails to mount (it was always 0)
- A VSC on a volume that WMI doesn't list is skipped with a warning instead of stopping the run; volumes mounted in a folder no longer match a drive letter
- VSC creation times use the UTC offset WMI gives instead of the local time zone
- A VSC that fails to mount says why, and a failed mount can no longer be reported as mounted
- Mount folders whose path has characters outside the system code page work; --dl only accepts a drive letter (C, C: or C:\\)
- No longer needs WmiLight.Native.dll (VSCMount.exe, VSCMount.dll and VSCMount.runtimeconfig.json are all there is), so it can't fail with "Unable to load DLL 'WmiLight.Native.dll'"
- Error reports and logged exceptions include source file and line numbers

### ⌨️ WxTCmd

- The evidence is no longer changed: the database was opened in place, which wrote the -wal into ActivitiesCache.db and deleted the -wal and -shm files. The database (with its -wal) is now copied to a temporary folder and only the copy is read; rows that are only in the -wal are still included (all of them in some Windows 11 databases)
- Text in other languages is kept: payloads were read as ASCII, so Cyrillic, Japanese and other text became ??? in DisplayText, Payload and ClipboardPayload
- ContentInfo shows the file's URI as it is (file:///C:/temp/a.txt?VolumeId={...}&amp;ObjectId={...}); it was cut at fixed places into "file:Unmapped GUID: //C:/..." with characters of the IDs missing
- Windows 10 1803 databases are read: the whole Activity table was lost ("The input string 'UserActionState' was not in a correct format"); one bad row or value no longer loses its table either (the value is listed and the row kept), and a damaged or truncated database gives the rows that could be read
- New columns at the end: AppActivityId (PR #7, Issue #6) and ClipboardText, the copied text in ClipboardPayload decoded from base64 (Issue #9)
- No longer writes SQLite.Interop.dll / libSQLite.Interop.so into the folder it is run from: the SQLite library ships as files next to it, e\_sqlite3.dll (Windows x64), libe\_sqlite3.so (Linux x64) and libe\_sqlite3.dylib (macOS Apple silicon), so Linux and macOS work (`dotnet WxTCmd.dll`) without building a SQLite library. A SQLite.Interop.dll / libSQLite.Interop.so left by an older WxTCmd in a folder it was run from can be deleted
- DisplayText no longer ends in " ()" when there is no app name
- Uses GuidMapping 2026.10.2 (OneDrive and Creative Cloud Files GUIDs named by prefix)
- New --temp: the temporary copy of ActivitiesCache.db goes to a Temp folder next to WxTCmd (or the folder given with --temp) instead of the user's %TEMP%; if it can't be written to, WxTCmd stops and asks for --temp
- Error reports and logged exceptions include source file and line numbers
- Exit code 1 when the tool fails (invalid arguments, a missing input file, a database it can't read, CSVs that can't be written); damaged databases and files that aren't timeline databases are reported and exit 0 (it was always 0)
- New --co saves all console messages to &lt;yyyyMMddHHmmss&gt;\_WxTCmd\_&lt;version&gt;\_ConsoleLog.txt (UTC time) in the --csv directory (Issues #259); --debug and --trace show the time of each message again (they showed the --dt date) (2026.10.1)

### 🖥️ XWFIM

- ⚡ The list of available versions loads about 2-3 times faster: the per-version requests to x-ways.net run 3 at a time instead of one after another
- ⚡ Starts about 4x faster (3.8 s → 0.9 s) and uses about a third of the memory (545 → 182 MB): the program is one .NET single-file exe (with its text files next to it) instead of being packed with Costura
- No longer uses the nsoftware IPWorks components (downloads, encryption and zip files are done by .NET itself) or AlphaFS; now .NET 10 and DevExpress 26.1, sharp on high DPI monitors
- Saved download credentials are protected for your Windows account (DPAPI); settings of earlier versions are read and protected on the next save. Settings saved by 2026.5.0 cannot be read (they were damaged when saved): enter the credentials again
- The window stays responsive while versions are read, credentials are tested and X-Ways is downloaded and installed; testing credentials is one request
- Fixes: Excire was installed when Tesseract was checked; clean install downloaded MPlayer, Tesseract and Excire even when unchecked; BYOD WinHex shortcuts; a missing executable ended the install; an unreadable settings file stopped the program at startup; No on exit saved the settings; links in the menus; starting from a shortcut with another Start in folder
- A download that stops sending data now ends after 60 seconds with an error instead of hanging the install (and the window, which can't be closed during an install) forever. Version lookups, credential and proxy tests time out after 60 seconds too.
- Clicking Install while the version list was being read again crashed and left the window locked (it couldn't be closed). Install is now disabled while the list is read, and any error before the install starts is reported and the window unlocked.
- Proxy settings and Credentials are disabled during an install (they replaced the connection the install was using).
- ⚡ The download speed is now correct. It was divided by the time since the first install of the session started, so it was too low after the first file.
- Create portable reports a canceled copy, a copy error, a removed drive or a too-short WinHex.cfg in its status list instead of crashing with its controls left disabled.
- Verify installation and the proxy Test no longer freeze the window.
- MPlayer's file is no longer requested from x-ways.net when MPlayer isn't selected, and reading the version list makes one request fewer.
- Installs can be canceled while they download (a Cancel button takes Install's place). Nothing in the install folder is changed, and the downloaded files are deleted from Temp
- Create portable shows its own progress bar and a Cancel button in place of Windows' copy dialog. The window stays responsive during the copy, Notepad++ extraction and WinHex.cfg update
- A failed or canceled download no longer leaves a `.part` file in Temp
- Error reports and logged exceptions include source file and line numbers

## 🗂️ Earlier changes

Before the 2026.10 release, changes were listed by the date they were released.

### 📅 2026

#### 2026-05-09

- **XWFIM**: Fix for new URLs from X-Ways, make URLs.txt available externally for easier updates without recompiling

#### 2026-05-01

- **Get-ZimmermanTools**: Update download URL to point to new location due to updated documentation formatting. This version uses an alias I can control, so we will not run into this issue again

#### 2026-04-26

- **All tools**: ALL tools are being released as 2026.5.0, including any outstanding bug fixes, plugin updates, nuget etc. This will be gradual releases over the next few days, but version numbers will standardize to 2026.5.0

#### 2026-04-24

- **Registry Explorer, LECmd**: Add checking for empty long names in rare circumstances
- **Timeline Explorer**: Update plugin for Amcache additional column
- **AmcacheParser**: add originalFileName field

### 📅 2025

#### 2025-05-05

- **MFTECmd, RECmd, Registry Explorer, WxTCmd, SQLECmd**: PR merge updates, nuget

#### 2025-03-26

- **Get-ZimmermanTools**: make default net9

#### 2025-03-22

- **XWFIM**: Install Excite

#### 2025-01-22

- **VSCMount**: Fix WMI issue

#### 2025-01-12

- Some nuget updates to fix a missing dll issue when looking at VSCs

#### 2025-01-08

- **All tools**: Updated Nuget, built net 9.0 versions

### 📅 2024

#### 2024-11-26

- **RECmd, rla**: nuget updates

#### 2024-11-03

- **EvtxECmd, SQLECmd**: Map refresh
- **RECmd**: Batch and plugin updates
- **Registry Explorer**: plugin updates

#### 2024-09-13

- **RECmd, EvtxECmd**: Refresh included maps and batch files
- **Get-ZimmermanTools**: Use downloads.ericzimmermantools.com vs downloads.mikestammer.com

#### 2024-09-11

- **PECmd**: Handle new Windows 11 version, nuget

#### 2024-09-03

- **Timeline Explorer**: Update Plugins (nuget, hayabusa fix for timestamp timezone)
- **rla, RECmd**: Nuget, update recmd to handle DEFAULT hive

#### 2024-06-14

- **LECmd**: Nuget and fix null in target ids

#### 2024-04-06

- **XWFIM**: Fix BYOD url and handle betas with same version as current release version

#### 2024-03-14

- **SBECmd**: Output full path in filename column in dedupe output

#### 2024-03-07

- **MFTECmd**: Parent ref number to unsigned value

### 📅 2023

#### 2023-12-12

- **XWFIM**: Fix broken path due to x-ways changes

#### 2023-10-16

- **JumpList Explorer, JLECmd**: Nuget, appId cleanup

#### 2023-10-11

- **EvtxECmd**: Support ranges in --inc and --ext like 1000-1100

#### 2023-10-05

- Finalize SBE changes for 7z and rar file support

#### 2023-09-27

- **Timeline Explorer**: Add Remove Groups hotkey to Tools. Clears all groups when used

#### 2023-09-14

- **LECmd, JLECmd**: Support passing in codepage for non-English languages when strings are not Unicode

#### 2023-09-08

- **LECmd**: Nuget and fix for missing LongName in certain shellitems

#### 2023-08-07

- **LECmd**: Nuget and update File size label with (bytes)

#### 2023-07-29

- **XWFIM**: Fix BYOD path
- **Registry Explorer**: Fix for serilog logging

#### 2023-06-01

- **ShellBags Explorer**: Fixes for some logging in net 6 version
- **Timeline Explorer**: Update EZTools parser plugin for automaticdestinations

#### 2023-05-21

- **AmcacheParser**: Strip leading 0s from DriverId, nuget

#### 2023-05-18

- **Get-ZimmermanTools**: Now defaults to .net 6 ONLY. use 0 for both 4 and 6 like it used to be

### 📅 2022

#### 2022-12-08

- **EvtxECmd**: Nuget updates (including security issue in Newtonsoft Json), updated/new maps

#### 2022-11-28

- **Registry Explorer**: New/refreshed plugins. (net 6 only)
- **RECmd**: Nuget updates, new/refreshed plugins
- **rla**: Nuget updates

#### 2022-11-26

- **Registry Explorer**: Nuget updates, new/refreshed plugins. (net 6 only)

#### 2022-11-21

- **Timeline Explorer**: Nuget, ignore first row in Generic csv plugin when it starts with '#TYPE '

#### 2022-10-26

- **SrumECmd**: Handle ! in filenames, nuget

#### 2022-10-24

- **JumpList Explorer**: Open jumplist if full path to a jump list is passed as an argument

#### 2022-10-20

- **MFTECmd**: Nuget, add new extended attribute, add more flags

#### 2022-08-05

- **RECmd**: Remove -q as it did nothing, nuget
- **RBCmd**: Update JSON and CSV to be consistent in descriptions, nuget

#### 2022-07-16

- **rla**: with -f, create out directory if it does not exist vs exploding

#### 2022-07-06

- **AmcacheParser**: Nuget updates

#### 2022-06-22

- **MFTECmd**: Add --dr switch to dump resident files to subdirectory of --csv

#### 2022-06-15

- **LECmd, JLECmd, RBCmd, RecentFileCacheParser**: Use UTC datetime in CSV output filename vs local time, nuget

#### 2022-06-02

- **RECmd, rla**: Fix for extracting embedded filenames on non-Windows platforms, nuget
- **XWFIM**: Fix issues with two versions existing at same time (different SR levels)

#### 2022-05-27

- **LECmd**: Nuget dependency updates (fix for Beef004 extension block)

#### 2022-05-20

- **bstrings**: Fix extra text in help text for --sl

#### 2022-04-15

- **EZViewer**: Fix issue with hex viewer context menus

#### 2022-03-10

- **MFTECmd**: $I30 tweaks for empty index pages

#### 2022-03-02

- **MFTECmd**: Add $I30 support, including slack entry extraction, extract slack index entries from FILE records

#### 2022-02-20

- **SBECmd**: Match on LAST username in filename, not first

#### 2022-02-19

- **AmcacheParser**: Resolve absolute path when not provided, nuget

#### 2022-02-16

- **Timeline Explorer**: Add CTRL+SHIFT+F to auto apply selected cell text as column filter, tweaked plugins

#### 2022-02-11

- **Registry Explorer, RECmd, rla**: Nuget and plugin updates

#### 2022-02-08

- **SBECmd**: Tweak how csvs are generated, extract out user profile name vs the entire path to hive in filename, ensure files do not get overwritten when they exist in --csv

#### 2022-02-06

- **SrumECmd**: Add info on unknown tables, nuget, remove more nlog refs

#### 2022-02-03

- **Timeline Explorer**: Add Reopen last closed tab (CTRL+SHIFT+T) to reopen last closed file

#### 2022-01-29

- **RECmd**: give .net6 version its own subfolder
- **Timeline Explorer**: Add psort format plugin

#### 2022-01-28

- **MFTECmd**: All new output for --de mode

#### 2022-01-25

- **MFT Explorer**: net 6 build, nuget
- **SDB Explorer**: net 6 build, nuget
- **JumpList Explorer**: net 6 build, nuget
- **EZViewer**: net 6 build, nuget

#### 2022-01-24

- **rla**: Split out from Registry Explorer, net 6 build, switch to serilog vs nlog
- **RECmd**: Split out from Registry Explorer, net 6 build, switch to serilog vs nlog
- **Registry Explorer**: net 6 build, nuget, updated bookmarks, plugins, etc.
- **SBECmd**: Split out from Shellbags Explorer
- **ShellBags Explorer**: net 6 build, nuget, switch to serilog vs nlog
- **Timeline Explorer**: net 6 build, nuget, switch to serilog vs nlog
- **Hasher**: nuget, control updates

#### 2022-01-22

- **iisGeolocate**: nuget, add net 6 build, switch to serilog vs nlog, update geo database
- **AmcacheParser**: nuget, add net 6 build, switch to serilog vs nlog
- **JLECmd**: nuget, add net 6 build, switch to serilog vs nlog
- **EvtxECmd**: nuget, add net 6 build, switch to serilog vs nlog
- **SQLECmd**: nuget, switch to serilog vs nlog
- **WxTCmd**: nuget, switch to serilog vs nlog
- **Get-ZimmermanTools**: Ignore All\_6.zip

#### 2022-01-21

- **SumECmd**: nuget, add net 6 build, switch to serilog vs nlog
- **AppCompatCacheParser**: nuget, add net 6 build, switch to serilog vs nlog
- **bstrings**: nuget, add net 6 build, switch to serilog vs nlog
- **VSCMount**: nuget, add net 6 build, switch to serilog vs nlog
- **RBCmd**: nuget, add net 6 build, switch to serilog vs nlog
- **RecentFileCacheParser**: nuget, add net 6 build, switch to serilog vs nlog

#### 2022-01-20

- **Get-ZimmermanTools**: Added -NetVersion switch to control what to get. Use -NetVersion to control which flavor of tool you get: 4 for net 4.6.2 and 6 for net 6 (recommended!). If not specified, you get both!!!!!1
- **SrumECmd**: nuget, add net 6 build, switch to serilog vs nlog
- **MFTECmd**: nuget, add net 6 build, switch to serilog vs nlog

#### 2022-01-02

- **Timeline Explorer**: Fix expected header in SBECmd plugin so the CSV is detected as SBE vs generic CSV

### 📅 2021

#### 2021-10-15

- **Timeline Explorer**: Enable sweet sweet rounded corners on Windows 11

#### 2021-10-08

- **SDB Explorer, SrumECmd, SumECmd, ShellBags Explorer, Hasher, MFT Explorer, JLECmd, JumpList Explorer, Timeline Explorer, Registry Explorer, XWFIM, EZViewer, LECmd**: nuget/control updates

#### 2021-10-07

- **MFTECmd**: More better reparse point support, some new EAs involving linux

#### 2021-10-01

- **MFTECmd**: Fix for --blf due to CSVHelper refactoring

#### 2021-09-28

- **MFTECmd**: Add -m switch to resolve parent directories when -f points to a $J file
- **Timeline Explorer**: Add support for ParentPath in $J, better handling of local changes to layouts (your customizations are NOT lost on updates anymore)

#### 2021-09-27

- **MFTECmd**: Resolve deleted paths from orphaned extension records

#### 2021-09-21

- **All Registry based tools**: update parser with significant speed improvements when lots of deleted items are present

#### 2021-09-09

- **Registry Explorer**: New plugins

#### 2021-09-03

- **MFTECmd**: Add check for invalid dates in IndexEntry

#### 2021-08-08

- **Timeline Explorer**: Fix CTRL-T for generic files
- **bstrings**: add --ms switch for max size. nuget

#### 2021-07-23

- **XWFIM**: Add Tesseract

#### 2021-07-21

- **ShellBags Explorer**: Fix CSV Export having 'height' columns when exporting CSV from GUI
- **Timeline Explorer**: Update EZTools plugin for SBE fix

#### 2021-07-13

- **RECmd**: Add 'ROOT' alias, to always find the root of the hive, regardless of name. In some hives, ROOT is the key name, but in others, it is not known without using Registry Explorer or similar. Using --kn ROOT gets around this and will always return the root of the hive (and display the actual name)

#### 2021-07-07

- **EvtxECmd**: Nuget updates, verify directory exists and exit if not with -d

#### 2021-07-01

- **Timeline Explorer**: Add Nirsoft BrowsingHistoryView CSV support

#### 2021-06-24

- **Registry Explorer**: Add syncing of bookmarks with Github via Bookmarks menu

#### 2021-06-15

- **AmcacheParser, AppCompatCacheParser**: Make binary smaller

#### 2021-06-09

- **Timeline Explorer**: Add option (on by default) for multiline tabs vs a single row of tabs that goes out to the right. Setting is remembered
- **LECmd**: Update nuget, update lnk parser

#### 2021-06-08

- **RECmd**: Tweaks for overlong paths in --csv when writing plugin output in -f mode (vs -d mode).

#### 2021-06-04

- **RECmd**: Tweaks for overlong paths in --csv when writing plugin output. This version replaces the base path (-d) in the output filename, making the plugin output names MUCH shorter when -d is very lengthy to begin with

#### 2021-05-28

- **Registry Explorer, RECmd**: Tweaks for determining plugins using regex vs \* glob patterns, improved RECmd batch mode functionality

#### 2021-05-27

- **WxTCmd**: Fix issue with StartTime in ActivityOperations output

#### 2021-05-26

- **All GUIs**: Nuget and third party controls updates

#### 2021-05-24

- **All Registry based tools**: Update to latest Registry nuget package containing improvements for processing hives with empty pages. This also greatly improves parsing hives extracted from memory. Thanks to Martin Willing for the sample hive!

#### 2021-05-06

- **SrumECmd**: AnyCPU vs x86

#### 2021-04-16

- **Timeline Explorer**: Handle situation where a file has only a header and no data. In this case, the file will not be loaded and a warning message is shown in the Messages window
- **Registry Explorer**: Update to .net 4.7.2 as required version (included in Windows 10 since 1809)
- **ShellBags Explorer**: Update to .net 4.7.2 as required version (included in Windows 10 since 1809)
- **All GUIs**: Add License.txt file (MIT)

#### 2021-04-15

- **Registry Explorer**: Add LastWriteTimestamp to WordWheelQuery plugin
- **RECmd**: nuget, add search hit summary at end of run

#### 2021-04-14

- **Timeline Explorer**: Add support for double clicking a TLE session file and having it load

#### 2021-04-12

- **LECmd**: Handle corrupt data in shell item (0x31)

#### 2021-04-08

- **Timeline Explorer**: Nuget updates, add support for SumECmd CSV output, add shortcuts for expand/collapse groups to Tools (and keyboard shortcut)

#### 2021-04-05

- **EvtxECmd**: Tweak internal regex to preserve nested data better

#### 2021-03-20

- **Many programs**: Embed pdb info to ease fixing any issues found, nuget and control updates

#### 2021-03-18

- **iisGeolocate**: tweaks for handling multi-gig logs, other improvements

#### 2021-03-17

- **iisGeolocate**: total rewrite

#### 2021-03-12

- **MFTECmd**: Fix output with symlink reparse points where the name was slightly off

#### 2021-03-11

- **SBECmd**: Fix issue dumpming parameters when none were supplied

#### 2021-03-09

- **Timeline Explorer**: Add support for CTRL-F'ing generic CSV files

#### 2021-03-08

- **SQLECmd**: Include name of database in CSV files, prevent 0 byte files from being created (i.e. when no data was found that matches a query), updated maps
- **WxTCmd**: Include profile name where database was found in CSV names, when present. nuget updates
- **Timeline Explorer**: Add support for KAPE Minitimeline output csv, add line and tag columns to generic CSV plugin

#### 2021-03-01

- **LECmd**: Remove BOM from CSV

#### 2021-02-23

- **LECmd**: Resolve relative paths to absolute path, nuget updates
- **JLECmd**: Resolve relative paths to absolute path, nuget updates

#### 2021-02-22

- **SQLECmd**: updated maps, handle file in use errors in hunt mode

#### 2021-02-19

- **RECmd**: For Batch mode, put all plugin csv files into a subdirectory of --csv (based on the timestamp of the main batch file) so the main batch file is separate from the plugin related details

#### 2021-02-15

- **Registry Explorer**: 10 or so new plugins!!
- **SQLECmd**: Nuget updates, fix example referencing wrong program

#### 2021-02-13

- **TimeApp**: IP addresses use async, move IPs to own tab. Tighten up layout

#### 2021-02-12

- **TimeApp**: add ipv6, countdown and stopwatch

#### 2021-02-11

- **SQLECmd**: updated maps

#### 2021-02-04

- **Timeline Explorer**: CSV parser fix upstream
- **EvtxECmd**: Now that Csv parser is fixed, go back to json payload as its more descriptive

#### 2021-02-03

- **RECmd**: Add --sync switch to auto update batch files from Github repo

#### 2021-02-02

- **EvtxECmd**: Replace json payload with plain text as it greatly shrinks the file sizes of the CSVs while retaining all the searchable data. If more details are needed, dump to XML and look at relevant record.
- **Timeline Explorer**: EvtxECmd plugin adjust for EvtxECmd

#### 2021-02-01

- **RECmd**: Add --jsonf
- **Timeline Explorer**: EvtxECmd plugin header fix

#### 2021-01-28

- **RECmd**: More binary converter options (Epoch, Sid)

#### 2021-01-26

- **EvtxECmd**: Create Maps dir if its missing, nuget

#### 2021-01-25

- **Registry Explorer**: Nuget/controls, updated network plugin
- **ShellBags Explorer**: Nuget/controls, fix for Has Explored in grid

#### 2021-01-20

- **MFTECmd**: Nuget updates, add --fl switch for condensed file listing
- **Timeline Explorer**: Update EZTools plugin for MFTECmd --fl file

#### 2021-01-19

- **JumpList Explorer**: nuget and controls
- **JLECmd**: Nuget updates

#### 2021-01-17

- **RECmd**: nuget, utf8 encoding for plugin csv

#### 2021-01-12

- **PECmd**: Read only file fix
- **EvtxECmd**: Update maps

#### 2021-01-11

- **Registry Explorer**: Add TaskCache plugin

#### 2021-01-07

- **PECmd**: Nuget updates, use FIPS compliant SHA-1. no version bump

#### 2021-01-05

- **Registry Explorer**: nuget, splash screen fix

#### 2021-01-01

- **SrumECmd**: Handle dupe key issue between energy usage and LT table

### 📅 2020

#### 2020-12-30

- **SrumECmd**: Initial release

#### 2020-12-28

- **Timeline Explorer**: Plugin tweaks, support a new EZTool, fix issue accessing plugin menu more than once, nuget

#### 2020-12-21

- **EvtxECmd**: Updated maps, make Provider mandatory, rename maps for more consistency, nuget

#### 2020-12-17

- **Timeline Explorer**: Add Excel support back so xlsx and xls files open in TLE vs showing crazy stuff

#### 2020-12-13

- **WxTCmd**: Tons of cleanup and new stuff from json. More consistent between data from two primary tables, new fields including timezone, etc. Nuget updates, etc
- **Timeline Explorer**: Nuget, controls, all viewers are now in plugins vs built-in. This means anyone can add new supported types. Added View \| Messages window, added Debug option when parsing, added View \| Plugins, lots of other polish. Note that Layout names have changed!

#### 2020-12-11

- **AmcacheParser**: Handle a few more value names, nuget (no version bump)

#### 2020-12-07

- **LECmd**: Nuget updates, handle long file paths, ignore 0 byte files

#### 2020-11-30

- **XWFIM**: Version 2.0! Nuget and control updates, plus drop tooltips.txt in XWF folder if it does not exist

#### 2020-11-25

- **EvtxECmd**: Nuget updates, updated maps. Change to read entire XML string vs just the EventData\|UserData. Allows for more flexibility in maps. Nothing needs to change with any maps!

#### 2020-11-20

- **Registry Explorer**: Nuget updates, improvements to Registry parser, plugin updates. 3rd party control updates

#### 2020-11-16

- **Timeline Explorer**: Add Copy on right click to Options, add Kape Triage timeline support, nuget

#### 2020-11-13

- **Timeline Explorer**: fix read only Datetime format, remove options menu from Tools, pull out copy header to main Tools menu

#### 2020-11-09

- **MFTECmd**: Make Bodyfile UTF-8

#### 2020-11-06

- **Timeline Explorer**: Fix copy cells to clipboard and honoring Copy Column header

#### 2020-11-05

- **Timeline Explorer**: Hide Find form vs closing it to prevent error on reopening

#### 2020-11-04

- **Timeline Explorer**: Fix more plaso related issues...

#### 2020-10-31

- **Timeline Explorer**: Added global find for all supported file types via Tools menu or CTRL-F, added a bunch of shortcuts to Tools menu, added new shortcut to select all values in current column, added open file count to bottom status bar

#### 2020-10-28

- **Timeline Explorer**: Control and nuget updates, double click active file to copy full path to clipboard

#### 2020-10-25

- **Timeline Explorer**: Remove filters for appcompatcache and evtxecmd in layout

#### 2020-10-13

- **Timeline Explorer**: Session files now remember each file's tagged rows between TLE runs

#### 2020-10-12

- **Timeline Explorer**: Update for new EvtxECmd map (added Keywords column)
- **EvtxECmd**: Add optional Provider and lookup tables (See System\_1.map and System\_42.map for examples)

#### 2020-10-08

- **Timeline Explorer**: Added new shortcut, CTRL-E, to clear all filters

#### 2020-10-05

- **EvtxECmd**: Republish after fixing regression with new DanderSpritz detection

#### 2020-10-01

- **EvtxECmd**: Nuget updates, updated maps, Added DanderSpritz hidden record detection. Try it with <https://github.com/3gstudent/Eventlogedit-evtx--Evolution/blob/master/System2.evtx>
- **Timeline Explorer**: Updated to handle new EvtxECmd CSV

#### 2020-09-09

- **EvtxECmd**: handle flush errors, nuget, updated maps

#### 2020-08-20

- **Timeline Explorer**: Tab management added
- **Registry Explorer**: Add TrustedDocs plugin
- **Timeline Explorer**: Updated controls, nuget. Handle invalid timestamps generated by Plaso...

#### 2020-07-23

- **Registry Explorer**: Updated controls and nuget and added Windows Product Key decoder to Data Interpreter

#### 2020-07-20

- **Timeline Explorer**: Add CTRL-R shortcut for resize columns command

#### 2020-07-01

- **Timeline Explorer**: Clean up TZWorks data...

#### 2020-06-25

- **RECmd**: Tweak batch mode to include key when it contained no values to ensure all timestamps are accounted for

#### 2020-06-17

- **AppCompatCacheParser**: Fix erroneous message about no logs and not being administrator (did not bump version #)

#### 2020-06-09

- **JLECmd**: Fix issue with extra \\\\ in UNC path for absolute path (no version # bump)
- **JumpList Explorer**: Updated controls, fix extra \\\\ in UNC path. Smaller binary (no ver # change)
- **Timeline Explorer**: Updated controls (no ver # change)

#### 2020-05-12

- **Registry Explorer**: Fix label in prefs, fix issue with Available bookmarks font in tree, update controls

#### 2020-05-06

- **bstrings**: Fix examples missing a dash, updated nuget
- **Timeline Explorer**: Fix issue with header for SBECmd detection, updated controls

#### 2020-05-01

- **XWFIM**: Fix issue with binary path in BYOD lnk file

#### 2020-04-17

- **ShellBags Explorer**: Populate ‘HasExplored’ in SBECmd output
- **LECmd**: Fix HTML missing labels, update css, change target accessed to accessed (vs mod)
- **JLECmd**: Fix css issues

#### 2020-04-16

- **JLECmd**: Do not favor 32-bit process, nuget (smaller binary!)
- **LECmd**: Do not favor 32-bit process, nuget (smaller binary!)
- **MFTECmd**: nuget (smaller binary!)

#### 2020-04-14

- **AmcacheParser**: Sign binary

#### 2020-04-02

- **Hasher**: Updated controls

#### 2020-03-02

- **MFTECmd**: Remove extra tab char in last accessed timestamp
- **JLECmd**: Remove comma in MRU value when &gt;= 1000

#### 2020-02-20

- **Get-ZimmermanTools**: Update signing cert

#### 2020-02-06

- Everything has been updated (more or less) for signing with a new code certificate
- **Timeline Explorer**: Updated controls and bump version to 1.0!
- **ShellBags Explorer**: Updated nuget and 3rd party controls, several new GUIDs
- **bstrings**: Nuget updates
- **AmcacheParser**: nuget updates, handle additional value names
- **AppCompatCacheParser**: Nuget updates
- **EZViewer**: Nuget and 3rd party controls
- **EvtxECmd**: Nuget updates, updated and new maps
- **Hasher**: Nuget and 3rd party controls
- **JLECmd**: Nuget updates
- **JumpList Explorer**: Nuget and 3rd party controls
- **JLECmd**: Nuget updates
- **MFTECmd**: Nuget updates
- **MFT Explorer**: Nuget and 3rd party controls
- **PECmd**: Nuget updates
- **RBCmd**: Nuget updates
- **RecentFileCacheParser**: Nuget updates
- **SDB Explorer**: Nuget and 3rd party controls
- **VSCMount**: Nuget updates
- **WxTCmd**: Nuget updates
- **Registry Explorer**: Nuget and 3rd party controls

#### 2020-01-23

- **AppCompatCacheParser**: Move debug option from -d to --debug to be consistent with other tools

#### 2020-01-13

- **WxTCmd**: Add support for ActivityOperations table

#### 2020-01-08

- **Registry Explorer**: Updated controls, new plugin, nuget, and some fixes for fringe issues

#### 2020-01-07

- **bstrings**: Nuget updates, new code signing cert

#### 2020-01-06

- **MFTECmd**: Handle zero length SACL in $SDS file
- **Timeline Explorer**: Add new SigCheck related file for Troy Larson

### 📅 2019

#### 2019-12-16

- **Hasher**: Allow window to be smaller, updated controls and nuget

#### 2019-11-22

- **AmcacheParser**: Fix issue reading amcache.hve on a live system (buffer too small error)
- **AppCompatCacheParser**: Nuget updates
- **Registry Explorer**: Fix very rare bug related to value slack search when looking at free vk records

#### 2019-11-21

- **PECmd**: Fix issue with css for HTML report with mislabeled Volume timestamp

#### 2019-11-20

- **Timeline Explorer**: Read unsupported files fully on opening to prevent error when closing tab

#### 2019-11-18

- **Timeline Explorer**: Always use comma separator in unknown CSVs

#### 2019-11-12

- **Timeline Explorer**: Nicer looking filter criteria

#### 2019-11-05

- **Registry Explorer**: Fix context menu to copy key name to clipboard
- **Timeline Explorer**: Better json formatting in MiniView, add ability to adjust font size in MiniView per dfirfpi

#### 2019-11-01

- **Registry Explorer**: Minor visual tweaks

#### 2019-10-31

- **Registry Explorer**: Fix issue showing Tech details from available bookmarks, BOLD the selected key in available bookmarks

#### 2019-10-30

- **EvtxECmd**: Updated maps to most recent versions

#### 2019-10-29

- **Timeline Explorer**: Updated controls to allow for sorting by group summary, improved utilization of space when using Find, nuget updates, and Miniview format option improvements

#### 2019-10-28

- **Registry Explorer**: Fix issue showing deleted values in some cases, nuget and 3rd party control updates
- **LECmd**: Add support for shellitem type 0x22

#### 2019-10-19

- **MFTECmd**: SDS parsing fix when lots of empty pages exist

#### 2019-10-18

- **JumpList Explorer**: Display summary of jump list for automatic destinations in the main grid when root item is selected

#### 2019-10-15

- **Timeline Explorer**: Prevent crash when trying to tag non-supported files (i.e. no Tag or Line # column)
- **JLECmd**: Change labels for Automatic destinations properties to reflect 'DestList Created on' and 'DestList Modified on' so people do not thing these timestamps are target file related

#### 2019-10-08

- **Timeline Explorer**: Tweak drag and drop

#### 2019-10-07

- **Timeline Explorer**: Handle updated $J format (source file column)

#### 2019-10-04

- **RECmd**: Add IncludeBinary and BinaryConvert options to batch mode. See example for how to use
- **Get-ZimmermanTools**: Do not mark file as updated in CSV if file is in use

#### 2019-10-03

- **bstrings**: Add another Dashcoin regex

#### 2019-10-01

- **AppCompatCacheParser**: Add source file and duplicate columns to output
- **Timeline Explorer**: Handle updated AppCompat format
- **RECmd**: Add --sa switch to search for everything at once

#### 2019-09-30

- **Timeline Explorer**: Handle updated autorunsc output

#### 2019-09-26

- **JumpList Explorer, JLECmd**: Handle case where a destlist does not have a lnk file
- **Timeline Explorer**: Add ability to open a directory from Open with like you can do with a file, add ability to view raw and formatted messages in Details view for supertimeline

#### 2019-09-20

- **Timeline Explorer**: Fix for random \\n issues

#### 2019-09-18

- **Registry Explorer**: Display RegFileTime values in UTC vs local time in values grid

#### 2019-09-17

- **JLECmd**: Add interaction count to destlist section/csv, nuget and control updates
- **JumpList Explorer**: Add interaction count to destlist section, nuget and control updates
- **Timeline Explorer**: handle new jumplist format with interaction count, nuget updates
- **Registry Explorer, RECmd**: Added MountedDevices plugin, fix fringe issue finding root key

#### 2019-09-05

- **Registry Explorer**: Add ability to open live system hives (when running as admin), general tweaks and cleanup, added rla.exe

#### 2019-09-04

- **PECmd**: Work around more FTK Imager stupidity with mounted images, nuget updates

#### 2019-09-03

- **AmcacheParser**: Nuget updates, pull in last write timestamps from subkeys vs parent key for a few of the new format data sets
- **EZViewer**: Updated controls, nuget

#### 2019-08-29

- **MFTECmd**: Updated nuget and use ads attribue id vs inheriting primary $DATA id in inode column when generating body file

#### 2019-08-28

- **XWFIM**: Handle renaming of BYOD executables, control updates

#### 2019-08-26

- **EvtxECmd**: Handle possible issue with relative paths on command line, nuget updates

#### 2019-08-21

- **Get-ZimmermanTools**: Add proxy support via PR

#### 2019-08-19

- **Get-ZimmermanTools**: Honor $dest for everything, not just the CSV!

#### 2019-08-17

- **AppCompatCacheParser**: Resolve full path to hive before processing logs when no relative or absolute path is provided, nuget updates

#### 2019-08-12

- **RECmd**: Fix missing nuget package for .net core build

#### 2019-08-10

- **PECmd**: Secondary check a pf file still exists (after it was initially found) before opening it

#### 2019-08-08

- **MFT Explorer**: Added

#### 2019-08-06

- **Registry Explorer**: Allow changing font size via Preferences. Changes take effect on restarting Registry Explorer, nuget updates and control updates
- **EvtxECmd**: Update included Maps

#### 2019-08-03

- **EvtxECmd**: Handle edge case where event payload has a non-standard name

#### 2019-07-31

- **EZViewer**: Add support for pictures (jpg, png, bmp, etc)

#### 2019-07-30

- **Registry Explorer**: Show all unassociated deleted values in their own group "Unassociated deleted values". This contains all values that could not be associated back to a key. Also added 'export bytes to file' in hex viewer, add double clicking 'Selected bytes' to enter number of bytes to select. This version also contains fixes when holding SHIFT when loading hives for auto LOG replay

#### 2019-07-16

- **PECmd**: Handle newer windows 10 format more reliably

#### 2019-07-12

- **RECmd**: Add/update 3 batch mode files from Troy Larson for RECmd

#### 2019-07-06

- **Timeline Explorer**: Convert \\n to a real line feed in l2t csv file

#### 2019-07-03

- **MFTECmd**: Handle all bad timestamps and add two new extended attributes

#### 2019-07-01

- **MFTECmd**: Handling bad RecordModifiedOn timestamp value in free FILE record

#### 2019-06-30

- **Get-ZimmermanTools**: Formatting, fix for running on PS core on mac

#### 2019-06-26

- **iisGeolocate**: Pretty much complete rewrite to handle data better, especially if fields contain spaces

#### 2019-06-24

- **EvtxECmd**: Updated maps, added --sync command, SIGNIFICANT reduction in memory usage
- **Get-ZimmermanTools**: Remove dependency on 7-Zip
- **EZViewer**: Updated controls and nuget
- **Hasher**: Updated controls and nuget
- **JumpList Explorer**: Updated controls and nuget
- **ShellBags Explorer**: Updated controls and nuget
- **SDB Explorer**: Updated controls and nuget
- **Timeline Explorer**: Updated controls and nuget
- **WxTCmd**: Fix tsv extension for one of the output files, updated controls and nuget
- **Timeline Explorer**: Fix issue loading WxTCmd Activity\_PackageIDs csv

#### 2019-06-21

- **PECmd**: Handle issue where detected ADS is not a prefetch file
- **Timeline Explorer**: Detect CSVs from RECmd batch mode

#### 2019-06-20

- **RECmd**: Fix missing dependency for extensionblocks for some plugins
- **Registry Explorer**: Update checksum in addition to sequence number when replaying transaction logs

#### 2019-06-17

- **PECmd**: ISO8601 timestamps when exporting to json

#### 2019-06-12

- **EvtxECmd**: 9 new maps from Troy Larson for Microsoft-Windows-Shell-Core\_Operational log
- **RECmd**: Updated batch file from Troy Larson

#### 2019-06-11

- **EvtxECmd**: Update Microsoft-Windows-TerminalServices-RemoteConnectionManager\_Operational\_1149.map to find properties in elements vs attributes

#### 2019-06-10

- **EvtxECmd**: Add 16 new maps, add auto detection and reporting when the timestamp is observed going backwards
- **Timeline Explorer**: Add 'Format' button to cell details view (double click a cell to show all contents in a dedicated window). Format currently supports JSON data (useful when looking at EvtxECmd output)
- **Registry Explorer**: Updated plugin for Terminal Services and BAM/DAM, add Unload project option, updated controls and nuget

#### 2019-06-01

- **Timeline Explorer**: Fix replacing \\n in certain cases

#### 2019-05-31

- **MFTECmd**: When using --do, verify --dd is set

#### 2019-05-30

- **Get-ZimmermanTools**: Update to show progress of overall downloads

#### 2019-05-27

- **PECmd**: Open files as read only
- **MFTECmd**: SDS fix for 0 size records
- **EvtxECmd**: Add earliest and latest event date to metrics
- **PECmd**: Detect pf files in ADSs, warn and display them

#### 2019-05-24

- **PECmd**: Better json output, remove pretty switch, add jsonf switch, nuget update

#### 2019-05-20

- **EZViewer**: Updated controls
- **LECmd**: Set empty properties to null when generating json output

#### 2019-05-17

- **LECmd**: Add NetworkPath column to CSV and json output that pulls NetworkShareInfo.NetworkShareName out of the lnk file (if present)
- **Timeline Explorer**: Handle new column in LECmd
- **RBCmd**: Tweaked error handling when paths do not exist after searching
- **VSCMount**: Updated VSC discovery method, improved timestamp resolution, updated controls

#### 2019-05-16

- **MFTECmd**: Fix bodyfile output when using --bodyf

#### 2019-05-15

- **JumpList Explorer**: Control updates, add CTRL-R to Reload all jump lists

#### 2019-05-14

- **MFTECmd**: Fix for processing loose files when vss is not in use
- **EvtxECmd**: Add --vss switch that finds and extracts evtx from all available VSCs on drive letter specified by -f or -d. Note the same path is used when looking in VSCs. Also added --dedupe which is ON by default
- **RECmd**: Add --vss switch that finds and extracts Registry hives from all available VSCs on drive letter specified by -f or -d. Note the same path is used when looking in VSCs. Also added --dedupe which is ON by default
- **PECmd**: Add --vss switch that finds and extracts prefetch hives from all available VSCs on drive letter specified by -f or -d. Note the same path is used when looking in VSCs. Also added --dedupe which is ON by default

#### 2019-05-13

- **MFTECmd**: Add --vss switch that finds and extracts data from all available VSCs on drive letter specified by -f. Also added --dedupe which is OFF by default

#### 2019-05-08

- **EvtxECmd**: Added --sd and --ed for timestamp filtering. use the same format to pass in strings as --dt

#### 2019-05-06

- **Timeline Explorer**: Handle Payload column in EvtxECmd CSV output

#### 2019-05-04

- **RECmd**: Honor datetime format when saving out plugin details in batch mode

#### 2019-05-03

- **EvtxECmd**: Add --pj switch to include event payload (only the payload) as json in CSV export. it is ON by default.

#### 2019-05-01

- **LECmd**: Better json output, updated MAC vendor list, nuget
- **MFTECmd**: Better json output, nuget

#### 2019-04-30

- **EvtxECmd**: New maps

#### 2019-04-29

- **EvtxECmd**: For json, use ISO8601 format

#### 2019-04-28

- **ShellBags Explorer**: Fix SBECmd not liking relative paths in some cases, updated controls

#### 2019-04-27

- **Timeline Explorer**: More screen real estate, more search options, consolidated search interface, support for EvtxECmd CSV output

#### 2019-04-26

- **EvtxECmd**: Added maps, make timestamp in CSV show up with full sub-second precision
- **EvtxECmd**: Beta release

#### 2019-04-10

- **MFTECmd**: Handle case where --csv .\\foo is used vs using full path

#### 2019-04-02

- **Registry Explorer**: New plugin for Taskband, updated controls

#### 2019-03-28

- **LECmd, JLECmd, JumpList Explorer**: Updated Property store GUID/ID pairings (thanks David Via!), improved Guid to folder lookups, nuget updates

#### 2019-03-27

- **AmcacheParser**: fix issue with Shortcuts key in new format when value is missing, nuget updates, add --debug and --trace switches

#### 2019-03-24

- **MFTECmd**: Swap out --vl switch for --debug and --trace, fix for rare issue when reading USN to find starting point where the data actually starts

#### 2019-03-18

- **MFTECmd**: Fix issue with --de complaining about destination path

#### 2019-03-16

- **Registry Explorer**: Updated controls, more batch mode examples for RECmd
- **Get-ZimmermanTools**: Add missing period

#### 2019-03-15

- **ShellBags Explorer**: Fix issue with csv exports, updated controls, new guids

#### 2019-03-04

- **MFTECmd**: Handle case where a file with same name as directory being created already exists
- **bstrings**: Add crypto wallet regex patterns
- **MFTECmd**: Handle missing time in extended EA

#### 2019-03-13

- **PECmd**: Add initial support for new Windows 10 prefetch format
- **AppCompatCacheParser**: Handle locked files to include LOG files
- **AmcacheParser**: More properties from different keys

#### 2019-03-11

- **MFTECmd**: Verify drive letter exists for --csv, --json, etc before running. Show more details about MFT processed (FILE record count, size) when running

#### 2019-03-09

- **MFTECmd**: MUCH improved handling of giant files, locked or otherwise

#### 2019-03-08

- **Timeline Explorer**: In FLS timelines, treat the meta column as a number for sorting purposes
- **bstrings**: Handle \\ at end of -o as it expects a FILE, not a directory

#### 2019-03-06

- **EZViewer**: Fix issue with not showing PDFs

#### 2019-03-03

- **EZViewer**: Add Extensions.yaml which lets end user control which extenions get opened by which viewer.

#### 2019-02-28

- **Get-ZimmermanTools**: Add -UseBasicParsing to all Invoke-WebRequest calls
- **Registry Explorer**: RECmd batch mode matching rewritten. Much faster and more accurate. GUI tweaks in Registry Explorer
- **MFTECmd**: Dependencies and handle some input errors
- **ShellBags Explorer**: control update, nuget, some new GUIDs
- **bstrings**: Tweak input validation

#### 2019-02-26

- **SDB Explorer**: Updated controls, File \| Open set to CTRL-o
- **Timeline Explorer**: File \| Open set to CTRL-o, Add Option to tontrol if headers are copied to clipboard on CTRL-C (See Tools menu)
- **Registry Explorer**: File \| Open set to CTRL-o
- **ShellBags Explorer**: File \| Open set to CTRL-o
- **JumpList Explorer**: File \| Open set to CTRL-o
- **EZViewer**: File \| Open set to CTRL-o

#### 2019-02-23

- **LECmd**: Update shell item
- **Registry Explorer**: Show RegMultiSz values split up, one string on a line, vs all on one
- **JumpList Explorer**: Lnk dependency bump
- **JLECmd**: Lnk dependency bump

#### 2019-02-21

- **bstrings**: relative path tweak, nuget, and fody
- **EZViewer**: Fix issue creating context menus in Explorer

#### 2019-02-19

- **AmcacheParser**: Handle more value names, clean up errors with free records

#### 2019-02-18

- **PECmd**: Recursive search fixes for permission issues.

#### 2019-02-15

- **RECmd**: Use case-insensitive comparisons in batch mode

#### 2019-02-09

- **Timeline Explorer**: Update to handle ($&amp;\*#^\*&amp;^%%$ column header changes in pescan, nuget updates

#### 2019-02-06

- **EZViewer**: Allow resizeing rows and columns in spreadsheets
- **EZViewer**: Add options to add/remove context menu "Open with EZViewer" entry
- **EZViewer**: Add Hex view that allows you to see the loaded file in a hex viewer. Use button in lower right to load.

#### 2019-02-05

- **EZViewer**: INITIAL RELEASE!! Standalone, zero dependency viewer for .doc, .docx, .xls, .xlsx, .txt, .log, .rtf, .otd, .htm, .html, .mht, .csv, and .pdf. Any non-supported files are shown in a hex editor (with data interpreter!)

#### 2019-02-04

- **LECmd**: Update to handle MTP devices better
- **JLECmd**: : Update to handle lnk files for MTP devices better. nuget updates
- **JumpList Explorer**: : Update to handle lnk files for MTP devices better, nuget, 3rd party controls

#### 2019-01-29

- **MFTECmd**: add jsonf and nuget

#### 2019-01-28

- **AppCompatCacheParser, AmcacheParser**: nuget updates
- **MFTECmd**: Add --fls option (used in conjuction with --de) and 5 extended attributes decoded
- **LECmd**: Better recursive searching when using -d option (faster, ignore reparse points, etc) and nuget updates
- **JLECmd**: Better recursive searching when using -d option (faster, ignore reparse points, etc) and nuget updates

#### 2019-01-25

- **AppCompatCacheParser**: Fix bug related to reading offline hive on SAMBA share

#### 2019-01-24

- **Registry Explorer**: Fix bug when opening offline hives and not prompting for LOG files
- **RBCmd**: When a directory is deleted, show all child files in output vs just top level directory
