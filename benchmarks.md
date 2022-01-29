# .net 4 vs .net 6 EZ Tool Benchmarks

Below is a table comparing the .net 4 and .net 6 results when running an identical command against the same evidence (mostly from Lone Wolf 2018 Scenario). None of these commands were ran at the same time for a respective tool, i.e., the command for AmcacheParser (.net 4) was ran and finished before the command for AmcacheParser (.net 6) was ran. Mind you, these benchmarks are provided as is with these specific commands. It's very likely if you run commands different from the ones below that your benchmarks may vary. 

In summary, there's improvements across the board in parsing speed with .net 6 while yielding identical results to the .net 4 that have been around for years at this point. Results were compared in file size, line counts, etc, using Timeline Explorer, Beyond Compare, and EditPad Pro. 

See for yourself below!

| Tool | .net 4 Time (Sec) | .net 6 Time (Sec) | % Improvement | Command Used |
|--|--|--|--|--|
| AmcacheParser | 1.16 | 0.893 | 29.90% | `.\AmcacheParser.exe -f   ".\LoneWolf2018\\Amcache.hve"   --csv ".\output" --debug --mp -i` |
| AppCompatCacheParser | 0.963 | 0.784 | 22.83% | `.\AppCompatCacheParser.exe -f   ".\LoneWolf2018\SYSTEM"   --csv c:\temp\ --debug` |
| bstrings | 21.926 | 9.797 | 123.80% | `.\bstrings.exe -d   ".\LoneWolf2018\" -a --ls cloudy` |
| EvtxECmd | 22.1782 | 17.4617 | 27.01% | `.\EvtxECmd.exe -d   ".\LoneWolf2018\logs"   --csv ".\LoneWolf2018\" --debug` |
| JLECmd | 0.3109 | 0.2481 | 25.31% | `.\JLECmd.exe -d   ".\LoneWolf2018\\Recent\"   --mp --csv ".\output"` |
| LECmd | 0.5362 | 0.3928 | 36.51% | `.\LECmd.exe -d   ".\LoneWolf2018\\Recent\"   --mp --csv ".\output"` |
| MFTECmd | 3.2324 | 1.8764 | 72.27% | `.\MFTECmd.exe -f   '.\LoneWolf2018\$MFT' --csv   ".\output"` |
| PECmd | 9.6571 | 6.8533 | 40.91% | `.\PECmd.exe -d   ".\LoneWolf2018\prefetch\" --csv   ".\LoneWolf2018\" --mp` |
| RBCmd | 0.0484 | 0.0218 | 122.02% | `.\RBCmd.exe -d   '.\LoneWolf2018\$Recycle.Bin\S-1-5-21-2734969515-1644526556-1039763013-1001'   --csv '.\output' --debug` |
| RecentFileCacheParser | 0.1878091 | 0.1785781 | 5.17% | `.\RecentFileCacheParser.exe -f   ".\LoneWolf2018\RecentFileCache.bcf"   --csv ".\output\"` |
| RECmd | 440.004 | 30.815 | 1327.89% | `.\RECmd.exe -d   ".\LoneWolf2018\" --sa cloudy --debug` |
| SBECmd | 1.14 | 0.83 | 37.35% | `.\SBECmd.exe -d   ".\LoneWolf2018\" --csv   ".\LoneWolf2018\" --debug` |
| SQLECmd | 4.5646 | 4.3613 | 4.66% | `.\SQLECmd.exe -d   .\LoneWolf2018\ --hunt --csv   .\LoneWolf2018\ --debug` |
| SrumECmd | 1.677 | 1.4052 | 19.34% | `.\SrumECmd.exe -d   ".\LoneWolf2018\" --csv   ".\output" --debug` |
| SumECmd | 78.5192 | 63.8961 | 22.89% | `.\SumECmd.exe -d   ".\LoneWolf2018\SUM" --csv   ".\LoneWolf2018\SUM" --debug` |
| Timeline Explorer | 21 | 15 | 40.00% | 1gb CSV generated from   EventTranscript.DB SQLECmd Query |
| WxTCmd | 0.1673 | 0.1365 | 22.56% | `.\WxTCmd.exe -f   '.\LoneWolf2018\\ActivitiesCache.db'   --csv '.\LoneWolf2018\'` |

FYI: Paths have been shorted for readability purposes within the above Markdown table
