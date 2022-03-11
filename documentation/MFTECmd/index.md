# MFTECmd

## Command Line Options
    Description:
      MFTECmd version 1.2.1.0
    
      Author: Eric Zimmerman (saericzimmerman@gmail.com)
      https://github.com/EricZimmerman/MFTECmd
    
      Examples: MFTECmd.exe -f "C:\Temp\SomeMFT" --csv "c:\temp\out" --csvf MyOutputFile.csv
                MFTECmd.exe -f "C:\Temp\SomeMFT" --csv "c:\temp\out"
                MFTECmd.exe -f "C:\Temp\SomeMFT" --json "c:\temp\jsonout"
                MFTECmd.exe -f "C:\Temp\SomeMFT" --body "c:\temp\bout" --bdl c
                MFTECmd.exe -f "C:\Temp\SomeMFT" --de 5-5
    
                Short options (single letter) are prefixed with a single dash. Long commands are prefixed with two dashes
    
    Usage:
      MFTECmd [options]
    
    Options:
      -f <f>           File to process ($MFT | $J | $Boot | $SDS | $I30). Required
      -m <m>           $MFT file to use when -f points to a $J file (Use this to resolve parent path in $J CSV output).
      --json <json>    Directory to save JSON formatted results to. This or --csv required unless --de or --body is
                       specified
      --jsonf <jsonf>  File name to save JSON formatted results to. When present, overrides default name
      --csv <csv>      Directory to save CSV formatted results to. This or --json required unless --de or --body is
                       specified
      --csvf <csvf>    File name to save CSV formatted results to. When present, overrides default name
      --body <body>    Directory to save bodyfile formatted results to. --bdl is also required when using this option
      --bodyf <bodyf>  File name to save body formatted results to. When present, overrides default name
      --bdl <bdl>      Drive letter (C, D, etc.) to use with bodyfile. Only the drive letter itself should be provided
      --blf            When true, use LF vs CRLF for newlines [default: False]
      --dd <dd>        Directory to save exported FILE record. --do is also required when using this option
      --do <do>        Offset of the FILE record to dump as decimal or hex. Ex: 5120 or 0x1400 Use --de or --debug to see
                       offsets
      --de <de>        Dump full details for entry/sequence #. Format is 'Entry' or 'Entry-Seq' as decimal or hex. Example:
                       5, 624-5 or 0x270-0x5.
      --fls            When true, displays contents of directory specified by --de. Ignored when --de points to a file
                       [default: False]
      --ds <ds>        Dump full details for Security Id as decimal or hex. Example: 624 or 0x270
      --dt <dt>        The custom date/time format to use when displaying time stamps. See https://goo.gl/CNVq0k for
                       options [default: yyyy-MM-dd HH:mm:ss.fffffff]
      --sn             Include DOS file name types [default: False]
      --fl             Generate condensed file listing. Requires --csv [default: False]
      --at             When true, include all timestamps from 0x30 attribute vs only when they differ from 0x10 [default:
                       False]
      --rs             When true, recover slack space from FILE records when processing MFT files. This option has no
                       effect for $I30 files [default: False]
      --vss            Process all Volume Shadow Copies that exist on drive specified by -f [default: False]
      --dedupe         Deduplicate -f & VSCs based on SHA-1. First file found wins [default: False]
      --debug          Show debug information during processing [default: False]
      --trace          Show trace information during processing [default: False]
      --version        Show version information
      -?, -h, --help   Show help and usage information
    
    
    -f is required. Exiting
    

## Change log

Coming soon!

