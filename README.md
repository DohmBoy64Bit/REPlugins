# REPlugins — Ghidra 12.1.2 Extension Pack

Pre-built Ghidra extensions for reverse engineering workflows, compiled against **Ghidra 12.1.2 PUBLIC**.

**For AI agents:** see [METHODOLOGY.md](METHODOLOGY.md) for how we build, patch, fork, and package these extensions.

| Plugin | Purpose | Source |
|--------|---------|--------|
| [GhidraMCP](GhidraMCP/) | MCP server — 249 AI/automation tools over HTTP | [bethington/ghidra-mcp](https://github.com/bethington/ghidra-mcp) |
| [XEXLoaderWV](XEXLoaderWV/) | Xbox 360 XEX/XEXP loader with PDB/XDB support | [zeroKilo/XEXLoaderWV](https://github.com/zeroKilo/XEXLoaderWV) |
| [N64LoaderWV](N64LoaderWV/) | Nintendo 64 ROM loader (`.z64`, `.n64`, `.v64`) | [zeroKilo/N64LoaderWV](https://github.com/zeroKilo/N64LoaderWV) |
| [ghidra-xbe](ghidra-xbe/) | Original Xbox XBE loader with XbSymbolDatabase | [XboxDev/ghidra-xbe](https://github.com/XboxDev/ghidra-xbe) |
| [NTRGhidra](NTRGhidra/) | Nintendo DS (NDS/DSi/NTR) loader with overlays | [pedro-javierf/NTRGhidra](https://github.com/pedro-javierf/NTRGhidra) |
| [GhidraNes](GhidraNes/) | NES (iNES 1.0) ROM loader with bank switching | [kylewlacy/GhidraNes](https://github.com/kylewlacy/GhidraNes) |
| [SnesLoader](SnesLoader/) | SNES (LoROM/HiROM) loader and disassembler | [DohmBoy64Bit/ghidra-snes-loader](https://github.com/DohmBoy64Bit/ghidra-snes-loader) (fork, Ghidra 12.1.2) |
| [GhidraBoy](GhidraBoy/) | Game Boy / SM83 loader and processor module | [DohmBoy64Bit/GhidraBoy](https://github.com/DohmBoy64Bit/GhidraBoy) (fork, Ghidra 12.1.2) |
| [GameCubeLoader](GameCubeLoader/) | Nintendo GameCube / Wii binary loader (DOL, REL) | [Cuyler36/Ghidra-GameCube-Loader](https://github.com/Cuyler36/Ghidra-GameCube-Loader) |
| [SegaMasterSystemLoader](SegaMasterSystemLoader/) | Sega Master System / Game Gear ROM loader (Z80) | [DohmBoy64Bit/Ghidra-SegaMasterSystem-Loader](https://github.com/DohmBoy64Bit/Ghidra-SegaMasterSystem-Loader) (fork, Ghidra 12.1.2) |
| [ghidra_sega_ldr](ghidra_sega_ldr/) | Sega Mega Drive / Genesis ROM loader (M68000 + 32X SH-2) | [DohmBoy64Bit/ghidra_sega_ldr](https://github.com/DohmBoy64Bit/ghidra_sega_ldr) (fork, Ghidra 12.1.2) |
| [Ghidra-SegaSaturn-Loader](Ghidra-SegaSaturn-Loader/) | Sega Saturn loader (ISO, MC, YSS save states) | [VGKintsugi/Ghidra-SegaSaturn-Loader](https://github.com/VGKintsugi/Ghidra-SegaSaturn-Loader) |
| [ghidra_sdc_ldr](ghidra_sdc_ldr/) | Sega Dreamcast RAM dump loader (SuperH4) | [DohmBoy64Bit/ghidra_sdc_ldr](https://github.com/DohmBoy64Bit/ghidra_sdc_ldr) (fork, Ghidra 12.1.2) |
| [ghidra_psx_ldr](ghidra_psx_ldr/) | Sony PlayStation PSX loader (R3000, PSYQ signatures) | [lab313ru/ghidra_psx_ldr](https://github.com/lab313ru/ghidra_psx_ldr) |
| [ghidra-emotionengine-reloaded](ghidra-emotionengine-reloaded/) | PlayStation 2 Emotion Engine loader (EE, MMI, VU0, STABS) | [chaoticgd/ghidra-emotionengine-reloaded](https://github.com/chaoticgd/ghidra-emotionengine-reloaded) |
| [Ps3GhidraScripts](Ps3GhidraScripts/) | PlayStation 3 scripts (PRX/ELF analysis, syscalls, TOC) | [clienthax/Ps3GhidraScripts](https://github.com/clienthax/Ps3GhidraScripts) |
| [ghidra-allegrex](ghidra-allegrex/) | PlayStation Portable (PSP) Allegrex CPU module (ELF/PRX, VFPU) | [kotcrab/ghidra-allegrex](https://github.com/kotcrab/ghidra-allegrex) |
| [CodeCut](CodeCut/) | Object file boundary locator (DeepCut ML + CodeCut GUI) | [JHUAPL/CodeCut](https://github.com/JHUAPL/CodeCut) |
| [ghidra-switch-loader](ghidra-switch-loader/) | Nintendo Switch loader (NSO, NRO, NCA, XCI, KIP) | [DohmBoy64Bit/Ghidra-Switch-Loader](https://github.com/DohmBoy64Bit/Ghidra-Switch-Loader) (fork, Ghidra 12.1.2) |

## Requirements

- **Ghidra 12.1.2 PUBLIC**
- **Java 21**
- **Python 3.10+** (GhidraMCP bridge only)

## Quick install

1. Open the Ghidra **Project Window**.
2. **File → Install Extensions → Add** (green **+**).
3. Install each extension ZIP:
   - `GhidraMCP/GhidraMCP-5.13.1.zip`
   - `XEXLoaderWV/ghidra_12.1.2_PUBLIC_*_XEXLoaderWV.zip`
   - `N64LoaderWV/ghidra_12.1.2_PUBLIC_*_N64LoaderWV.zip`
   - `ghidra-xbe/ghidra_12.1.2_PUBLIC_*_ghidra-xbe.zip`
   - `GhidraBoy/ghidra_12.1.2_PUBLIC_*_GhidraBoy.zip`
   - `NTRGhidra/ghidra_12.1.2_PUBLIC_*_NTRGhidra.zip`
   - `GhidraNes/ghidra_12.1.2_PUBLIC_*_GhidraNes.zip`
   - `SnesLoader/ghidra_12.1.2_PUBLIC_*_SnesLoader.zip`
   - `GameCubeLoader/GameCubeLoader-1.3.0-921504c-Ghidra_12.1.2.zip`
   - `SegaMasterSystemLoader/ghidra_12.1.2_PUBLIC_20260620_Ghidra-SegaMasterSystem-Loader.zip`
   - `ghidra_sega_ldr/ghidra_12.1.2_PUBLIC_20260620_ghidra_sega_ldr.zip`
   - `Ghidra-SegaSaturn-Loader/ghidra_12.1.2_PUBLIC_20260620_Ghidra-SegaSaturn-Loader.zip`
   - `ghidra_sdc_ldr/ghidra_12.1.2_PUBLIC_20260620_ghidra_sdc_ldr.zip`
   - `ghidra_psx_ldr/ghidra_12.1.2_PUBLIC_20260620_ghidra_psx_ldr.zip`
   - `ghidra-emotionengine-reloaded/ghidra_12.1.2_PUBLIC_20260620_ghidra-emotionengine-reloaded.zip`
   - `Ps3GhidraScripts/ghidra_12.1.2_PUBLIC_20260620_Ps3GhidraScripts.zip`
   - `ghidra-allegrex/ghidra_12.1.2_PUBLIC_20260620_ghidra-allegrex.zip`
   - `CodeCut/ghidra_12.1.2_PUBLIC_20260620_codecut-gui.zip`
   - `CodeCut/ghidra_12.1.2_PUBLIC_20260620_deepcut-ghidra.zip`
   - `ghidra-switch-loader/SwitchLoader-1.6.1-f269b17-Ghidra_12.1.2.zip`
4. **Restart Ghidra** after installing.

Each subfolder has an `INSTALL.txt` with plugin-specific steps.

### Extension version column

Ghidra marks an extension **red** when `extension.properties` `version` does not match your Ghidra install. All plugins in this pack target **12.1.2**. XEXLoaderWV was rebuilt with `version=@extversion@` so it matches (upstream had hardcoded `13.0.0`).

## GhidraMCP + Cursor

After Ghidra is running and the MCP server is up (default `http://127.0.0.1:8089`):

```json
{
  "mcpServers": {
    "ghidra": {
      "command": "python",
      "args": ["FULL_PATH_TO/GhidraMCP/bridge_mcp_ghidra.py"]
    }
  }
}
```

```bash
pip install -r GhidraMCP/requirements.txt
```

Enable the plugin in the **Project Window**: **File → Configure → Configure All Plugins → GhidraMCP**.

The **Tools → GhidraMCP** menu appears in the Project Window (not CodeBrowser).

Verify:

```bash
curl http://127.0.0.1:8089/check_connection
```

## Import formats

| Plugin | File types |
|--------|------------|
| XEXLoaderWV | `.xex`, `.xexp` (+ PDB/XDB via Advanced import) |
| N64LoaderWV | `.z64`, `.n64`, `.v64` |
| ghidra-xbe (XboxExecutableLoader) | `.xbe` (Original Xbox) |
| NTRGhidra | `.nds` (Nintendo DS ROMs), NITRO / DSi binaries |
| SnesLoader | `.sfc`, `.smc`, `.fig`, `.swc` (SNES ROMs, LoROM/HiROM) |
| GhidraNes | `.nes` (iNES 1.0 NES ROMs) |
| GhidraBoy | Game Boy ROMs / boot ROMs |
| GameCubeLoader | `.dol` (GameCube/Wii executables), `.rel` (relocatable modules), apploaders, RAM dumps |
| SegaMasterSystemLoader | `.sms` (Sega Master System), `.gg` (Game Gear) |
| ghidra_sega_ldr | `.md`, `.gen` (Sega Mega Drive / Genesis), 32X auto-detect |
| Ghidra-SegaSaturn-Loader | `.iso` (Saturn disc images), `.mc` (Mednafen save states), `.yss` (Yabause save states) |
| ghidra_sdc_ldr | `.bin` (Dreamcast RAM dumps, 16/32 MB) |
| ghidra_psx_ldr | `.ps-exe`, `.cpe`, `.exe` (PSX executables); `.obj`, `.lib` (PSYQ) |
| ghidra-emotionengine-reloaded | `.elf` (PS2 ELF with `.mdebug`), `.p2s` (PCSX2 save states) |
| Ps3GhidraScripts | `.elf`, `.prx` (PS3/PRX executables, PowerISA-Altivec BE) |
| ghidra-allegrex | `.elf`, `.prx` (PSP executables, Allegrex); `.bin` (raw) |
| CodeCut | Any binary (analysis-only plugin, not a loader) |
| ghidra-switch-loader | `.nso`, `.nro`, `.nca`, `.xci`, `.kip` (Nintendo Switch); `.elf` |

**XEX PDB import (Advanced):** enable **Load PDB File** and **Use experimental PDB loader**, disable **Process .pdata**, choose **MSDIA** parser.

**N64 symbols:** optional `example_signatures.txt` / `example_n64sym.txt` in the N64LoaderWV folder.

### Original Xbox type hints (optional)

After importing an `.xbe` with **ghidra-xbe**, you can improve decompiler output by parsing Xbox SDK-style headers into the program:

1. Clone or download [mborgerson/xbox-includes](https://github.com/mborgerson/xbox-includes).
2. In **CodeBrowser**, with your XBE program open and auto-analysis finished:
   - **File → Parse C Source…**
   - Click the green **+** and add `xbox.h` from the xbox-includes repo.
   - Click **Parse to Program**.
3. In **Data Type Manager**, right-click your program (e.g. `default.xbe`) and choose **Apply Function Datatypes**.

This adds struct/typedef/function signature hints so Ghidra's decompiler can show meaningful types instead of raw pointers and unknowns. It is optional but worth doing early on large XBEs.

## Build info

Built **2026-06-20** against `ghidra_12.1.2_PUBLIC`.

| Component | Build method |
|-----------|------------|
| GhidraMCP | Maven (`pom.xml` `ghidra.version=12.1.2`) + `python -m tools.setup` |
| XEXLoaderWV | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` |
| N64LoaderWV | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` |
| ghidra-xbe | Gradle `buildExtension` + XbSymbolDatabase + xtlid codegen |
| NTRGhidra | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` (no patches needed) |
| GhidraNes | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` (compat adapter layer, no patches needed) |
| SnesLoader | Gradle `buildExtension` (requires `-x buildModuleHelp`; patched for ImporterSettings API) |
| GhidraBoy | Gradle `assemble` after patching for Ghidra 12.1.2 loader API changes |
| GameCubeLoader | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` (no patches needed) |
| SegaMasterSystemLoader | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` (patched for ImporterSettings API; no wrapper, requires system Gradle) |
| ghidra_sega_ldr | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` (patched for ImporterSettings API; no wrapper, requires system Gradle) |
| Ghidra-SegaSaturn-Loader | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` (no patches needed; no wrapper, requires system Gradle) |
| ghidra_sdc_ldr | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` (`-x buildModuleHelp`; patched for ImporterSettings API; no wrapper, requires system Gradle) |
| ghidra_psx_ldr | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` (no patches needed; has wrapper) |
| ghidra-emotionengine-reloaded | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` (no patches needed; no wrapper, requires system Gradle) |
| Ps3GhidraScripts | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` (no patches needed; has wrapper) |
| ghidra-allegrex | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` (no patches needed; Kotlin + Sleigh; has wrapper) |
| CodeCut | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` (`-x buildModuleHelp` for codecut-gui; patched org.jdom?org.jdom2; no wrapper, requires system Gradle) |
| ghidra-switch-loader | Gradle `buildExtension` with `GHIDRA_INSTALL_DIR` (patched Guava imports + NXOAdapter; has wrapper) |

Extensions are version-specific because they compile against that Ghidra install's SDK JARs. Rebuild against a new Ghidra path when upgrading Ghidra.

## Layout

```
REPlugins/
├── README.md
├── METHODOLOGY.md
├── GhidraMCP/
│   ├── GhidraMCP-5.13.1.zip
│   ├── bridge_mcp_ghidra.py
│   ├── requirements.txt
│   └── INSTALL.txt
├── XEXLoaderWV/
│   ├── ghidra_12.1.2_PUBLIC_*_XEXLoaderWV.zip
│   └── INSTALL.txt
├── N64LoaderWV/
│   ├── ghidra_12.1.2_PUBLIC_*_N64LoaderWV.zip
│   ├── example_signatures.txt
│   └── INSTALL.txt
├── GhidraBoy/
│   ├── ghidra_12.1.2_PUBLIC_*_GhidraBoy.zip
│   └── INSTALL.txt
├── GameCubeLoader/
│   ├── GameCubeLoader-1.3.0-921504c-Ghidra_12.1.2.zip
│   └── INSTALL.txt
├── SegaMasterSystemLoader/
│   ├── ghidra_12.1.2_PUBLIC_20260620_Ghidra-SegaMasterSystem-Loader.zip
│   └── INSTALL.txt
├── ghidra_sega_ldr/
│   ├── ghidra_12.1.2_PUBLIC_20260620_ghidra_sega_ldr.zip
│   └── INSTALL.txt
├── Ghidra-SegaSaturn-Loader/
│   ├── ghidra_12.1.2_PUBLIC_20260620_Ghidra-SegaSaturn-Loader.zip
│   └── INSTALL.txt
├── ghidra_sdc_ldr/
│   ├── ghidra_12.1.2_PUBLIC_20260620_ghidra_sdc_ldr.zip
│   └── INSTALL.txt
├── ghidra_psx_ldr/
│   ├── ghidra_12.1.2_PUBLIC_20260620_ghidra_psx_ldr.zip
│   └── INSTALL.txt
├── ghidra-emotionengine-reloaded/
│   ├── ghidra_12.1.2_PUBLIC_20260620_ghidra-emotionengine-reloaded.zip
│   └── INSTALL.txt
├── Ps3GhidraScripts/
│   ├── ghidra_12.1.2_PUBLIC_20260620_Ps3GhidraScripts.zip
│   └── INSTALL.txt
├── ghidra-allegrex/
│   ├── ghidra_12.1.2_PUBLIC_20260620_ghidra-allegrex.zip
│   └── INSTALL.txt
├── CodeCut/
│   ├── ghidra_12.1.2_PUBLIC_20260620_codecut-gui.zip
│   ├── ghidra_12.1.2_PUBLIC_20260620_deepcut-ghidra.zip
│   └── INSTALL.txt
├── ghidra-switch-loader/
│   ├── SwitchLoader-1.6.1-f269b17-Ghidra_12.1.2.zip
│   └── INSTALL.txt
├── NTRGhidra/
│   ├── ghidra_12.1.2_PUBLIC_20260620_NTRGhidra.zip
│   └── INSTALL.txt
├── GhidraNes/
│   ├── ghidra_12.1.2_PUBLIC_20260620_GhidraNes.zip
│   └── INSTALL.txt
├── SnesLoader/
│   ├── ghidra_12.1.2_PUBLIC_20260620_SnesLoader.zip
│   └── INSTALL.txt
└── ghidra-xbe/
    ├── ghidra_12.1.2_PUBLIC_*_ghidra-xbe.zip
    └── INSTALL.txt
```

## License

Each extension retains its upstream license:

- GhidraMCP — Apache 2.0
- GhidraNes — Apache 2.0
- SnesLoader — MIT
- SegaMasterSystemLoader — Unlicense
- ghidra_sega_ldr — Apache 2.0
- Ghidra-SegaSaturn-Loader — Apache 2.0
- ghidra_sdc_ldr — Apache 2.0
- ghidra_psx_ldr — Apache 2.0
- ghidra-emotionengine-reloaded — Apache 2.0
- Ps3GhidraScripts — Unlicense
- ghidra-allegrex — Apache 2.0
- CodeCut — Apache 2.0
- ghidra-switch-loader — ISC
- NTRGhidra — Apache 2.0
- XEXLoaderWV / N64LoaderWV / ghidra-xbe / GhidraBoy / GameCubeLoader — see upstream repositories
