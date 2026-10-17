# 🎮 GilbX Launcher

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![.NET](https://img.shields.io/badge/.NET-C%23-512BD4)](https://dotnet.microsoft.com/)
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux-lightgrey)]()
[![Status](https://img.shields.io/badge/Status-Pre--Alpha%20%2F%20WIP-orange)]()

**GilbX Launcher** is an open-source preservation environment and native compatibility layer written in C# (.NET) designed to run **Xbox Live Indie Games (XBLIG)** natively on modern Windows and Linux systems without requiring full console emulation.

> 🌐 **Looking for the game catalog, compatibility database, or bug reporting?**  
> Visit our sister repository and community platform: **[GilbX-Web](https://github.com/GilbX-Labs/GilbX-Web)**

---

## 🎯 Technical Problem & Overview

Between 2008 and 2017, over 3,400 indie games were published on the Xbox 360 *Xbox Live Indie Games* service using Microsoft's XNA Framework. Today, running these titles on modern PCs faces four major technical barriers that **GilbX** solves natively:

1. **Zero-Friction Package Extraction (STFS / LIVE):** XBLIG backups are stored inside raw Xbox 360 STFS (`LIVE`) containers. GilbX parses and unpacks these containers on the fly—no external extraction tools required for the end user.
2. **PowerPC Big-Endian to x86/x64 Little-Endian Conversion:** Game assets (`.xnb` textures, audio, and shaders) were compiled for the Xbox 360 PowerPC architecture (`Big-Endian`) using LZX/XMemCompress streams. GilbX decompresses and flips byte endianness automatically for modern PC hardware.
3. **Extinct Xbox Live Dependencies (`GamerServices`):** XBLIG titles crash on startup when attempting to contact defunct Xbox Live servers or check trial licenses. GilbX provides an in-memory mock layer for `Microsoft.Xna.Framework.GamerServices` (simulating local profiles and forcing `Guide.IsTrialMode = false`).
4. **XNA 3.1 / 4.0 to FNA Bridging & Runtime Patching:** Translates legacy XNA calls to **FNA (SDL2)** and uses **Harmony (`Lib.Harmony`)** to inject game-specific bytecode fixes directly in RAM (such as redirecting hardcoded Xbox `d:\` storage paths) without modifying original game binaries on disk.

---

## 🏗️ Ecosystem & Decoupled Architecture

The **GilbX** project (`GilbX-Labs`) is strictly divided into two independent platforms:

* **🖥️ GilbX-Launcher (This Repository):** A 100% offline desktop launcher and execution engine. It does not fetch metadata or connect to external databases. Its sole responsibility is unpacking containers, converting assets, injecting compatibility shims, running the game, and generating standardized `crash_report.log` files if an unhandled exception occurs.
* **🌍 GilbX-Web:** The community portal combining a complete XBLIG database & collection tracker (*Backloggd-style*), a compatibility index (*ProtonDB-style*), and a ticket/request system where users attach their `crash_report.log` files to help developers verify and patch games.

### Solution Structure (`GilbX.slnx`)

```text
GilbX-Launcher/
├── GilbX.Unpacker/   # Native STFS/LIVE container extractor, LZX decompressor & Big-Endian to Little-Endian .xnb converter
├── GilbX.Core/       # FNA bridge, XNA 3.1 shims, and Microsoft.Xna.Framework.GamerServices mocking layer
└── GilbX.Launcher/   # Desktop application, SHA-256 / TitleID scanner, Harmony runtime patcher, and crash logger
```

---

## ⚖️ Legal Disclaimer

**GilbX is strictly a preservation and compatibility tool.**

This repository does not contain, distribute, or link to any copyrighted Xbox 360 system files, XBLIG game executables, or proprietary assets. Users must supply their own legally obtained game backup packages to use this software.

---

## 📄 License

This project is licensed under the GNU General Public License v3.0 (GPL-3.0) — see the LICENSE file for details.

---
