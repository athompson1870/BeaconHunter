# BeaconHunter

A Windows security tool that uses **Event Tracing for Windows (ETW)** to detect and respond to C2 beacon behavior. BeaconHunter identifies processes with suspicious callback patterns (e.g., periodic network callbacks), scores them, and lets you monitor or suspend offending threads.

**Author:** Andrew Oliveau

## Features

- **Beacon detection** — Finds processes with threads in `ExecutionDelay` wait state (typical of beacon callbacks) and tracks their network activity.
- **Network beacon score** — Scores threads based on callback timing; regular intervals increase the score (beacon-like behavior).
- **ETW-based visibility** — Uses Microsoft-Windows-WinINet, Kernel-Process, Kernel-File, Kernel-Audit-API-Calls, and DNS-Client providers for network, process, file, and DNS events.
- **Monitor** — View beacon scores, IP stats, DNS queries, PID/TID history, command/process/file history.
- **Actions** — Manually suspend TIDs, auto-suspend threads above a score threshold, or protect Sysmon from remote termination.

## Requirements

- **Windows** (ETW and Win32 APIs used)
- **.NET Framework 4.5**
- **Run as Administrator** (required for ETW and thread suspension)

## Build

1. Open `BeaconHunter.sln` in Visual Studio (or use MSBuild).
2. Restore NuGet packages:
   ```bash
   nuget restore BeaconHunter.sln
   ```
3. Build the solution (Debug or Release).

Or from the solution directory:

```bash
msbuild BeaconHunter.sln /p:Configuration=Release
```

## Run

1. Run the built executable **as Administrator** (e.g. `BeaconHunter\bin\Release\BeaconHunter.exe` or `BeaconHunter\bin\Debug\BeaconHunter.exe`).
2. Use the main menu:
   - **Monitor** — View beacon score, IP stats, DNS, PID/TID history, command/terminate/file logs, or ignore specific TIDs.
   - **Action** — Manually suspend a TID, set an automatic score threshold for suspension, or view suspension logs.
3. Options (main menu): toggle System Verbose, Network Verbose, and Sysmon Protection.

Logs are written to a `Logs` folder next to the executable (e.g. `Network_Log.txt`, `Process_Log.txt`, `DNS_Log.txt`, etc.).

## Project structure

| Path | Description |
|------|-------------|
| `BeaconHunter/Program.cs` | Main entry point, ETW session, menu, scoring, and thread suspension logic |
| `BeaconHunter/Log.cs`     | Logging helpers for network, process, DNS, file, termination, and suspension events |
| `BeaconHunter/AsciiArt.cs`| Startup banner |
| `BeaconHunter/BeaconHunter.csproj` | Project file and NuGet references |

## Dependencies (NuGet)

- **Microsoft.Diagnostics.Tracing.TraceEvent** — ETW session and event parsing
- **ConsoleTables** — Formatted tables for beacon score and IP stats
