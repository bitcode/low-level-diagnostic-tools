# Low-Level Debugging Methodology

This repository serves as a comprehensive reference guide for Language Learning Models (LLMs) to understand the available diagnostic tools installed and ready for use in Windows environments. It documents various debugging utilities, their command-line interfaces, and practical usage scenarios for troubleshooting system and application issues.

## Purpose

The documentation in this repository helps LLMs understand how to:

1. Analyze and diagnose application crashes
2. Debug low-level errors in Windows environments
3. Inspect system logs and identify error sources
4. Run and analyze memory dumps
5. Examine event logs for system issues
6. Troubleshoot DLL dependencies and conflicts
7. Perform security analysis on binaries

## Available Tools

The repository documents several categories of debugging and diagnostic tools:

### Binary Analysis Tools
- **Dumpbin**: Analyze PE/COFF binaries, inspect imports/exports, and view disassembly
- **Dependency Walker**: Diagnose DLL dependency issues and missing modules
- **Sigcheck**: Verify digital signatures and check files against VirusTotal

### Debuggers
- **WinDbg**: Microsoft's powerful debugger for user-mode and kernel-mode debugging
- **LiveKd**: Kernel debugging on live systems without requiring a reboot

### System Monitoring
- **Process Explorer/Monitor**: Track process activity, file system, and registry operations
- **DebugView**: Capture debug output from applications
- **Event Log Tools**: PowerShell cmdlets for examining system and application logs

### Sysinternals Suite
- Collection of command-line utilities for advanced system troubleshooting
- Tools for process, network, file system, and registry analysis

## Command-Line Debugging Examples

### Analyzing Crash Dumps
```
windbg -z c:\crash\app.dmp
windbg -c "!analyze -v;q" -z c:\crash\app.dmp
procdump -ma -e 1 -f "" application.exe c:\dumps\app.dmp
```

### Diagnosing DLL Issues
```
depends.exe /c /of:output.txt application.exe
listdlls -v application.exe
dumpbin /IMPORTS application.exe
```

### Monitoring Process Activity
```
procmon /backingfile c:\logs\capture.pml /quiet
handle -a -p application.exe
```

### Examining Event Logs
```
Get-EventLog -LogName Application -Newest 100 | Where-Object {$_.EntryType -eq 'Error'}
Get-WinEvent -FilterHashtable @{LogName='Application'; Level=2} -MaxEvents 50
```

### Security Analysis
```
sigcheck -i -a -h application.exe
Get-AuthenticodeSignature -FilePath application.exe
```

## Usage During Development

When debugging applications during development:

1. **Crash Diagnosis**:
   - Use debuggers to attach to running processes: `windbg -pn application.exe`
   - Set up crash dump collection: `procdump -ma -e application.exe`
   - Analyze memory usage: `vmmap -p application.exe`

2. **Performance Issues**:
   - Monitor file and registry access: `procmon /filter "Process Name is application.exe"`
   - Track handle leaks: `handle -a -p application.exe`
   - Examine memory allocation: `rammap`

3. **Dependency Problems**:
   - Check DLL loading: `depends.exe application.exe`
   - Verify binary compatibility: `dumpbin /HEADERS application.exe`
   - Monitor dynamic loading: `depends.exe /c /pa:1 /pl:1 application.exe`

4. **System Integration**:
   - Check autorun entries: `autoruns -a`
   - Monitor service behavior: `psservice query application`
   - Examine network connections: `tcpview`

## Contributing

This repository is designed as a reference guide. Contributions to improve documentation, add new tools, or provide better examples are welcome.

## License

This documentation is provided for educational purposes. All tools mentioned are subject to their own licenses and terms of use.