[← Back to README](../README.md)

# Installation

- [Homebrew (macOS / Linux)](#homebrew-macos--linux)
- [Windows](#windows)
- [Install from source (macOS / Linux)](#install-from-source-macos--linux)
- [Download binary (all platforms)](#download-binary-all-platforms)
- [Requirements](#requirements)
- [Environment Variables](#environment-variables)
- [Windows Config Paths](#windows-config-paths)

---

## Homebrew (macOS / Linux)

```bash
brew install soqudev/soqu-mem
```

Upgrade to latest:

```bash
brew update && brew upgrade soqu-mem
```

> **Migrating from Cask?** If you installed soqu-mem before v1.0.1, it was distributed as a Cask. Uninstall first, then reinstall:
> ```bash
> brew uninstall --cask soqu-mem 2>/dev/null; brew install soqudev/soqu-mem
> ```

---

## Windows

**Option A: Install via `go install` (recommended for technical users)**

If you have Go installed, this is the cleanest and most trustworthy path — the binary is compiled on your machine from source, so no antivirus will flag it:

```powershell
go install github.com/soqudev/soqu-mem/cmd/soqu-mem@latest
# Binary goes to %GOPATH%\bin\soqu-mem.exe (typically %USERPROFILE%\go\bin\)
```

Ensure `%GOPATH%\bin` (or `%USERPROFILE%\go\bin`) is on your `PATH`.

**Option B: Build from source**

```powershell
git clone https://github.com/soqudev/soqu-mem.git
cd soqu-mem
go install ./cmd/soqu-mem
# Binary goes to %GOPATH%\bin\soqu-mem.exe (typically %USERPROFILE%\go\bin\)

# Optional: build with version stamp (otherwise `soqu-mem version` shows "dev")
$v = git describe --tags --always
go build -ldflags="-X main.version=local-$v" -o soqu-mem.exe ./cmd/soqu-mem
```

**Option C: Download the prebuilt binary**

1. Go to [GitHub Releases](https://github.com/soqudev/soqu-mem/releases)
2. Download `soqu-mem_<version>_windows_amd64.zip` (or `arm64` for ARM devices)
3. Extract `soqu-mem.exe` to a folder in your `PATH` (e.g. `C:\Users\<you>\bin\`)

```powershell
# Example: extract and add to PATH (PowerShell)
Expand-Archive soqu-mem_*_windows_amd64.zip -DestinationPath "$env:USERPROFILE\bin"
# Add to PATH permanently (run once):
[Environment]::SetEnvironmentVariable("Path", "$env:USERPROFILE\bin;" + [Environment]::GetEnvironmentVariable("Path", "User"), "User")
```

> **Antivirus false positives on prebuilt binaries**
>
> Windows Defender and other antivirus tools (ESET, Brave's built-in scanner) have flagged some
> soqu-mem prebuilt releases as malware (`Trojan:Script/Wacatac.H!ml` or similar). This is a
> **heuristic false positive**. The binary is built reproducibly from the public source code
> via GoReleaser and contains no malicious code.
>
> **Why does this happen?** Prebuilt binaries from small open-source projects are unsigned (code
> signing certificates cost hundreds of dollars per year). Many AV engines automatically flag
> unsigned executables from unknown publishers, especially recently compiled Go binaries. The
> same alert has been observed on Claude Code's own MSIX installer, which confirms this is an
> AV heuristic issue, not a code problem.
>
> **Maintainer stance:** We will not pay for a code signing certificate at this time. This is a
> distribution trust problem, not a security problem. The source code is fully auditable.
>
> **Recommended workaround:** Technical Windows users should prefer **Option A (`go install`)** or
> **Option B (build from source)**. Binaries you compile locally will not trigger AV alerts because
> they originate from your own machine.

> **Other Windows notes:**
> - Data is stored in `%USERPROFILE%\.soqu-mem\soqu-mem.db`
> - Override with `SOQU_MEM_DATA_DIR` environment variable
> - All core features work natively: CLI, MCP server, TUI, HTTP API, Git Sync
> - No WSL required for the core binary — it's a native Windows executable

---

## Install from source (macOS / Linux)

```bash
git clone https://github.com/soqudev/soqu-mem.git
cd soqu-mem
go install ./cmd/soqu-mem

# Optional: build with version stamp (otherwise `soqu-mem version` shows "dev")
go build -ldflags="-X main.version=local-$(git describe --tags --always)" -o soqu-mem ./cmd/soqu-mem
```

---

## Download binary (all platforms)

Grab the latest release for your platform from [GitHub Releases](https://github.com/soqudev/soqu-mem/releases).

| Platform | File |
|----------|------|
| macOS (Apple Silicon) | `soqu-mem_<version>_darwin_arm64.tar.gz` |
| macOS (Intel) | `soqu-mem_<version>_darwin_amd64.tar.gz` |
| Linux (x86_64) | `soqu-mem_<version>_linux_amd64.tar.gz` |
| Linux (ARM64) | `soqu-mem_<version>_linux_arm64.tar.gz` |
| Windows (x86_64) | `soqu-mem_<version>_windows_amd64.zip` |
| Windows (ARM64) | `soqu-mem_<version>_windows_arm64.zip` |

---

## Requirements

- **Go 1.25+** to build from source (not needed if installing via Homebrew or downloading a binary)
- That's it. No runtime dependencies.

The binary includes SQLite (via [modernc.org/sqlite](https://pkg.go.dev/modernc.org/sqlite) — pure Go, no CGO). Works natively on **macOS**, **Linux**, and **Windows** (x86_64 and ARM64).

---

## Environment Variables

| Variable | Description | Default |
|---|---|---|
| `SOQU_MEM_DATA_DIR` | Data directory | `~/.soqu-mem` (Windows: `%USERPROFILE%\.soqu-mem`) |
| `SOQU_MEM_PORT` | HTTP server port | `7437` |

---

## Windows Config Paths

When using `soqu-mem setup`, config files are written to platform-appropriate locations:

| Agent | macOS / Linux | Windows |
|-------|---------------|---------|
| OpenCode | `~/.config/opencode/` | `%APPDATA%\opencode\` |
| Gemini CLI | `~/.gemini/` | `%APPDATA%\gemini\` |
| Codex | `~/.codex/` | `%APPDATA%\codex\` |
| Claude Code | Managed by `claude` CLI | Managed by `claude` CLI |
| VS Code | `.vscode/mcp.json` (workspace) or `~/Library/Application Support/Code/User/mcp.json` (user) | `.vscode\mcp.json` (workspace) or `%APPDATA%\Code\User\mcp.json` (user) |
| Antigravity | `~/.gemini/antigravity/mcp_config.json` | `%USERPROFILE%\.gemini\antigravity\mcp_config.json` |
| Data directory | `~/.soqu-mem/` | `%USERPROFILE%\.soqu-mem\` |
