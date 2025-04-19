# WinDbg Command Summary from Crash Analysis

## cdb.exe (WinDbg CLI)

### Task: Open Crash Dump
*   `cdb -z NinjaTrader.exe_250414_223416.dmp`

## WinDbg (Debugger Commands)

### Task: Automated Analysis
*   `!analyze -v`

### Task: Display Stack Trace
*   `k`
*   `kP`

### Task: Display Exception Record
*   `.exr -1`

### Task: Display Context Record
*   `.cxr -1`

### Task: List Loaded Modules
*   `lm`