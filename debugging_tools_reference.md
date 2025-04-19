# Low-Level Debugging Tools Reference

---

## Dumpbin

**Brief Description:** A command-line utility that displays information about COFF binary files (executables, object files, libraries). Useful in low-level debugging for inspecting imports/exports, symbols, section headers, and disassembly.

**Key Features/Use Cases:**
*   Inspect exported/imported functions (`/EXPORTS`, `/IMPORTS`).
*   View symbol tables (`/SYMBOLS`).
*   Disassemble code sections (`/DISASM`).
*   Examine section headers and raw data (`/HEADERS`, `/RAWDATA`).
*   Analyze dependencies and load configuration (`/DEPENDENTS`, `/LOADCONFIG`).

**Command-Line Focus:**
Dumpbin is exclusively a command-line tool with no GUI interface, making it ideal for integration into automated debugging workflows and scripts. Its output can be redirected to files for further analysis or documentation.

**Practical Examples:**
*   `dumpbin /EXPORTS mylibrary.dll` (Show exported functions)
*   `dumpbin /IMPORTS myprogram.exe` (Show imported functions and DLLs)
*   `dumpbin /DISASM myobject.obj` (Disassemble an object file)
*   `dumpbin /HEADERS /SECTION:.text myprogram.exe` (Show headers and .text section details)
*   `dumpbin /ALL /OUT:analysis.txt myprogram.exe` (Output all information to a file)

