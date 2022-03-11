# bstrings

## Command Line Options

   Description:
     bstrings version 1.5.2.0
   
     Author: Eric Zimmerman (saericzimmerman@gmail.com)
     https://github.com/EricZimmerman/bstrings
   
     Examples: bstrings.exe -f "C:\Temp\UsrClass 1.dat" --ls URL
               bstrings.exe -f "C:\Temp\someFile.txt" --lr guid
               bstrings.exe -f "C:\Temp\aBigFile.bin" --fs c:\temp\searchStrings.txt --fr c:\temp\searchRegex.txt -s
               bstrings.exe -d "C:\Temp" --mask "*.dll"
               bstrings.exe -d "C:\Temp" --ar "[\x20-\x37]"
               bstrings.exe -d "C:\Temp" --cp 10007
               bstrings.exe -d "C:\Temp" --ls test
               bstrings.exe -f "C:\Temp\someOtherFile.txt" --lr cc --sa
               bstrings.exe -f "C:\Temp\someOtherFile.txt" --lr cc --sa -m 15 -x 22
               bstrings.exe -f "C:\Temp\UsrClass 1.dat" --ls mui --sl
   
   Usage:
     bstrings [options]
   
   Options:
     -f <f>          File to search. Either this or -d is required
     -d <d>          Directory to recursively process. Either this or -f is required
     -o <o>          File to save results to
     -a              If set, look for ASCII strings. Use -a false to disable [default: True]
     -u              If set, look for Unicode strings. Use -u false to disable [default: True]
     -m <m>          Minimum string length [default: 3]
     -b <b>          Chunk size in MB. Valid range is 1 to 1024. Default is 512 [default: 512]
     -q              Quiet mode (Do not show header or total number of hits) [default: False]
     -s              Really Quiet mode (Do not display hits to console. Speeds up processing when using -o) [default:
                     False]
     -x <x>          Maximum string length. Default is unlimited [default: -1]
     -p              Display list of built in regular expressions [default: False]
     --ls <ls>       String to look for. When set, only matching strings are returned
     --lr <lr>       Regex to look for. When set, only strings matching the regex are returned
     --fs <fs>       File containing strings to look for. When set, only matching strings are returned
     --fr <fr>       Directory to save bodyfile formatted results to. --bdl is also required when using this option
     --ar <ar>       Range of characters to search for in 'Code page' strings. Specify as a range of characters in hex
                     format and enclose in quotes. Default is [\x20 -\x7E] [default: [ -~]]
     --ur <ur>       Range of characters to search for in 'Code page' strings. Specify as a range of characters in hex
                     format and enclose in quotes. Default is [\x20 -\x7E] [default: [ -~]]
     --cp <cp>       Code page to use. Default is 1252. Use the Identifier value for code pages at https://goo.gl/ig6DxW
                     [default: 1252]
     --mask <mask>   When using -d, file mask to search for. * and ? are supported. This option has no effect when using -f
     --ms <ms>       When using -d, maximum file size to process. This option has no effect when using -f [default: -1]
     --ro            When true, list the string matched by regex pattern vs string the pattern was found in (This may
                     result in duplicate strings in output. ~ denotes approx. offset) [default: False]
     --off           Show offset to hit after string, followed by the encoding (A=1252, U=Unicode) [default: False]
     --sa            Sort results alphabetically [default: False]
     --sl            When true, use LF vs CRLF for Sort results by length [default: False]
     --debug         Show debug information during processing [default: False]
     --trace         Show trace information during processing [default: False]
     --version       Show version information
     -?, -h, --help  Show help and usage information
   
   
   Either -f or -d is required. Exiting
    
## Change log

Coming soon!
