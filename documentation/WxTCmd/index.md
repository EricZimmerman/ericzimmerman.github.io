# WxTCmd

## Command Line Options

     Description:
       WxTCmd version 1.0.0.0
    
       Author: Eric Zimmerman (saericzimmerman@gmail.com)
       https://github.com/EricZimmerman/WxTCmd
    
       Examples: WxTCmd.exe -f "C:\Users\eric\AppData\Local\ConnectedDevicesPlatform\L.eric\ActivitiesCache.db" --csv c:\temp
     
                 Database files are typically found at
       'C:\Users\<profile>\AppData\Local\ConnectedDevicesPlatform\L.<profile>\ActivitiesCache.db'
     
                 Short options (single letter) are prefixed with a single dash. Long commands are prefixed with two dashes
     
     Usage:
       WxTCmd [options]
     
     Options:
       -f <f>          File to process. Required
       --csv <csv>     Directory to save CSV formatted results to. Be sure to include the full path in double quotes
       --dt <dt>       The custom date/time format to use when displaying timestamps. See https://goo.gl/CNVq0k for options
                       [default: yyyy-MM-dd HH:mm:ss]
       --debug         Show debug information during processing [default: False]
       --trace         Show trace information during processing [default: False]
       --version       Show version information
       -?, -h, --help  Show help and usage information
     
     
     -f is required. Exiting

## Change log

Coming soon!
