# RBCmd

## Command Line Options

    Description:
      RBCmd version 1.5.0.0
   
     Author: Eric Zimmerman (saericzimmerman@gmail.com)
     https://github.com/EricZimmerman/RBCmd
   
     Examples: RBCmd.exe -f "C:\Temp\INFO2"
               RBCmd.exe -f "C:\Temp\$I3VPA17" --csv "D:\csvOutput"
               RBCmd.exe -d "C:\Temp" --csv "c:\temp"
   
               Short options (single letter) are prefixed with a single dash. Long commands are prefixed with two dashes
   
   
    Usage:
      RBCmd [options]
    
    Options:
      -d <d>          Directory to recursively process. Either this or -f is required
      -f <f>          File to process. Either this or -d is required
      -q              Only show the filename being processed vs all output. Useful to speed up exporting to json and/or csv
      --csv <csv>     Directory to save CSV formatted results to. Be sure to include the full path in double quotes
      --csvf <csvf>   File name to save CSV formatted results to. When present, overrides default name
      --dt <dt>       The custom date/time format to use when displaying time stamps. See https://goo.gl/CNVq0k for
                      options. Default is: yyyy-MM-dd HH:mm:ss [default: yyyy-MM-dd HH:mm:ss]
      --debug         Show debug information during processing [default: False]
      --trace         Show trace information during processing [default: False]
      --version       Show version information
      -?, -h, --help  Show help and usage information
    
    
    Either -f or -d is required. Exiting
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
    
## Change log

Coming soon!