**Official Documentation Link:**
[https://learn.microsoft.com/en-us/cpp/build/reference/dumpbin-options?view=msvc-170](https://learn.microsoft.com/en-us/cpp/build/reference/dumpbin-options?view=msvc-170)

---

## Sigcheck

**Brief Description:** A command-line utility from Sysinternals that verifies file digital signatures, checks VirusTotal status, and displays file version information. Useful in low-level debugging/analysis to verify binary integrity and check for known malware.

**Key Features/Use Cases:**
*   Verify file digital signatures and certificate chains (`-i`).
*   Check file hash against VirusTotal database (`-v`).
*   Display extended file version information (`-a`).
*   Recursively scan directories for specific file types (`-s`, `-e`).
*   Output results in CSV format for further analysis (`-c`).

**Command-Line Focus:**
Sigcheck is designed as a pure command-line tool, optimized for batch processing and integration into automated security workflows. Its parameters can be combined for comprehensive analysis, and output can be formatted for machine processing.

**Practical Examples:**
*   `sigcheck -i -a suspicious.exe` (Check signature and show all info)
*   `sigcheck -v -h suspicious.dll` (Check VirusTotal based on hash)
*   `sigcheck -e -s -c -o C:\Windows\System32` (Scan directory for unsigned executables, output to CSV)
*   `sigcheck -t -v -h *.exe` (Check all EXEs in current directory against VirusTotal, showing only those with positive hits)
*   `sigcheck -u -e C:\Program Files` (Show only unsigned files with "Verified" publisher information)

**Official Documentation Link:**
[https://learn.microsoft.com/en-us/sysinternals/downloads/sigcheck](https://learn.microsoft.com/en-us/sysinternals/downloads/sigcheck)

---

## Get-EventLog (PowerShell Module: Microsoft.PowerShell.Management)

**Brief Description:** A PowerShell cmdlet that retrieves events from classic Windows event logs (System, Application, Security). Essential for low-level debugging to correlate application crashes or system issues with OS-level events. *Note: For newer Windows versions, `Get-WinEvent` is often preferred.*

**Key Features/Use Cases:**
*   Filter events by log name, source, event ID, time range (`-LogName`, `-Source`, `-InstanceId`, `-After`, `-Before`).
*   Retrieve specific number of newest/oldest events (`-Newest`, `-Oldest`).
*   Search event messages for specific strings (`Where-Object {$_.Message -like '*error*'}`).
*   Pipe output to other PowerShell commands for advanced filtering and formatting.
*   Export results to CSV, XML, or other formats for further analysis.

**Command-Line Focus:**
As a PowerShell cmdlet, Get-EventLog is designed for command-line use and scripting. It integrates seamlessly with PowerShell's pipeline architecture, allowing complex event analysis through command chaining and output redirection.

**Practical Examples:**
*   `Get-EventLog -LogName System -Newest 100 | Where-Object {$_.EntryType -eq 'Error'}` (Get last 100 System errors)
*   `Get-EventLog -LogName Application -Source "MyApp" -After (Get-Date).AddDays(-1)` (Get events from "MyApp" in the last day)
*   `Get-EventLog -LogName Security -InstanceId 4624 -Newest 50` (Get last 50 successful logon events)
*   `Get-EventLog -LogName System -EntryType Error | Export-Csv -Path errors.csv` (Export all system errors to CSV)
*   `Get-EventLog -LogName Application | Group-Object Source | Sort-Object Count -Descending | Select-Object -First 10` (Find top 10 sources generating events)

**Official Documentation Link:**
[https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-eventlog?view=powershell-5.1](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-eventlog?view=powershell-5.1)

---

## Microsoft Defender PowerShell Module(s)

**Brief Description:** A collection of PowerShell cmdlets to manage and query Microsoft Defender Antivirus settings, scans, and detections. Useful in low-level analysis to check AV status, exclusions, and threat history related to potentially malicious binaries.

**Key Features/Use Cases:**
*   Get current threat detections (`Get-MpThreatDetection`).
*   Check AV health status and definitions (`Get-MpComputerStatus`).
*   Manage exclusions (`Get-MpPreference`, `Set-MpPreference -ExclusionPath`).
*   Initiate and control scans (`Start-MpScan`, `Remove-MpThreat`).
*   Configure real-time protection settings (`Set-MpPreference -DisableRealtimeMonitoring`).

**Command-Line Focus:**
The Microsoft Defender modules are designed for command-line administration and automation through PowerShell. This console-centric approach allows for scripted security management, integration with other debugging tools, and remote administration capabilities.

**Practical Examples:**
*   `Get-MpThreatDetection` (List recent threats detected)
*   `Get-MpComputerStatus` (Check if AV is running and up-to-date)
*   `Get-MpPreference | Select-Object ExclusionPath, ExclusionProcess` (View current exclusions)
*   `Start-MpScan -ScanType QuickScan -AsJob` (Start a quick scan as a background job)
*   `Get-MpThreatDetection | Where-Object {$_.ThreatID -eq "2147519003"} | Remove-MpThreat` (Remove specific threat by ID)
*   `Invoke-Command -ComputerName Server01 -ScriptBlock {Get-MpComputerStatus}` (Check Defender status on remote server)

**Official Documentation Link:**
[https://learn.microsoft.com/en-us/powershell/module/defender/?view=windowsserver2025-ps](https://learn.microsoft.com/en-us/powershell/module/defender/?view=windowsserver2025-ps)

---

## Dependency Walker (depends.exe)

**Brief Description:** Scans Windows modules (exe, dll, ocx, sys) and builds a hierarchical tree diagram of all dependent modules. Crucial for low-level debugging to diagnose missing DLL errors or resolve version conflicts.

**Key Features/Use Cases:**
*   Lists all implicitly and dynamically loaded DLLs.
*   Highlights missing dependencies or invalid modules.
*   Shows function imports/exports for each module.
*   Provides command-line interface for automation and batch processing.
*   Returns detailed error codes for programmatic analysis.

**Command-Line Focus:**
While Dependency Walker has a GUI interface, its command-line capabilities (depends.exe) provide powerful options for automated analysis and integration into debugging workflows. The console mode (`/c`) is particularly useful for scripting and batch processing.

**Practical Examples:**
*   `depends /c /of:output.txt myprogram.exe` (Console mode, save full output to text file)
*   `depends /c /oc:results.csv myprogram.exe` (Console mode, save results in CSV format)
*   `depends /c /pa:1 /pb /ot:log.txt myprogram.exe` (Console mode with profiling options enabled)
*   `depends /c /f:1 /u:1 myprogram.exe` (Console mode with full paths and undecorated C++ functions)

**Official Documentation Link:**
[https://www.dependencywalker.com/](https://www.dependencywalker.com/)

For detailed command-line options, refer to the `dependency-walker-cli.md` document in this repository, which contains a comprehensive list of all command-line parameters, their descriptions, usage examples, and return value codes. This document serves as the authoritative reference for Dependency Walker's command-line interface.

---

## WinDbg

**Brief Description:** A powerful debugger from Microsoft's Debugging Tools for Windows package that supports both user-mode and kernel-mode debugging. WinDbg provides comprehensive debugging capabilities through its command-line interface, making it the primary tool for advanced Windows debugging scenarios.

**Key Features/Use Cases:**
* **Kernel-Mode Debugging:** Debug Windows kernel, drivers, and system crashes (`-k` option).
* **User-Mode Debugging:** Debug applications, services, and processes (`-p`, `-pn` options).
* **Crash Dump Analysis:** Analyze memory dumps from system or application crashes (`-z` option).
* **Symbol Management:** Robust symbol handling for source-level debugging (`-y` option).
* **Extension Commands:** Extensible architecture with specialized debugging extensions (`.load`, `!` commands).
* **Scripting Support:** Automate debugging tasks with script files (`-c` option, `$<` command).

**Core Functionality:**
* **Command Execution:** Most debugging operations are performed through the command console rather than the GUI.
* **Memory Inspection:** Examine memory contents with various commands (`d`, `dt`, `dv`).
* **Breakpoint Management:** Set, clear, and manage breakpoints (`bp`, `bu`, `bl`).
* **Call Stack Analysis:** Examine call stacks and local variables (`k`, `kv`).
* **Process Control:** Control execution flow (step, continue, break) with `g`, `t`, `p` commands.
* **Remote Debugging:** Connect to remote targets (`-remote`, `-server` options).

**Practical Examples:**
* `windbg -k com:port=COM1,baud=115200` (Connect to kernel debugging target via serial port)
* `windbg -z c:\crash\memory.dmp` (Open and analyze a crash dump file)
* `windbg -pn notepad.exe` (Attach to running Notepad process)
* `windbg -c "!analyze -v;q" -z c:\crash\memory.dmp` (Run automated crash analysis and exit)
* `windbg -y "srv*c:\symbols*https://msdl.microsoft.com/download/symbols"` (Set symbol path to Microsoft symbol server)

**Command-Line Focus:**
Most WinDbg operations are performed through its command console interface rather than the GUI. The command-line options (documented in windbg-command-line-options.md) provide extensive control over the debugging session. Users should become familiar with the command syntax and debugging commands for effective use.

**Official Documentation Link:**
[https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-download-tools](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-download-tools)


---

## Sysinternals Suite

**Brief Description:** A comprehensive collection of command-line utilities for Windows system troubleshooting, diagnostics, and low-level debugging. Created by Mark Russinovich and now maintained by Microsoft, these tools provide deep insights into Windows internals and are essential for advanced debugging scenarios.

**Key Categories and Tools:**

**Process and System Monitoring:**
* **Process Explorer:** Advanced replacement for Task Manager with detailed process information, handle viewing, and DLL inspection.
* **Process Monitor:** Real-time file system, Registry, and process/thread activity monitoring.
* **ProcDump:** Command-line utility for monitoring and generating process dumps based on various triggers.
* **LiveKd:** Provides kernel debugging of a live system without requiring a reboot or second computer.

**Network Utilities:**
* **TCPView:** Displays detailed listings of all TCP and UDP connections.
* **PsPing:** Measures network performance, including latency and bandwidth.
* **PsTools Suite:** Collection of command-line tools for remote system administration and information gathering.

**File and Disk Utilities:**
* **Streams:** Reveals and manages NTFS alternate data streams.
* **SDelete:** Securely deletes files with DoD-compliant overwriting.
* **Disk2vhd:** Creates VHD versions of physical disks for virtual machine use or offline analysis.

**System Information:**
* **Autoruns:** Shows programs configured to run during system boot or user login.
* **WinObj:** Object Manager namespace viewer showing internal Windows object hierarchy.
* **Handle:** Displays open handles for any process, including files, Registry keys, and more.
* **ListDLLs:** Shows all loaded DLLs in processes, highlighting version mismatches.

**Debugging Tools:**
* **DebugView:** Monitors and displays debug output from local or remote processes.
* **NotMyFault:** Deliberately crashes or hangs systems for testing crash dump collection and analysis.
* **PsExec:** Executes processes remotely for debugging on other systems.
* **Sysmon:** System monitoring service that logs system activity to the Windows event log.

**Command-Line Focus:**
The Sysinternals Suite is primarily designed for command-line operation, with most tools offering extensive command-line parameters for automation and scripting. While some tools have GUI interfaces, they all support command-line operation for integration into debugging workflows and batch processing.

**Practical Examples:**
* `procexp /accepteula` (Launch Process Explorer with EULA auto-accepted)
* `procmon /backingfile c:\logs\capture.pml /quiet` (Start Process Monitor with silent capture to file)
* `procdump -ma -e 1 -f "" notepad.exe c:\dumps\notepad.dmp` (Create full memory dump when notepad throws an exception)
* `psexec \\remotecomputer -s cmd.exe` (Launch command prompt on remote system with SYSTEM privileges)
* `handle -a -p explorer.exe` (Show all handles opened by Explorer)
* `listdlls -v notepad.exe` (Show all DLLs loaded by Notepad with verbose information)
* `autoruns -a -s -c` (Show all autorun entries, sorted by entry location)
* `sysmon -i -h md5,sha256 -n` (Install Sysmon with specific hash algorithms and network monitoring)

**Official Documentation Link:**
[https://learn.microsoft.com/en-us/sysinternals/](https://learn.microsoft.com/en-us/sysinternals/)
