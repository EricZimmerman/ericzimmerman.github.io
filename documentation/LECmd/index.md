# LECmd

## Command Line Options

   Description:
     LECmd version 1.5.0.0
   
     Author: Eric Zimmerman (saericzimmerman@gmail.com)
     https://github.com/EricZimmerman/LECmd
   
     Examples: LECmd.exe -f "C:\Temp\foobar.lnk"
               LECmd.exe -f "C:\Temp\somelink.lnk" --json "D:\jsonOutput" --jsonpretty
               LECmd.exe -d "C:\Temp" --csv "c:\temp" --html c:\temp --xml c:\temp\xml -q
               LECmd.exe -f "C:\Temp\some other link.lnk" --nid --neb
               LECmd.exe -d "C:\Temp" --all
   
               Short options (single letter) are prefixed with a single dash. Long commands are prefixed with two dashes
   
    
    Usage:
      LECmd [options]
    
    Options:
      -f <f>          File to process. Either this or -d is required
      -d <d>          Directory to recursively process. Either this or -f is required
      -r              Only process lnk files pointing to removable drives [default: False]
      -q              Only show the filename being processed vs all output. Useful to speed up exporting to json and/or csv
                      [default: False]
      --all           Process all files in directory vs. only files matching *.lnk [default: False]
      --csv <csv>     Directory to save CSV formatted results to. Be sure to include the full path in double quotes
      --csvf <csvf>   File name to save CSV formatted results to. When present, overrides default name
      --xml <xml>     Directory to save XML formatted results to. Be sure to include the full path in double quotes
      --html <html>   Directory to save xhtml formatted results to. Be sure to include the full path in double quotes
      --json <json>   Directory to save json representation to. Use --pretty for a more human readable layout
      --pretty        When exporting to json, use a more human readable layout [default: False]
      --nid           Suppress Target ID list details from being displayed [default: False]
      --neb           Suppress Extra blocks information from being displayed [default: False]
      --dt <dt>       The custom date/time format to use when displaying time stamps. See https://goo.gl/CNVq0k for options
                      [default: yyyy-MM-dd HH:mm:ss]
      --mp            Display higher precision for time stamps [default: False]
      --debug         Show debug information during processing [default: False]
      --trace         Show trace information during processing [default: False]
      --version       Show version information
      -?, -h, --help  Show help and usage information
    
    
    Either -f or -d is required. Exiting
    
## Change log

Coming soon!
