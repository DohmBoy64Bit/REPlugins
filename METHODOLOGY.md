# REPlugins Methodology

This document explains how we build, validate, package, and distribute Ghidra extensions for **Ghidra 12.1.2 PUBLIC**. It is written for AI agents (and humans) continuing this work on new plugins or Ghidra versions.

Human-facing install docs live in [README.md](README.md). Per-plugin notes live in each subfolder's `INSTALL.txt`.

---

## Goals

1. **Pin everything to one Ghidra version** — currently `12.1.2 PUBLIC` at `D:\ghidra_12.1.2_PUBLIC`.
2. **Build from source against that install's SDK** — not from prebuilt zips meant for older Ghidra releases.
3. **Ship a portable pack** — each plugin gets a subfolder with install ZIP, optional JAR, and `INSTALL.txt`.
4. **Fix upstream when needed** — if an extension does not compile on 12.1.2, patch source, fork, and publish the fork.
5. **Keep metadata honest** — extension `version` in `extension.properties` must match Ghidra or the UI shows the row in **red**.

---

## Environment (Windows reference)

| Tool | Path / version |
|------|----------------|
| Ghidra | `D:\ghidra_12.1.2_PUBLIC` |
| Java | 21 (Eclipse Adoptium) |
| Maven | `~\\tools\\apache-maven-3.9.6\\bin\\mvn.cmd` (for GhidraMCP) |
| Gradle | repo wrapper or `~\\tools\\gradle-8.12.1\\bin\\gradle.bat` |
| Python | 3.10+ (GhidraMCP bridge only) |
| User extensions | `%APPDATA%\\ghidra\\ghidra_12.1.2_PUBLIC\\Extensions\\` |
| Ghidra extension archives | `D:\\ghidra_12.1.2_PUBLIC\\Extensions\\Ghidra\\` |

Always set before Gradle builds:

```powershell
$env:GHIDRA_INSTALL_DIR = "D:\ghidra_12.1.2_PUBLIC"
$env:JAVA_HOME = "C:\Program Files\Eclipse Adoptium\jdk-21.0.11.10-hotspot"
```

Verify Ghidra version:

```powershell
Select-String application.version "D:\ghidra_12.1.2_PUBLIC\Ghidra\application.properties"
# application.version=12.1.2
```

---

## Core concept: why builds are version-specific

Ghidra extensions are **not** universal binaries. They compile against JARs inside the Ghidra install:

- `Ghidra/Framework/**/*.jar`
- `Ghidra/Features/**/*.jar`
- etc.

Pointing `GHIDRA_INSTALL_DIR` at `12.1.2` links your plugin to that SDK. The output ZIP name usually embeds the version, e.g. `ghidra_12.1.2_PUBLIC_20260620_GhidraBoy.zip`.

**You do not need to edit Ghidra itself.** You either:

- rebuild unchanged upstream source (works if APIs still match), or
- patch extension source for API breaks (common on major Ghidra bumps).

---

## Standard workflow (every new extension)

```
1. Clone upstream repo
2. Read README / build.gradle / CI for build commands and extra deps
3. Set GHIDRA_INSTALL_DIR to target Ghidra install
4. Build
5. If compile fails → diagnose API changes → patch → rebuild
6. Verify extension.properties version == Ghidra version (not red in UI)
7. Copy artifacts to REPlugins/<PluginName>/
8. Write INSTALL.txt (install steps, file types, fork URL if patched)
9. Install locally for smoke test (optional)
10. Update README.md table + commit REPlugins
11. If source was patched → fork upstream → push patches → link fork in README
```

---

## Build systems in this pack

### A. Ghidra Gradle extensions (most loaders)

Used by: XEXLoaderWV, N64LoaderWV, ghidra-xbe, GhidraBoy, GameCubeLoader, SegaMasterSystemLoader, ghidra_sega_ldr, Ghidra-SegaSaturn-Loader.

Pattern:

```powershell
cd <repo>
$env:GHIDRA_INSTALL_DIR = "D:\ghidra_12.1.2_PUBLIC"
.\gradlew.bat buildExtension    # Ghidra's standard extension task
# or
.\gradlew.bat assemble          # GhidraBoy uses custom zip task
```

Output: `dist/*.zip` or `build/distributions/*.zip`.

**Gradle applies `buildExtension.gradle` from the Ghidra install**, which reads `application.version` and sets extension metadata.

### B. GhidraMCP (Maven)

Used by: GhidraMCP only.

1. Set `pom.xml` → `<ghidra.version>12.1.2</ghidra.version>` (must match install path).
2. Install Ghidra JARs into local Maven once:

   ```powershell
   python -m tools.setup ensure-prereqs --ghidra-path "D:\ghidra_12.1.2_PUBLIC"
   python -m tools.setup build
   python -m tools.setup deploy --ghidra-path "D:\ghidra_12.1.2_PUBLIC"
   ```

