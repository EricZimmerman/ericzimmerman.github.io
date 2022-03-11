# AppCompatCacheParser

## Command Line Options

   Description:
     AppCompatCache Parser version 1.5.0.0
   
     Author: Eric Zimmerman (saericzimmerman@gmail.com)
     https://github.com/EricZimmerman/AppCompatCacheParser
   
     Examples: AppCompatCacheParser.exe --csv c:\temp -t -c 2
               AppCompatCacheParser.exe --csv c:\temp --csvf results.csv
   
               Short options (single letter) are prefixed with a single dash. Long commands are prefixed with two dashes
   
   
    Usage:
      AppCompatCacheParser [options]
    
    Options:
      -f <f>                  Full path to SYSTEM hive to process. If this option is not specified, the live Registry will
                              be used
      --csv <csv> (REQUIRED)  Directory to save CSV formatted results to. Be sure to include the full path in double quotes
      --csvf <csvf>           File name to save CSV formatted results to. When present, overrides default name
      --c <c>                 The ControlSet to parse. Default is to extract all control sets [default: -1]
      -t                      Sorts last modified timestamps in descending order [default: False]
      --dt <dt>               The custom date/time format to use when displaying time stamps. See https://goo.gl/CNVq0k for 
                              options [default: yyyy-MM-dd HH:mm:ss]
      --nl                    When true, ignore transaction log files for dirty hives [default: False]
      --debug                 Show debug information during processing [default: False]
      --trace                 Show trace information during processing [default: False]
      --version               Show version information
      -?, -h, --help          Show help and usage information
    
   
## Change log

Coming soon!
