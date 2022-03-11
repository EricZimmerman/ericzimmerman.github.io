# SrumECmd

## Command Line Options

      Description:
        SrumECmd version 0.5.1.0
    
       Author: Eric Zimmerman (saericzimmerman@gmail.com)
       https://github.com/EricZimmerman/Srum
    
       Examples: SrumECmd.exe -f "C:\Temp\SRUDB.dat" -r "C:\Temp\SOFTWARE" --csv "C:\Temp\"
                 SrumECmd.exe -f "C:\Temp\SRUDB.dat" --csv "c:\temp"
                 SrumECmd.exe -d "C:\Temp" --csv "c:\temp"
    
                 Short options (single letter) are prefixed with a single dash. Long commands are prefixed with two dashes
    
     
      Usage:
        SrumECmd [options]
      
      Options:
        -f <f>                  SRUDB.dat file to parse
        -r <r>                  SOFTWARE hive to process. This is optional, but recommended
        -d <d>                  Directory to recursively process, looking for SRUDB.dat and SOFTWARE hive. This mode is
                                primarily used with KAPE so both SRUDB.dat and SOFTWARE hive can be located
        --csv <csv> (REQUIRED)  Directory to save CSV formatted results to. Be sure to include the full path in double quotes
        --dt <dt>               The custom date/time format to use when displaying time stamps. See https://goo.gl/CNVq0k for 
                                options
                                 [default: yyyy-MM-dd HH:mm:ss]
        --debug                 Show debug information during processing [default: False]
        --trace                 Show trace information during processing [default: False]
        --version               Show version information
        -?, -h, --help          Show help and usage information
     
## Change log

Coming soon!
