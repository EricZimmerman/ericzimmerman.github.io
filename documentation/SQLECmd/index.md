# SQLECmd

## Command Line Options

     Description:
       SQLECmd version 1.0.0.0
    
      Author: Eric Zimmerman (saericzimmerman@gmail.com)
      https://github.com/EricZimmerman/SQLECmd
    
      Examples: SQLECmd.exe -f "C:\Temp\someFile.db" --csv "c:\temp\out"
                SQLECmd.exe -d "C:\Temp\" --csv "c:\temp\out"
                SQLECmd.exe -d "C:\Temp\" --hunt --csv "c:\temp\out"
    
                Short options (single letter) are prefixed with a single dash. Long commands are prefixed with two dashes
    
     Usage:
       SQLECmd [options]
     
     Options:
       -f <f>          File to process. This or -d is required
       -d <d>          Directory to process that contains SQLite files. This or -f is required
       --csv <csv>     Directory to save CSV formatted results to
       --json <json>   Directory to save JSON formatted results to
       --dedupe        Deduplicate -f or -d files based on SHA-1. First file found wins [default: True]
       --hunt          When true, all files are looked at regardless of name and file header is used to identify SQLite
                       files, else filename in map is used to find databases [default: False]
       --maps <maps>   The path where event maps are located. Defaults to 'Maps' folder where program was executed [default:
                       C:\Users\CFUser\OneDrive - Kroll\Desktop\EZ Tools\net6\SQLECmd\Maps]
       --sync          If true, the latest maps from https://github.com/EricZimmerman/SQLECmd/tree/master/SQLMap/Maps are
                       downloaded and local maps updated [default: False]
       --debug         Show debug information during processing [default: False]
       --trace         Show trace information during processing [default: False]
       --version       Show version information
       -?, -h, --help  Show help and usage information
     
     
     -f or -d is required. Exiting
     
## Change log

Coming soon!