Output: `target/GhidraMCP-<ver>.zip` and `target/GhidraMCP-<ver>.jar`.

---

## Extension-specific notes (lessons learned)

### XEXLoaderWV

- **Build:** `XEXLoaderWV/XEXLoaderWV` subfolder, `gradle buildExtension`.
- **Red row fix:** upstream had `version=13.0.0` (plugin release number). Change to `version=@extversion@` in `extension.properties` so Gradle stamps `12.1.2`.

### N64LoaderWV

- **Build:** repo root, `gradle buildExtension`.
- **No source changes** needed for 12.1.2 — already uses `@extversion@`.

### ghidra-xbe

- **Extra deps before build:**
  - [XbSymbolDatabase](https://github.com/Cxbx-Reloaded/XbSymbolDatabase) v3.1.160 → copy `XbSymbolDatabaseCLI.exe` to `os/win_x86_64/`
  - [xtlid.xml](https://github.com/XboxDev/xtlid/releases) → generate `XbeXtlidDb.java` via `xsltproc` or .NET XSLT (strip UTF-8 BOM if using .NET)
- **Build:** `gradle buildExtension`.
- **Optional decomp improvement:** parse [mborgerson/xbox-includes](https://github.com/mborgerson/xbox-includes) `xbox.h` via **File → Parse C Source**, then **Apply Function Datatypes**.

### GhidraBoy

- **Upstream targets Ghidra ≤ 11.4.2** — does not compile on 12.1.2 without patches.
- **Required patches (fork: [DohmBoy64Bit/GhidraBoy](https://github.com/DohmBoy64Bit/GhidraBoy)):**
  1. `Sha256.java`: `ghidra.util.HashUtilities` → `generic.hash.HashUtilities`
  2. `GameBoyLoader.java`: migrate `AbstractProgramLoader` overrides to `Loader.ImporterSettings` API (`loadProgram`, `loadProgramInto`, `getDefaultOptions` signature changes)
- **Build:** `gradlew assemble` (not full `build` — upstream tests may fail in local env; distribution zip from `assemble` is sufficient for the pack).
- **Inspect API when stuck:** `javap` against Ghidra JARs, e.g. `AbstractProgramLoader`, `Loader$ImporterSettings`.

### SnesLoader

- **Upstream archived** — [achan1989/ghidra-snes-loader](https://github.com/achan1989/ghidra-snes-loader) targets Ghidra 9.1+ and is no longer maintained.
- **Fork:** [DohmBoy64Bit/ghidra-snes-loader](https://github.com/DohmBoy64Bit/ghidra-snes-loader) with Ghidra 12.1.2 patches.
- **Required patches:**
  1. `SnesLoader.java`: migrate `AbstractProgramLoader` overrides to `ImporterSettings` API (`loadProgram(ImporterSettings)`, `loadProgramInto(Program, ImporterSettings)`, `getDefaultOptions(..., boolean mirrorFsLayout)`, `createProgram(ImporterSettings)`)
  2. `LoRomLoader.java`: add 4th `boolean` parameter to `createByteMappedBlock` call.
- **Build:** `SnesLoader/` subfolder, `gradle buildExtension -x buildModuleHelp` (pre-existing help CSS issue in upstream).
- **No wrapper:** requires system Gradle (`gradlew` pinned to 5.6.3 which is incompatible with Java 21).

### GhidraNes

- **Build:** `GhidraNes/GhidraNes` subfolder, `gradle buildExtension`.
- **No source patches** needed for Ghidra 12.1.2 — uses a compat adapter layer (`src/compat/ghidra12`) auto-detected by build script.
- **No wrapper:** requires system Gradle (no `gradlew` in repo). Use `$env:USERPROFILE\tools\gradle-8.12.1\bin\gradle.bat`.
- **`extension.properties`** already uses `@extversion@`.

### NTRGhidra

- **No source changes** needed for Ghidra 12.1.2 — already uses `@extversion@` and builds clean.
- **Build:** repo root, `gradle buildExtension`.
- **Overlay feature:** requires plugin activation in `File → Configure → Miscellaneous → NTRGhidraPlugin`.
- **Note:** `os/` platform directories are empty (README.txt placeholders only); no native code shipped.

### GhidraMCP

- **Not a loader** — HTTP MCP server plugin + Python bridge.
- **Menu location:** Project Window → **Tools → GhidraMCP** (not CodeBrowser; implements `ApplicationLevelPlugin`).
- **Verify:** `curl http://127.0.0.1:8089/check_connection`
- **Bridge:** `bridge_mcp_ghidra.py` in GhidraMCP folder; Cursor MCP config points at it.

### GameCubeLoader

- **No source changes** needed for Ghidra 12.1.2 — upstream released 1.3.1 targeting Ghidra 12.1 (May 2026).
- **Build:** repo root, `.\gradlew.bat` (wrapper 8.10.2; uses `buildExtension`).
- **ZIP naming:** uses custom scheme `GameCubeLoader-<ver>-<git>-Ghidra_<version>.zip` (not `ghidra_12.1.2_PUBLIC_*` prefix).
- **`extension.properties`** already uses `@extversion@`.
- **Dependency:** pulls `org.lz4:lz4-java:1.5.1` via Maven Central for compression support.

### SegaMasterSystemLoader

- **Upstream is WIP** — 3 commits, no releases, targets Ghidra 9.x.
- **Required patches:**
  1. `SMSLoaderLoader.java`: remove `MemoryConflictHandler` import (removed in Ghidra 12.x).
  2. `load()` method: migrate 7-param `load(ByteProvider, LoadSpec, List<Option>, Program, MemoryConflictHandler, TaskMonitor, MessageLog)` to 2-param `load(Program, Loader.ImporterSettings)`. Access provider/monitor/log via `importerSettings.provider()`, `importerSettings.monitor()`, `importerSettings.log()`.
  3. `getDefaultOptions()`: add 5th `boolean optionsOnly` param.
  4. `validateOptions()`: add `Program program` param.
- **Build:** repo root, no wrapper; requires system Gradle (`$env:USERPROFILE\tools\gradle-8.12.1\bin\gradle.bat buildExtension`).
- **`extension.properties`** already uses `@extversion@`.
- **Fork:** [DohmBoy64Bit/Ghidra-SegaMasterSystem-Loader](https://github.com/DohmBoy64Bit/Ghidra-SegaMasterSystem-Loader) with Ghidra 12.1.2 patches.

### ghidra_sega_ldr

- **Upstream targets Ghidra 11.0.1** (v2.3, Mar 2024) — needs ImporterSettings migration.
- **Required patches:**
  1. `SegaLoader.java`: migrate `load(ByteProvider, LoadSpec, List<Option>, Program, TaskMonitor, MessageLog)` to `load(Program, Loader.ImporterSettings)`. Access provider/loadSpec/monitor/log via importerSettings.
- **Build:** repo root, no wrapper; requires system Gradle.
- **`extension.properties`** already uses `@extversion@`.
- **Fork:** [DohmBoy64Bit/ghidra_sega_ldr](https://github.com/DohmBoy64Bit/ghidra_sega_ldr) with Ghidra 12.1.2 patches.
- **Features:** M68000 ROM + 32X SH-2 dual CPU, full memory map (Z80, VDP, I/O, Sega CD, 32X registers), auto-labelled vectors and header.

### Ghidra-SegaSaturn-Loader

- **No source changes** needed for Ghidra 12.1.2 — upstream released v12.0.0 targeting Ghidra 12.0 (Dec 2025, actively maintained).
- **Build:** repo root, no wrapper; requires system Gradle.
- **`extension.properties`** already uses `@extversion@`.
- **Features:** Saturn ISO, Mednafen (.mc), Yabause (.yss) save state loading with SH-2 register labelling.

---

## Packaging rules (REPlugins subfolder)

Each plugin folder should contain:

| File | Required | Purpose |
|------|----------|---------|
| `*_*.zip` | Yes | Ghidra **File → Install Extensions → Add** artifact |
| `*.jar` | Optional | Manual install / reference |
| `INSTALL.txt` | Yes | Human + AI install/run notes |
| `README.md` | Optional | Upstream readme copy |
| Extra assets | As needed | e.g. N64 signature files, GhidraMCP bridge/requirements |

Do **not** commit secrets. Use `.env.template` for GhidraMCP paths.

---

## Local installation (smoke test)

After building, install to user profile (Ghidra loads from here):

```powershell
$extBase = "$env:APPDATA\ghidra\ghidra_12.1.2_PUBLIC\Extensions"
Expand-Archive <zip> "$env:TEMP\ext-test" -Force
Copy-Item "$env:TEMP\ext-test\<ExtensionName>" "$extBase\<ExtensionName>" -Recurse -Force
```

Also copy ZIP to `D:\ghidra_12.1.2_PUBLIC\Extensions\Ghidra\` for reinstall via GUI.

Restart Ghidra. Check **File → Install Extensions** — version column should show **12.1.2** in normal text, not red.

---

## When to fork vs. use upstream

| Situation | Action |
|-----------|--------|
| Builds clean on 12.1.2, no metadata issues | Ship built ZIP; link upstream in README |
| Builds clean but wrong `extension.properties` version | Patch one line (`@extversion@`); may not need fork |
| Compile errors from Ghidra API changes | Patch source → **fork** → push → link fork in README + INSTALL.txt |
| Upstream accepts PR later | Keep fork until merged; REPlugins can switch link |

Fork workflow:

```powershell
git commit -m "Add Ghidra 12.1.2 compatibility."
gh repo fork Upstream/Repo --clone=false
git remote add fork https://github.com/<user>/Repo.git
git push -u fork main
```

Update `CHANGELOG`, `README` supported versions, and REPlugins table.

---

## Troubleshooting checklist

### Extension row is red in Install Extensions

- `extension.properties` `version` ≠ installed Ghidra version.
- Fix: use `@extversion@` at build time or hardcode `12.1.2`, rebuild, reinstall.

### Build: `GHIDRA_INSTALL_DIR is not defined`

- Export env var or pass `-PGHIDRA_INSTALL_DIR=...`.

### GhidraMCP: version mismatch on deploy

- `pom.xml` `ghidra.version` must match `--ghidra-path` / folder name.

### GhidraMCP: no Tools menu in CodeBrowser

- Expected. Use **Project Window → Tools → GhidraMCP**. Server may auto-start on port 8089.

### Nested extension folder after manual install

- ZIP extracts to `Extensions/XEXLoaderWV/XEXLoaderWV/` — flatten so `extension.properties` is at `Extensions/XEXLoaderWV/extension.properties`.

### Compile: `cannot find symbol` on Ghidra classes

- API moved between Ghidra versions. Use `javap` or IDE against 12.1.2 JARs; grep upstream Ghidra release notes; check similar fixed forks.
- Browse Ghidra API docs per version at [ghidradocs.com](https://www.ghidradocs.com/) (javadoc, API changes, help, class guides).

---

## Adding a new extension to the pack

1. Follow **Standard workflow** above.
2. Create `REPlugins/<Name>/` with ZIP + `INSTALL.txt`.
3. Add row to README.md plugin table and import formats table.
4. Add build row to README **Build info** table.
5. Update layout tree in README.
6. Commit REPlugins; push to `DohmBoy64Bit/REPlugins`.
7. If forked, ensure INSTALL.txt and README link to fork, not stale upstream.

---

## Repository map

| Repo | Role |
|------|------|
| [DohmBoy64Bit/REPlugins](https://github.com/DohmBoy64Bit/REPlugins) | Distribution pack (ZIPs + docs) |
| [DohmBoy64Bit/GhidraBoy](https://github.com/DohmBoy64Bit/GhidraBoy) | Patched fork for 12.1.2 |
| [DohmBoy64Bit/ghidra-snes-loader](https://github.com/DohmBoy64Bit/ghidra-snes-loader) | Patched fork for 12.1.2 |
| [DohmBoy64Bit/Ghidra-SegaMasterSystem-Loader](https://github.com/DohmBoy64Bit/Ghidra-SegaMasterSystem-Loader) | Patched fork for 12.1.2 |
| [DohmBoy64Bit/ghidra_sega_ldr](https://github.com/DohmBoy64Bit/ghidra_sega_ldr) | Patched fork for 12.1.2 |
| `$env:TEMP\opencode\` | Ephemeral clone workspace for new builds (per-session) |
| Upstream repos | Linked from README.md per plugin |

---

## AI agent quick reference

**Task: rebuild all extensions for Ghidra 12.1.2**

1. Confirm `D:\ghidra_12.1.2_PUBLIC` exists and `application.version=12.1.2`.
2. Rebuild each component using build system table above.
3. Refresh each `REPlugins/<plugin>/` folder.
4. Spot-check Install Extensions UI for red rows.
5. Update README dates/build table if anything changed.

**Task: port extension X to Ghidra 12.1.2**

1. Clone, set `GHIDRA_INSTALL_DIR`, build.
2. On failure, diff against Ghidra 12.1.2 SDK with `javap` / compiler errors.
3. Patch minimally (loader API, moved imports, renamed methods).
4. Verify `extension.properties` version.
5. Package, document in INSTALL.txt, fork if upstream unchanged.

**Task: verify GhidraMCP**

```powershell
curl http://127.0.0.1:8089/check_connection
curl http://127.0.0.1:8089/get_version
```

Expect `ghidra_version: 12.1.2` and plugin running.

---

## What we intentionally do not do

- Do not commit `.env` with secrets or local paths that expose private machine layout unless using templates.
- Do not force-push REPlugins unless user asks.
- Do not assume CodeBrowser has loader menus — check Project Window vs CodeBrowser per plugin type.
- Do not skip version metadata fixes — red extensions confuse users and break deploy scripts that check version parity.

---

*Last updated: 2026-06-20 — Ghidra 12.1.2 PUBLIC, 12 plugins in pack.*
