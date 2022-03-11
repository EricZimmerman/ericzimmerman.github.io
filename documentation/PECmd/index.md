# PECmd

## Command Line Options

    Description:
      PECmd version 1.5.0.0
   
     Author: Eric Zimmerman (saericzimmerman@gmail.com)
     https://github.com/EricZimmerman/PECmd
   
     Examples: PECmd.exe -f "C:\Temp\CALC.EXE-3FBEF7FD.pf"
               PECmd.exe -f "C:\Temp\CALC.EXE-3FBEF7FD.pf" --json "D:\jsonOutput" --jsonpretty
               PECmd.exe -d "C:\Temp" -k "system32, fonts"
               PECmd.exe -d "C:\Temp" --csv "c:\temp" --csvf foo.csv --json c:\temp\json
               PECmd.exe -d "C:\Windows\Prefetch"
   
               Short options (single letter) are prefixed with a single dash. Long commands are prefixed with two dashes
   
    Usage:
      PECmd [options]
    
    Options:
      -f <f>           File to process. Either this or -d is required
      -d <d>           Directory to recursively process. Either this or -f is required
      -k <k>           Comma separated list of keywords to highlight in output. By default, 'temp' and 'tmp' are
                       highlighted. Any additional keywords will be added to these
      -o <o>           When specified, save prefetch file bytes to the given path. Useful to look at decompressed Win10
                       files
      -q               Do not dump full details about each file processed. Speeds up processing when using --json or --csv
                       [default: False]
      --json <json>    Directory to save JSON formatted results to. Be sure to include the full path in double quotes
      --jsonf <jsonf>  File name to save JSON formatted results to. When present, overrides default name
      --csv <csv>      Directory to save CSV formatted results to. Be sure to include the full path in double quotes
      --csvf <csvf>    File name to save CSV formatted results to. When present, overrides default name
      --html <html>    Directory to save xhtml formatted results to. Be sure to include the full path in double quotes
      --dt <dt>        The custom date/time format to use when displaying time stamps. See https://goo.gl/CNVq0k for
                       options [default: yyyy-MM-dd HH:mm:ss]
      --mp             When true, display higher precision for timestamps [default: False]
      --vss            Process all Volume Shadow Copies that exist on drive specified by -f or -d [default: False]
      --dedupe         Deduplicate -f or -d & VSCs based on SHA-1. First file found wins [default: False]
      --debug          Show debug information during processing [default: False]
      --trace          Show trace information during processing [default: False]
      --version        Show version information
      -?, -h, --help   Show help and usage information
    
    
    Either -f or -d is required. Exiting
    
## Change log

Coming soon!
