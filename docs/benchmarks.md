---
hide:
  - navigation
---

# 📊 Benchmarks

How fast the EZ Tools are, and how much faster each move made them: the 2026.10 release, measured tool by tool, and
the 2022 move from .NET Framework 4 to .NET 6.

!!! info "The .NET versions"

    - **2022**: the tools moved from .NET Framework 4 to .NET 6, which made them faster across the board.
    - **2025**: .NET 6 reached end of support (November 2024), so the tools moved to .NET 9.
    - **2026.10**: every tool now targets **.NET 10**, a long term support release that Microsoft supports until
      November 2028 ([support policy](https://dotnet.microsoft.com/en-us/platform/support/policy/dotnet-core)).

## ⚡ 2026.10: speed and memory

Beyond the move to .NET 10, the 2026.10 release went through every tool for speed and memory. These are the measured
results, before → after, on the same files and machine. Each tool's full list of changes is on the
[ChangeLog](changelog.md) page.

### ⌨️ Command line tools

| Tool | Test | Before | After | Gain |
|:--|:--|--:|--:|:--|
| EvtxECmd | 1,039 event logs | 589 s | 300 s | ⚡ about **2x** faster |
| EvtxECmd | 128 MB Security log | 16.1 s | 6.6 s | ⚡ **2.4x** faster |
| iisGeolocate | 197 MB IIS log | 11.8 s · 619 MB | 1.3 s · 112 MB | ⚡ **9x** faster, a fifth of the memory |
| MFTECmd | $MFT, 2.1M records | 10.4 s | 5.7 s | ⚡ **1.8x** faster |
| MFTECmd | 2 GB $MFT, peak memory | 5.2 GB | 4.7 GB | 🧠 about 20% less memory |
| MFTECmd `--lowmem` | 2 GB $MFT | 4.7 GB · 11.7 s | 3.3 GB · 15.8 s | 🧠 about 30% less memory, 1.4x the time |
| bstrings | 152 MB file | 1.50 s · 1.27 GB | 0.95 s · 0.81 GB | ⚡ **1.6x** faster, a third less memory |
| RECmd, rla, AmcacheParser, AppCompatCacheParser, SBECmd | 153 MB SOFTWARE hive (Registry library) | 4.5 s · 13.2 GB allocated | 2.3 s · 2.1 GB allocated | ⚡ **2x** faster, a sixth of the allocations |
| LECmd | Shortcut parsing | | | ⚡ about 27% faster, less memory |
| SBECmd | Loading a hive | | | ⚡ about 15-20% faster |

### 🖥️ GUIs

Every GUI is now one .NET single-file exe instead of being packed with Costura, so it starts much faster and uses far
less memory.

| Program | Start time | Memory at start | Other |
|:--|--:|--:|:--|
| EZViewer | 3.5 s → **0.7 s** | 484 → **149 MB** | 🧠 images are released when the next file opens (ten 8000x6000 images: about 1.1 GB → 0.1 GB) |
| Hasher | 3.7 s → **0.8 s** | 513 → **165 MB** | ⚡ 100,000 small files: 28-34 s → **3-4 s**, 206 GB → 0.3-0.4 GB allocated |
| JumpList Explorer | 4.2 s → **1.1 s** | 478 → **200 MB** | |
| MFT Explorer | 4.5 s → **1.5 s** | 439 → **209 MB** | ⚡ Expand All 30-43% faster, recursive listings 25-45% faster |
| Registry Explorer | 4.9 s → **1.7 s** | 432 → **269 MB** | |
| SDB Explorer | 3.6 s → **0.8 s** | 490 → **166 MB** | |
| ShellBags Explorer | 4.5 s → **1.9 s** | 535 → **254 MB** | ⚡ loading a hive about 15-20% faster |
| Timeline Explorer | 3.9 s → **1.0 s** | 556 → **249 MB** | ⚡ Find **8-15x** faster (1.5M rows, 21 terms: about 2.5 minutes → 10 seconds) |
| XWFIM | 3.8 s → **0.9 s** | 545 → **182 MB** | |

Timeline Explorer also holds a loaded file in about half the memory:

| File loaded in Timeline Explorer | Before | After |
|:--|--:|--:|
| MFTECmd $MFT CSV | 145 MB | **76 MB** |
| MFTECmd $LogFile CSV | 358 MB | **188 MB** |
| MFTECmd $J CSV | 102 MB | **29 MB** |
| plaso L2T CSV, 1.5M rows | 1,726 MB | **786 MB** |

## 🏁 2022: .NET Framework 4 vs .NET 6

The same command run against the same evidence (mostly the Lone Wolf 2018 scenario) with the .NET 4 and the .NET 6
build of each tool, one after the other, never at the same time. The output was identical: results were compared by
file size, line counts and content with Timeline Explorer, Beyond Compare and EditPad Pro.

!!! note

    These numbers are for these commands on this evidence. Other commands and other data will give other numbers.

| Tool | .NET 4 (s) | .NET 6 (s) | Speedup | Improvement |
|:--|--:|--:|--:|--:|
| AmcacheParser | 1.16 | 0.893 | 1.30x | 29.90% |
| AppCompatCacheParser | 0.963 | 0.784 | 1.23x | 22.83% |
| bstrings | 21.926 | 9.797 | **2.24x** | 123.80% |
| EvtxECmd | 22.1782 | 17.4617 | 1.27x | 27.01% |
| JLECmd | 0.3109 | 0.2481 | 1.25x | 25.31% |
| LECmd | 0.5362 | 0.3928 | 1.37x | 36.51% |
| MFTECmd | 3.2324 | 1.8764 | 1.72x | 72.27% |
| PECmd | 9.6571 | 6.8533 | 1.41x | 40.91% |
| RBCmd | 0.0484 | 0.0218 | **2.22x** | 122.02% |
| RecentFileCacheParser | 0.1878091 | 0.1785781 | 1.05x | 5.17% |
| RECmd | 440.004 | 30.815 | 🚀 **14.28x** | 1327.89% |
| SBECmd | 1.14 | 0.83 | 1.37x | 37.35% |
| SQLECmd | 4.5646 | 4.3613 | 1.05x | 4.66% |
| SrumECmd | 1.677 | 1.4052 | 1.19x | 19.34% |
| SumECmd | 78.5192 | 63.8961 | 1.23x | 22.89% |
| Timeline Explorer | 21 | 15 | 1.40x | 40.00% |
| WxTCmd | 0.1673 | 0.1365 | 1.23x | 22.56% |

??? example "The commands used"

    Timeline Explorer was timed loading a 1 GB CSV made by an SQLECmd query of EventTranscript.db.

    ```powershell
    # AmcacheParser
    .\AmcacheParser.exe -f "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\Windows\AppCompat\Programs\Amcache.hve" --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout" --debug --mp -i

    # AppCompatCacheParser
    .\AppCompatCacheParser.exe -f "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\Windows\System32\config\SYSTEM" --csv c:\temp\ --debug

    # bstrings
    .\bstrings.exe -d "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E" -a --ls cloudy

    # EvtxECmd
    .\EvtxECmd.exe -d "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\Windows\System32\winevt\logs" --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E" --debug

    # JLECmd
    .\JLECmd.exe -d "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\Users\jcloudy\AppData\Roaming\Microsoft\Windows\Recent\" --mp --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout"

    # LECmd
    .\LECmd.exe -d "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\Users\jcloudy\AppData\Roaming\Microsoft\Windows\Recent\" --mp --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout"

    # MFTECmd
    .\MFTECmd.exe -f 'D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\$MFT' --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout"

    # PECmd
    .\PECmd.exe -d "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\Windows\prefetch\" --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E" --mp

    # RBCmd
    .\RBCmd.exe -d 'D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\$Recycle.Bin\S-1-5-21-2734969515-1644526556-1039763013-1001' --csv 'D:\KAPETrainingVM\LoneWolf2018KAPE\tout' --debug

    # RecentFileCacheParser
    .\RecentFileCacheParser.exe -f "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\RecentFileCache.bcf" --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\"

    # RECmd
    .\RECmd.exe -d "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E" --sa cloudy --debug

    # SBECmd
    .\SBECmd.exe -d "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E" --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E" --debug

    # SQLECmd
    .\SQLECmd.exe -d D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E --hunt --csv D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E --debug

    # SrumECmd
    .\SrumECmd.exe -d "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E" --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout" --debug

    # SumECmd
    .\SumECmd.exe -d "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\SUM" --csv "D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\SUM" --debug

    # WxTCmd
    .\WxTCmd.exe -f 'D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E\Users\jcloudy\AppData\Local\ConnectedDevicesPlatform\b5e4e06f22924dca\ActivitiesCache.db' --csv 'D:\KAPETrainingVM\LoneWolf2018KAPE\tout\E'
    ```
