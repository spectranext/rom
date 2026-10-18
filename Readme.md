# Spectranext ROM

The ZX Spectrum ROM and Z80 modules for **Spectranext**, a modern network cartridge with Wi-Fi connectivity and support for internet-connected Spectrum software.

This project builds on Dylan Smith's [Spectranet firmware](https://github.com/spectrumero/spectranet). It carries the Spectrum-side changes needed to use the Spectranext controller while retaining the familiar Spectranet socket, filesystem, BASIC extension, and NMI interfaces.

- **[Main website](https://spectranext.net/)** — the cartridge, its features, and project news.
- **[Documentation portal](https://docs.spectranext.net/)** — setup, compatibility, filesystems, development guides, and API reference.

## What lives here

The ROM runs on the Spectrum's Z80 and provides networking entry points, memory paging, startup, BASIC integration, and the NMI menu. Loadable modules add filesystem commands, streams, configuration, messages, snapshot management, ESXDOS compatibility, and matchmaking presentation.

Spectranext's controller does the work that benefits from modern hardware, including Wi-Fi connectivity and access to modern internet services. This repository contains the Z80 side of that integration, including the controller command gateway; the RP2350 and Wi-Fi processor firmware are separate components.

## Differences from the original Spectranet ROM

The upstream [Spectranet ROM](https://github.com/spectrumero/spectranet/tree/master/rom) targets the original Ethernet interface. This fork adapts that foundation for Spectranext hardware and adds:

| Area | Spectranext changes |
| --- | --- |
| Controller integration | A controller command gateway for Wi-Fi setup, DNS lookup, status messages, settings, version queries, firmware update requests, and joystick status. DNS is delegated to the controller, and the ROM's local DHCP client is removed. |
| Startup and configuration | Spectranext boot status, launcher and update-channel settings, and Spectrum Next startup adjustments. |
| Files and BASIC | URL-based mounts, URL resource loading into an available module slot, permission commands, stream seeking, and parsing fixes. |
| ESXDOS and DivMMC | An ESXDOS compatibility kernel and filesystem module, dot-command support, and changes to reset, paging, traps, and tape loading for coexistence with DivMMC. |
| Snapshots and interface | Snapshot handling and menu improvements, text input/output fixes, language selection, and Spanish translations. |
| Network games | A matchmaking module integrated with socket `CONNECT`. The controller manages discovery and pairing; the Z80 module presents the interface and restores the caller's paging and display state. |
| UDP and builds | Datagram boundary and empty-payload fixes, a separate controller ROM image, and corrected regeneration of cross-page symbols. |

Compatibility with existing Spectranet software is a project goal. These ROM images target Spectranext hardware; consult the [documentation](https://docs.spectranext.net/) for supported machines, expansion compatibility, and installation instructions.

## Source layout

- `rom/` — core ROM pages, networking, controller gateway, and ESXDOS kernel.
- `modules/` — BASIC extensions, streams, configuration, snapshots, ESXDOS, and matchmaking.
- `include/` and `syslib/` — shared interfaces, system definitions, and assembly libraries.
- `installer/`, `flashtools/`, and `utils/` — installation, flash utilities, and host tools.
- `examples/` and `experiments/` — inherited examples and development material.

## Building and developing

The assembly sources use GNU binutils for Z80, GNU Make, and Perl build helpers. The Makefiles build the core ROM pages, modules, and installer components. English is the default language; `LANG=es` selects Spanish translations.

A complete Spectranext image also needs external modules, launcher resources, and the image packaging tools from the wider Spectranext build. This repository alone is not a complete cartridge firmware build. The Z80 application libraries have moved to the separate [Spectranext SDK](https://github.com/spectranext/sdk).

Start with the [documentation portal](https://docs.spectranext.net/) for development interfaces and device update instructions. The [original Spectranet project](https://github.com/spectrumero/spectranet) remains the reference for the upstream implementation and its history.
