# Atoll VPN

A Windows desktop VPN client with a tray-based interface, built on WinUI 3 and powered by [sing-box](https://github.com/SagerNet/sing-box) as the underlying proxy engine

## Features

- **Windows 11-style Tray Flyout** the main control panel opens from the system tray, designed to feel like a native Windows 11 Quick Settings panel
- **Subscription management** import servers from Clash and SingBox compatible subscription URLs or bare server links directly
- **Favorite servers** mark individual servers as favorites for quick access
- **Multi-protocol support** VLESS and other protocols supported through sing-box configuration
- **Taskbar widget** real-time connection status embedded in the Windows taskbar
- **Connection history** live log of outbound connections, inbound connections, and DNS queries
- **Routing rules** per-domain, per-suffix, per-keyword, and per-process rules with direct (bypass VPN) or proxy actions
- **Auto-connect / auto-reconnect** configurable startup behavior and watchdog-based failure recovery
- **Built-in updater** checks GitHub releases, downloads in background, and notifies when an update is ready
- **System tray menu** quick mode toggle (TUN / Mixed), connection stats, and settings access
- **Theme-aware tray icon** automatically follows Windows Light/Dark system theme

## Requirements

- Windows 10 1809 (build 17763) or later
- .NET 10.0
- Windows App SDK 1.8

## Building

### Prerequisites

- Visual Studio 2022 with the following workloads:
  - .NET desktop development
  - Windows application development (WinUI / Windows App SDK)
- .NET 10 SDK
- Node.js (for the version generation script)

### Steps

```powershell
# Restore dependencies
dotnet restore

# Debug build
dotnet build -c Debug

# Release build
dotnet build -c Release -p:Platform=x64

# Release build for a specific branch (e.g. canary)
dotnet build -c Release -p:Platform=x64 -p:AppBranch=canary
```

Supported platforms: `x86`, `x64`, `ARM64`.

The project generates `Core\AppVersion.g.cs` before each build using `Installer\Version\index.js` and `version.json`.
Version is bumped automatically by the build: Debug always bumps `canary`, Release bumps `stable` by default.
The branch can be overridden with `-p:AppBranch=<branch>`.

The sing-box core binary (`AtollVPN Core.exe`) is expected under `Installer\bin\singbox\{platform}\` and is copied to the output directory during build.

### Packaging

Two distribution formats are produced by `Installer\Release.ps1`:

| Format | Notes |
|---|---|
| `.exe` (Inno Setup) | Recommended for end users, no certificate required |
| `.msixbundle` | Self-signed certificate, requires one-time trust of the included `.cer` file |

To build a release package, run:

```powershell
.\Installer\Release.ps1
```

The script prompts for platform, format, and branch. 

To update the sing-box core to a newer version:

```powershell
.\Installer\Singbox.ps1
```

## License

Licensed under the MIT License. See individual source files for copyright notices.

The bundled [sing-box](https://github.com/SagerNet/sing-box) binary is subject to its own license.
