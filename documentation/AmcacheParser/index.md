# AmcacheParser

## Command Line Options
    Description:
      AmcacheParser version 1.5.1.0
    
      Author: Eric Zimmerman (saericzimmerman@gmail.com)
      https://github.com/EricZimmerman/AmcacheParser
    
      Examples: AmcacheParser.exe -f "C:\Temp\amcache\AmcacheWin10.hve" --csv C:\temp
                AmcacheParser.exe -f "C:\Temp\amcache\AmcacheWin10.hve" -i on --csv C:\temp --csvf foo.csv
                AmcacheParser.exe -f "C:\Temp\amcache\AmcacheWin10.hve" -w "c:\temp\whitelist.txt" --csv C:\temp
    
                Short options (single letter) are prefixed with a single dash. Long commands are prefixed with two dashes
    
    Usage:
      AmcacheParser [options]
    
    Options:
      -f <f> (REQUIRED)       Amcache.hve file to parse
      -i                      Include file entries for Programs entries [default: False]
      -w <w>                  Path to file containing SHA-1 hashes to *exclude* from the results. Blacklisting overrides whitelisting
      -b <b>                  Path to file containing SHA-1 hashes to *include* from the results. Blacklisting overrides whitelisting
      --csv <csv> (REQUIRED)  Directory to save CSV formatted results to. Be sure to include the full path in double quotes
      --csvf <csvf>           File name to save CSV formatted results to. When present, overrides default name
      --dt <dt>               The custom date/time format to use when displaying time stamps. See https://goo.gl/CNVq0k for options [default: yyyy-MM-dd HH:mm:ss]
      --mp                    Display higher precision for time stamps [default: False]
      --nl                    When true, ignore transaction log files for dirty hives. Default is FALSE
                               [default: False]
      --debug                 Show debug information during processing [default: False]
      --trace                 Show trace information during processing [default: False]
      --version               Show version information
      -?, -h, --help          Show help and usage information
    
## Change log

Coming soon!

