# Toolchanger Overview

!!! warning "Toolchanger support is currently in beta"
    Toolchanger support in AFC is still in active development. While functional, you may
    encounter bugs or rough edges. If you run into issues, please reach out first in the
    [Armored Turtle Discord](https://discord.gg/armoredturtle) for help and discussion. If the
    issue is confirmed, please also report it on the
    [AFC-Klipper-Add-On GitHub](https://github.com/ArmoredTurtle/AFC-Klipper-Add-On/issues)
    so it can be tracked and resolved.

AFC supports toolchanger setups, allowing AFC to manage filament loading,
unloading, and tool selection across multiple toolheads. This guide covers the three supported
lane-to-toolhead connection modes and explains how to configure each one.

In a toolchanger setup, AFC coordinates with an external toolchanger system to handle both the
physical tool swap and the filament state for each toolhead. Currently AFC natively supports
[klipper-toolchanger](https://github.com/viesturz/klipper-toolchanger) for the physical tool
swap operation but can also work alongside other non-klipper-toolchanger setups using custom
macros. Tool swap handling is planned to move into AFC directly in a future release, at which point
klipper-toolchanger and other external toolchanger systems will continue to be supported.
 
Before configuring AFC, your toolchanger system must be fully installed and verified working
independently. If using [klipper-toolchanger](https://github.com/viesturz/klipper-toolchanger),
this includes confirming that `SELECT_TOOL`, `UNSELECT_TOOL`, tool offsets, and any docking
macros are all operating correctly. If you are not using klipper-toolchanger, ensure that any
custom tool selection and deselection macros are already set up and working before introducing
AFC. AFC layers on top of an existing working toolchanger system; attempting to configure AFC
before the toolchanger is functional will make troubleshooting significantly more difficult.
 
Each toolhead in your system is represented by an `[AFC_extruder]` section. Depending on how
filament reaches that toolhead, lanes are configured in one of three modes. The examples
throughout this guide are based on an HTLF unit with the following layout:
 
- `lane1` → `extruder` (direct)
- `lane2` → `extruder1` (direct)
- `lane3` + `lane4` → `extruder2` (hub)
- `extruder3` + `extruder4` → standalone (no unit lane)  

| Mode | Description |
|---|---|
| **Direct** | A single lane connects directly to a dedicated toolhead with no hub in between. Toolhead loading is triggered manually or via a toolchange. |
| **Direct Load** | Same as direct, but AFC automatically loads filament to the toolhead when a spool is inserted into the lane. |
| **Hub** | Two or more lanes from a unit share a single toolhead via a hub. |
| **Standalone** | A toolchanger toolhead has no AFC unit/lane attached; filament is loaded manually once filament is inserted and toolhead sensor is triggered. |