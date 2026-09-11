# HeimEngine // Releases

Compiled release binaries for **HeimEngine** (rScrapper), a Windows desktop app that collects and
analyses posts from Reddit, YouTube and X/Twitter.

This repository contains **only the built application**. It exists so installed copies can check for
updates anonymously; the source lives in a separate private repository. There is no issue tracker
here.

## Install

Download **`HeimEngine-win-Setup.exe`** from the [latest release](../../releases/latest) and run it.

- Installs per-user, under `%LOCALAPPDATA%\HeimEngine` with no administrator rights required.
- Self-contained: the .NET runtime is included, so nothing else needs installing for the app itself.
- Roughly 150 MB download, about 350 MB installed.

Windows SmartScreen will warn that the publisher is unrecognised, because these binaries are not
code-signed. Choose **More info → Run anyway** if you trust the source.

A portable `.zip` is also published if you would rather not install.

## First launch

HeimEngine sets itself up the first time you run it. There is nothing to configure and no
connection string to type.

It needs two things that are not bundled, and installs them itself if they are missing:

- **SQL Server LocalDB**, for the database. About 60 MB, downloaded from Microsoft. This is the one
  step that asks for **administrator approval** — the installer is machine-wide, so Windows shows a
  UAC prompt. It appears once, on a machine that does not already have LocalDB.
- **The [Brave browser](https://brave.com/download/)**, which the collector drives to load pages.
  Installed per-user, so there is no prompt for this one.

Both are verified as genuinely signed by their publisher before anything is run.

The splash screen reports what it is doing and offers **Skip**. If you skip, or decline the
administrator prompt, or have no connection, the app still starts and tells you what is missing —
**Settings → Database Connection → Set up local database** finishes the job later, reusing anything
already downloaded.

Once it is up, add a data source and collection begins on its own. On an existing installation
that has been updated, collection still waits for **Start** in the title bar; turn on
*Start collecting automatically on launch* in Settings if you would rather it did not.

### Using a different database

To point HeimEngine at SQL Server or Azure SQL instead of the local database, create
`%APPDATA%\HeimEngine\appsettings.user.json`:

```json
{
  "ConnectionStrings": {
    "HeimDb": "Server=your-server;Database=HeimEngineDb;User ID=...;Password=...;Encrypt=True;"
  }
}
```

That file sits outside the installation folder, so it survives updates. You can also paste a
connection string into **Settings → Database Connection**; the local database stays pinned at the
top of the saved connections list, so you can always get back to it.

You will also need outbound HTTPS access, and your firewall must allow the browser subprocess.

## Updates

The app checks this repository for new versions in the background and downloads them silently.
An update installs the next time you start the app — you are never interrupted mid-run and never
asked to restart.

## Your data

Everything you configure or collect lives in **`%APPDATA%\HeimEngine`**: the database and its files,
your connection string (encrypted with Windows DPAPI, readable only by your Windows account), logs,
plugins and settings overrides.

That is deliberately *not* the `%LOCALAPPDATA%\HeimEngine` folder the application is installed into.
That one is replaced wholesale on every update and removed when you uninstall; your data is kept
somewhere it cannot be caught by either.

## Verifying a download

Every release lists SHA-256 hashes alongside the assets. Note that the auto-updater verifies
downloads against the release manifest published here, not against a code-signing certificate.
