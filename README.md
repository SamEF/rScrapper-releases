# HeimEngine // Releases

Compiled release binaries for **HeimEngine** (rScrapper), a Windows desktop app that collects and
analyses posts from Reddit, YouTube and X/Twitter.

This repository contains **only the built application**. It exists so installed copies can check for
updates anonymously; the source lives in a separate private repository. There is no issue tracker
here.

## Install

Download **`HeimEngine-win-Setup.exe`** from the [latest release](../../releases/latest) and run it.

- Installs per-user, under `%LOCALAPPDATA%\HeimEngine` — no administrator rights required.
- Self-contained: the .NET runtime is included, so nothing else needs installing for the app itself.
- Roughly 150 MB download, about 350 MB installed.

Windows SmartScreen will warn that the publisher is unrecognised, because these binaries are not
code-signed. Choose **More info → Run anyway** if you trust the source.

A portable `.zip` is also published if you would rather not install.

## Before it will actually do anything

Two prerequisites are **not** bundled and must be present, or the app will start but do nothing
useful. It tells you if either is missing.

**1. A SQL Server instance.** The default configuration expects
[SQL Server LocalDB](https://learn.microsoft.com/sql/database-engine/configure-windows/sql-server-express-localdb),
which ships with SQL Server Express or the Visual Studio "Data storage and processing" workload.
Install it and you are done — the app creates its own database and applies its schema on first
launch, with nothing to configure.

To use a different server instead, create
`%LOCALAPPDATA%\HeimEngine\appsettings.user.json`:

```json
{
  "ConnectionStrings": {
    "HeimDb": "Server=your-server;Database=HeimEngineDb;User ID=...;Password=...;Encrypt=True;"
  }
}
```

That file sits outside the installation folder, so it survives updates.

**2. The [Brave browser](https://brave.com/download/).** HeimEngine drives a real browser to load
pages; Brave is the only one supported, and there is no fallback. Without it the interface works and
your existing data is readable, but every collection run fails.

You will also need outbound HTTPS access, and your firewall must allow the browser subprocess.

## Updates

The app checks this repository for new versions in the background and downloads them silently.
An update installs the next time you start the app — you are never interrupted mid-run and never
asked to restart.

## Your data

Everything you configure or install lives in `%LOCALAPPDATA%\HeimEngine` — the database connection
string (encrypted with Windows DPAPI, readable only by your account), logs, plugins and any settings
overrides. Updates replace the application, never that folder.

## Verifying a download

Every release lists SHA-256 hashes alongside the assets. Note that the auto-updater verifies
downloads against the release manifest published here, not against a code-signing certificate.
