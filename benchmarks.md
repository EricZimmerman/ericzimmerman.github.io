# Benchmarks

## .net 4 vs .net 6 EZ Tool Benchmarks

Below is a table comparing the .net 4 and .net 6 results when running an identical command against the same evidence (mostly from Lone Wolf 2018 Scenario). None of these commands were ran at the same time for a respective tool, i.e., the command for AmcacheParser (.net 4) was ran and finished before the command for AmcacheParser (.net 6) was ran. Mind you, these benchmarks are provided as is with these specific commands. It's very likely if you run commands different from the ones below that your benchmarks may vary. 

In summary, there's improvements across the board in parsing speed with .net 6 while yielding identical results to the .net 4 that have been around for years at this point. Results were compared in file size, line counts, etc, using Timeline Explorer, Beyond Compare, and EditPad Pro. 

See for yourself below!

### Benchmark Results 

| Tool | .net 4 Time (Sec) | .net 6 Time (Sec) | % Improvement | 
|--|--|--|--|
| AmcacheParser | 1.16 | 0.893 | 29.90% |
| AppCompatCacheParser | 0.963 | 0.784 | 22.83% |
| bstrings | 21.926 | 9.797 | 123.80% | 
| EvtxECmd | 22.1782 | 17.4617 | 27.01% |
| JLECmd | 0.3109 | 0.2481 | 25.31% |
| LECmd | 0.5362 | 0.3928 | 36.51% | 
| MFTECmd | 3.2324 | 1.8764 | 72.27% |
| PECmd | 9.6571 | 6.8533 | 40.91% | 
| RBCmd | 0.0484 | 0.0218 | 122.02% |
| RecentFileCacheParser | 0.1878091 | 0.1785781 | 5.17% |
| RECmd | 440.004 | 30.815 | 1327.89% | 
| SBECmd | 1.14 | 0.83 | 37.35% | 
| SQLECmd | 4.5646 | 4.3613 | 4.66% | 
| SrumECmd | 1.677 | 1.4052 | 19.34% |
| SumECmd | 78.5192 | 63.8961 | 22.89% |
| Timeline Explorer | 21 | 15 | 40.00% |
| WxTCmd | 0.1673 | 0.1365 | 22.56% |

### Commands Used

| Tool | Command Used/Notes |
|:---:|:---:|
| AmcacheParser | `.\AmcacheParser.exe -f   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\Windows\AppCompat\Programs\Amcache.hve"   --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout" --debug --mp -i` |
| AppCompatCacheParser | `.\AppCompatCacheParser.exe -f   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\Windows\System32\config\SYSTEM"   --csv c:\temp\ --debug` |
| bstrings | `.\bstrings.exe -d   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E" -a --ls cloudy` |
| EvtxECmd | `.\EvtxECmd.exe -d   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\Windows\System32\winevt\logs"   --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E" --debug` |
| JLECmd | `.\JLECmd.exe -d   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\Users\jcloudy\AppData\Roaming\Microsoft\Windows\Recent\"   --mp --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout"` |
| LECmd | `.\LECmd.exe -d   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\Users\jcloudy\AppData\Roaming\Microsoft\Windows\Recent\"   --mp --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout"` |
| MFTECmd | `.\MFTECmd.exe -f   'D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\$MFT' --csv   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout"` |
| PECmd | `.\PECmd.exe -d   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\Windows\prefetch\" --csv   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E" --mp` |
| RBCmd | `.\RBCmd.exe -d   'D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\$Recycle.Bin\S-1-5-21-2734969515-1644526556-1039763013-1001'   --csv 'D:\KAPETrainingVM\LoneWolf2018KAPE\tout' --debug` |
| RecentFileCacheParser | `.\RecentFileCacheParser.exe -f   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\RecentFileCache.bcf"   --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\"` |
| RECmd | `.\RECmd.exe -d   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E" --sa cloudy --debug` |
| SBECmd | `.\SBECmd.exe -d   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E" --csv   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E" --debug` |
| SQLECmd | `.\SQLECmd.exe -d   D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E --hunt --csv   D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E --debug` |
| SrumECmd | `.\SrumECmd.exe -d   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E" --csv   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout" --debug` |
| SumECmd | `.\SumECmd.exe -d   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\SUM" --csv   "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\SUM" --debug` |
| Timeline Explorer | 1gb CSV generated from   EventTranscript.DB SQLECmd Query |
| WxTCmd | `.\WxTCmd.exe -f   'D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\Users\jcloudy\AppData\Local\ConnectedDevicesPlatform\b5e4e06f22924dca\ActivitiesCache.db'   --csv 'D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E'` |
