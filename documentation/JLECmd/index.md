# JLECmd

## Command Line Options

   Description:
     JLECmd version 1.5.0.0
   
     Author: Eric Zimmerman (saericzimmerman@gmail.com)
     https://github.com/EricZimmerman/JLECmd
   
     Examples: JLECmd.exe -f "C:\Temp\f01b4d95cf55d32a.customDestinations-ms" --mp
             JLECmd.exe -f "C:\Temp\f01b4d95cf55d32a.automaticDestinations-ms" --json "D:\jsonOutput" --jsonpretty
             JLECmd.exe -d "C:\CustomDestinations" --csv "c:\temp" --html "c:\temp" -q
             JLECmd.exe -d "C:\Users\e\AppData\Roaming\Microsoft\Windows\Recent" --dt "ddd yyyy MM dd HH:mm:ss.fff"
   
             Short options (single letter) are prefixed with a single dash. Long commands are prefixed with two dashes
   
   
    Usage:
      JLECmd [options]
    
    Options:
      -f <f>             File to process. Either this or -d is required
      -d <d>             Directory to recursively process. Either this or -f is required
      --all              Process all files in directory vs. only files matching *.automaticDestinations-ms or
                         *.customDestinations-ms [default: False]
      --csv <csv>        Directory to save CSV formatted results to. This or --json required unless --de or --body is
                         specified
      --csvf <csvf>      File name to save CSV formatted results to. When present, overrides default name
      --json <json>      Directory to save json representation to. Use --pretty for a more human readable layout
      --html <html>      Directory to save xhtml formatted results to. Be sure to include the full path in double quotes
      --pretty           When exporting to json, use a more human readable layout [default: False]
      -q                 Only show the filename being processed vs all output. Useful to speed up exporting to json and/or
                         csv [default: False]
      --ld               Include more information about lnk files [default: False]
      --fd               Include full information about lnk files (Alternatively, dump lnk files using --dumpTo and process
                         with LECmd) [default: False]
      --appIds <appIds>  Path to file containing AppIDs and descriptions (appid|description format). New appIds are added
                         to the built-in list, existing appIds will have their descriptions updated
      --dumpTo <dumpTo>  Directory to save exported lnk files
      --dt <dt>          The custom date/time format to use when displaying timestamps. See https://goo.gl/CNVq0k for
                         options. Default is: yyyy-MM-dd HH:mm:ss [default: yyyy-MM-dd HH:mm:ss]
      --mp               Display higher precision for timestamps [default: False]
      --withDir          When true, show contents of Directory not accounted for in DestList entries [default: False]
      --debug            Show debug information during processing [default: False]
      --trace            Show trace information during processing [default: False]
      --version          Show version information
      -?, -h, --help     Show help and usage information
    
    
    Either -f or -d is required. Exiting
    
## Change log

Coming soon!
