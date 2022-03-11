# RecentFileCacheParser

## Command Line Options

    Description:
      RecentFileCacheParser version 1.5.0.0
    
      Author: Eric Zimmerman (saericzimmerman@gmail.com)
      https://github.com/EricZimmerman/RecentFileCacheParser
    
      Examples: RecentFileCacheParser.exe -f "C:\Temp\RecentFileCache.bcf" --csv "c:\temp"
                RecentFileCacheParser.exe -f "C:\Temp\RecentFileCache.bcf" --json "D:\jsonOutput" --jsonpretty
    
                Short options (single letter) are prefixed with a single dash. Long commands are prefixed with two dashes
    
    Usage:
      RecentFileCacheParser [options]
    
    Options:
      -f <f>          File to process. Required
      --csv <csv>     Directory to save CSV formatted results to. Be sure to include the full path in double quotes
      --csvf <csvf>   File name to save CSV formatted results to. When present, overrides default name
      --json <json>   Directory to save json representation to. Use --pretty for a more human readable layout
      --pretty        When exporting to json, use a more human readable layout [default: False]
      -q              Only show the filename being processed vs all output. Useful to speed up exporting to json and/or csv
                      [default: False]
      --version       Show version information
      -?, -h, --help  Show help and usage information
    
    
    -f is required. Exiting

   
## Change log

Coming soon!
