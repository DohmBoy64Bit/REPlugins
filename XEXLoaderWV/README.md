# X360 XEX Loader for Ghidra by Warranty Voider

This is a Ghidra loader extension for Xbox 360 XEX files.

## SaveEditors Fork Updates

This fork carries maintained fixes and publishes ready-to-install release zips.

- SaveEditors fork release: `13.0.0` (`Bug fixes`)
- Fixed PDB enum imports so enums use their actual underlying storage size instead of always importing as 8-byte enums.
- Fixed PDB root stream page counting so PDBs with sub-page root directories parse correctly.
- Fixed CodeView `LF_ARRAY` imports so byte lengths are converted into the correct element counts.
- Fixed `.pdata` handling so imported entries become real Ghidra functions instead of label-only symbols.
- Release zips for the fork are published on the [Releases](https://github.com/SaveEditors/XEXLoaderWV/releases) page.
- The upstream patch was submitted as [zeroKilo/XEXLoaderWV#33](https://github.com/zeroKilo/XEXLoaderWV/pull/33).

## Features

- Supports PDB/XDB files.
  - In the loader import page, click Advanced.
  - Tick `Load PDB File` and `Use experimental PDB loader`, then untick `Process .pdata`.
  - Select `MSDIA` parser.
- Supports XEXP delta patches.

Requires the minimum Java version required by your Ghidra install.

[![Alt text](https://img.youtube.com/vi/coGz0f7hHTM/0.jpg)](https://www.youtube.com/watch?v=coGz0f7hHTM)

<!-- this video is outdated -->
<!-- [![Alt text](https://img.youtube.com/vi/dBoofGgraKM/0.jpg)](https://www.youtube.com/watch?v=dBoofGgraKM) -->

## Build

This extension is built using the Gradle support files that ship with Ghidra. The required Java and Gradle versions come from:

```text
<GHIDRA_INSTALL_DIR>\Ghidra\application.properties
```

Do not edit those values in Ghidra just to build this project. Instead, use a JDK and Gradle version that satisfy your installed Ghidra release.

For Ghidra `12.0.4`, the current minimums are:

```text
application.java.min=21
application.gradle.min=8.5
```

This fork was validated against Ghidra `12.0.4`, JDK `21`, and Gradle `9.3.1`.

Example:

```powershell
$env:GHIDRA_INSTALL_DIR='A:\Tools\ghidra_12.0.4_PUBLIC'
$env:JAVA_HOME='A:\Tools\jdk-21'
gradle buildExtension
```

If you are building from Eclipse or another IDE, make sure it uses a compatible JDK and Gradle version for the Ghidra version you are targeting.
