# Overview of features

This section goes over the features that can be found in Armored Turtle Automated Filament Control (AFC) Software.

## TurtleNeck Buffer Ram Sensor

AFC allows the use of the TurtleNeck Buffers as a ram sensor for detecting when filament is loaded to the toolhead
extruder. This can be used in place of a toolhead filament sensor. To learn more about this feature please
see [Buffer Ram Sensor](installation/buffer-ram-sensor.md) document.

TurtleNeck Buffer can also detect clogs, jams and feeding issues before they result in a failed print. See [buffer fault detection](installation/buffer-overview.md#buffer-fault-detection) section in buffer overview for more information.

## Bypass

By default, if a hardware sensor is not set up for a bypass, AFC will create a virtual bypass filament sensor. 
Enabling the virtual filament sensor disables AFC functionality, and the enabled state persists across reboots.

You can also enable AFC bypass with a hardware sensor by printing out a [bypass](https://github.com/ArmoredTurtle/AFC-Accessories/tree/main/AFC_Bypass) 
accessory, connecting it inline after your buffer and adding a bypass filament sensor to klipper config like below. 
Once filament is inserted into the bypass side, the switch disables AFC functionality so you can print like normal.

```
[filament_switch_sensor bypass]
switch_pin: <replace with MCU pin that switch is connected to>
pause_on_runout: False
```

When either bypass is enabled/filament detect all AFC functionality with loading to the toolhead is disabled. Calling
the `TOOL_UNLOAD` macro will call the `UNLOAD_FILAMENT` macro if it exists so that filament can still be manually unloaded
from the toolhead.

## Lower stepper current when printing

For longer prints you may want to have the ability to lower BoxTurtles steppers current as they can get hot when engaged
for a long period of time.

Enabling lower current during printing can be enabled two ways:

1. Set `global_print_current` in AFC.cfg file
2. Set `print_current` for each AFC_stepper, this will override `global_print_current` in AFC.cfg

During testing, it was found that 0.6A worked well during printing and kept the steppers warm to the touch. We 
would not suggest going lower than this or the TurtleNeck buffers may not work as intended when using BOM spec steppers.

## Enabling switches to show up in Mainsail/Fluidd GUIs

AFC has the ability to add sensors as filament switches so they show up in Mainsail/Fluidd web GUI. This can either be
enabled globally by adding/uncommenting `enable_sensors_in_gui: True` in AFC.cfg file or enabled/disabled in individual
sections in your config file. Enabling this globally is useful for debugging purposes, but setting in individual
sections will override the global setting.

AFC_buffer, AFC_extruder, AFC_hub, and AFC_stepper sections in your AFC_hardware.cfg or AFC_Turtle(n).cfg have the
ability to enable sensor by adding `enable_sensors_in_gui: True`. There is an extra config value for AFC_stepper to
allow you to either show both sensors or just prep/load sensors by using `sensor_to_show: prep` or
`sensor_to_show: load`, leaving out sensor_to_show will show both sensors.

## Tool change count

AFC has the ability to keep track of number of tool changes when doing multicolor prints. Number of toolchanges
will be pulled from files metadata stored in moonraker. AFC will keep track of tool changes and print out the 
current tool change number when a T(n) command is called from gcode. 


!!!note "Minimum Moonraker Version Required"

    Make sure moonraker version is at least v0.9.3-64 to utilize this feature.  

If you have set up your `Change filament G-code` section to use `SET_AFC_TOOLCHANGES` in your slicer please remove
the following lines:

```cfg
{ if toolchange_count == 1 }SET_AFC_TOOLCHANGES TOOLCHANGES=[total_toolchanges]{endif }
```

Also remove the following if added to your `PRINT_END` section as number of toolchanges will now automatically reset back
once print is done/canceled.

`SET_AFC_TOOLCHANGES TOOLCHANGES=0`

## Setting extruder temp

AFC has the ability to automatically set extruder temperature based on filament material type loaded or spoolman
extruder temperature if it's set.

If not using spoolman make sure the material is set for your lanes and the temperature values will be pulled from
`default_material_temps` variable in `AFC.cfg` file. This list can also be updated/added to, just make sure new entries
have a comma in between and follow current format when adding new variables.

If spoolman extruder temperature or material type is not defined AFC defaults to `min_extrude_temp` variable defined in
`[extruder]` section in `printer.cfg`

```cfg
default_material_temps: PLA:210, ABS:235, ASA:235 # Default temperature to set extruder when loading/unloading lanes.
```

If spoolman extruder temperature is defined but you wish to stick to the values in the `default_material_temps` variable
then you can set the `ignore_spoolman_material_temps` option to `true` in `AFC.cfg`

```cfg
ignore_spoolman_material_temps: True  # When True, AFC will ignore temperatures set in Spoolman and use default_material_temps instead.
```

## Loading filament to hub

For users that have a hub not located in their Box Turtle, AFC has the ability to load filament to their hub once its
inserted. This is turned on by default and this will happen even if your hub is located in your Box Turtle. This can be
disabled by setting `load_to_hub: False` in your `AFC.cfg` file. Also individual lanes can be turned on/off by setting
`load_to_hub: True/False` under `[AFC_stepper <lane_name>]` section in your config.

## Variable purge length on filament change

AFC has the ability to purge different lengths with Orca's flush volumes when doing filament changes with T(n) macros. To
use this feature update your Change Filament G-Code section in your orca slicer to the following:

`T[next_extruder] PURGE_LENGTH=[flush_length]`

Could also be added to your PRINT_START macro with a specific length, this would be ideal for if your first filament is
not currently loaded as the PURGE_LENGTH from Orca for the first change would be zero

`T{initial_tool} PURGE_LENGTH=100`

!!!warning "Important Note"

    If your first filament is not currently loaded and needs to change, `PURGE_LENGTH` will be zero and the poop
    macro will then use `variable_purge_length` from AFC_Macro_Vars.cfg file, so make sure this is set correctly for
    your printer

## Spoolman

AFC has the ability to integrate with Spoolman. This is as simple as ensuring that the following information is 
present in your `moonraker.conf` file:

```ini
[spoolman]
server: http://<ip>:<port>
sync_rate: 5
```

For example:

```ini
[spoolman]
server: http://192.168.1.184:7912
sync_rate: 5
```

!!!note Spoolman Weight Check

    When assigning a spoolID from Spoolman, either via a UI like Mainsail or Fluidd, or via a macro like `SET_SPOOL_ID`,
    AFC will perform a check to ensure that the weight of the requested spool is not 0, null, or a negative value. If it is,
    AFC will reject the spool assignment and log an error message identifying the error. For advanced use cases, this can 
    be disabled by setting `disable_weight_check: True` in your `[AFC]` section of your configuration file.

### Spoolman QR Scanner Support

Support for QR scanners is provided through [SET_NEXT_SPOOL_ID](klipper/internal/spool.md#AFC_spool.AFCSpool.cmd_SET_NEXT_SPOOL_ID). 

A USB QR code scanner implementation [afc-spool-scan](https://github.com/kekiefer/afc-spool-scan) is available to install on the klipper host.

## Direct Drive

AFC has the ability to use direct loading straight to the extruder/toolhead. There should be no hub in-between that 
lane and the extruder when this option is used. Using `direct` will disable the ability to use the automatic 
calibration functions.

To enable `direct` mode, the following line needs to be added to the `[AFC_stepper <lane_name>]` section in your 
configuration:

``` cfg
hub: direct
```

## Espooler Print Assist

AFC has the ability to activate espooler forward movement when printing to help prevent spools from
walking around and riding up wheels when they get low. This feature is enabled by default once your filament weight 
gets below 500 grams.  

The goal of this is to enable the spooler for a small amount of time so that filament on the spool is loosened up some,
then by the time your printer extrudes `delta_movement` amount(defaults to 150) the filament on your spool should just be 
getting taut before print assist activates again.  

This feature can be turned off by adding `enable_assist: False` to your `[AFC_BoxTurtle Turtle_(n)]` or `[AFC]` or per `[AFC_stepper]` config sections.
If you would like to change the weight value where print assist is activated, then add `enable_assist_weight: <new_number>` 
to your configuration, this value can be added to the same sections as `enable_assist` variable. 

The following variables described in [AFC_lane](configuration/AFC_UnitType_1.cfg.md#afc_lane-lane_name-section) section are all
the values that go into the print assist logic: `enable_assist`, `enable_assist_weight`, `timer_delay`, `delta_movement`, `spoolrate`, `spool_ratio`,
`full_weight`, `spool_outer_diameter`, `spool_inner_diameter`, `espool_rot_dist`, `max_motor_rpm`.
These values can be configured per lane (`AFC_stepper`) or per Unit (`AFC_BoxTurtle`).

With this functionality the following macros allow you to enable/disable and tweak the settings for
print assist: 

- [SET_ESPOOLER_VALUES](klipper/internal/lane.md#AFC_assist.Espooler.cmd_SET_ESPOOLER_VALUES)  
- [ENABLE_ESPOOLER_ASSIST](klipper/internal/lane.md#AFC_assist.Espooler.cmd_ENABLE_ESPOOLER_ASSIST)  
- [DISABLE_ESPOOLER_ASSIST](klipper/internal/lane.md#AFC_assist.Espooler.cmd_DISABLE_ESPOOLER_ASSIST)  
- [TEST_ESPOOLER_ASSIST](klipper/internal/lane.md#AFC_assist.Espooler.cmd_DISABLE_ESPOOLER_ASSIST)    

If the default values for print assist are unspooling too much you can start off by changing either `max_motor_rpm` or 
`spool_ratio` to decrease the time that the N20 motors are active (aka cruise_time). 

Below is the default cruise time dependent on weight when using default variables:
![image](../assets/images/print_assist_cruise_time_vs_weight.png)

Formula to calculate `cruise_time`:
```python
rps = max_motor_rpm / 60
spool_rot_s = (espool_rot_dist * (rps / spool_ratio)) / (spool_outer_diameter * PI)
w_r = ((weight / full_weight) + 1) * ((spool_outer_diameter - spool_inner_diameter) * PI)
cruise_time = delta_movement / w_r / spool_rot_s
```

## Quiet Mode

AFC has the ability to run motors at slower speed when doing loads to reduce motor noise. This is helpful for
those that may have a printer in their bedroom and would like to run multicolor prints overnight. To enable
quiet mode there is a filament switch under your filament sensor called `Quiet Mode`, once this is enabled AFC will do long moves at
a slower speed(default: 50mm/s). Quiet mode speed does not apply to PTFE calibrations and lane resets.  

Speed for quiet mode can be updated by setting `quiet_moves_speed` variable in either `[AFC]` section, or 
`[AFC_stepper <name>]` [section](configuration/AFC_UnitType_1.cfg.md#afc_stepper-lane_name-section) (adding here override setting in `[AFC]` [section](configuration/AFC.cfg.md#afc-section)).

## Tracking Toolchange Statistics

AFC tracks all toolchanges, lane loading/unloading, number of changes since last load error, total number
of cuts performed, number of cuts since blade last changed and how long N20 motors have been active if
N20 are configured in your setup.  

AFC will also start warning in console when your number of blade cuts is 1k less than the tool cut threshold letting you 
know that it's getting close to change blade. Once number of cuts exceed threshold AFC starts printing out error messages 
in the console. If blade is changed use `AFC_CHANGE_BLADE` macro to reset count and date blade was changed.  

Use the following macros to print out statistics in console, update when blade has been changed and reset
N20 active time:  
- [AFC_STATS](klipper/internal/misc.md#AFC.afc.cmd_AFC_STATS) - prints statistics to console  
- [AFC_CHANGE_BLADE](klipper/internal/misc.md#AFC.afc.cmd_AFC_CHANGE_BLADE) - run macro when blade is changed, sets date that blade was changed and resets `Total since changed` count  
- [AFC_RESET_MOTOR_TIME](klipper/internal/lane.md#AFC_assist.Espooler.cmd_AFC_RESET_MOTOR_TIME) - run macro when N20 motor has been swapped out in a lane  
- [AFC_RESET_STATS](klipper/internal/misc.md#AFC.afc.cmd_AFC_RESET_STATS) - run macro to reset extruder and lane counts

Both variables can be added/updated in `[AFC]` [section](configuration/AFC.cfg.md#afc-section) :  
- `print_short_stats`: Add/uncomment to have the statistics printout to be skinnier. Useful for those that have consoles that are skinnier (eg. Klipperscreen )  
- `tool_cut_threshold`: Defaults to 10000 cuts, update to if you want threshold to be larger. This controls when AFC prints out warning/errors when number of cuts since changed reaches/exceeds this number.

!!!note
    As of AFC-Klipper-Add-On version 1.0.34 new averaging for load times was introduced. Before AFC would not keep track of the total time when averaging, now AFC has the ability to keep track of total time and average with load counts. For AFC to calculate the new averages, the [AFC_RESET_STATS](klipper/internal/misc.md#AFC.afc.cmd_AFC_RESET_STATS) macro needs to be run like the following(this will reset your current extruder counts and load times):  
    `AFC_RESET_STATS EXTRUDER=all`.

    Once this macro is run, Moonraker's database will first be backed up just in case someone would like to restore previous stats before resetting values. This reset needs to happen for AFC to average load times correctly based on load counts.

Examples of what statistics printout looks like:  
![stats_normal](../assets/images/afc_stats_wide.png)
![stats_short](../assets/images/afc_stats_short.png)

## Button controls

!!!note "Original Design"

    The original design of this feature was created by @Trev1Ak and is available [here](https://discord.com/channels/1229586267671629945/1327060485408952340).

    This feature is now built into the AFC-Klipper-Add-On and can be enabled by following the instructions below.

    Do **NOT** use the provided Klipper config file from the original design, as it is not compatible with the AFC-Klipper-Add-On.

An optional feature that can be supported is the use of physical buttons to control various functionality of the AFC system.

If enabled, and configured properly, the following functionality can be controlled via buttons:

Press <1.2 (short-press) seconds commands as follows:

- If no lane is loaded to tool head it will load commanded lane.
- If lane loaded to tool head is other than commanded lane it will unload other lane and load commanded lane.
- If the commanded lane is loaded to the tool head, it will automatically unload the lane.

Press >1.2 (long-press) seconds commands as follows:

- If lane is loaded to tool head it will unload lane and eject spool
- If another lane is loaded to tool head it will only eject commanded lane and not interrupt other lanes.

BOM: 

- 4ea Omron B3F-1026 switches/Optional verified off brand switches Amazon https://a.co/d/hmtJkk8
- 4ea JST 3 pin male connectors for AFC Lite board
- 3 Meters of 24awg or 28awg wire (your choice)

## Detecting runouts
AFC has the ability to detect runouts or filament breakage while printing. If filament is not detected at the toolhead or hub sensors while printing, then a pause command is issued with an error message stating what happened so the error can be fixed before resuming the print.  

During printing if the PREP sensor goes low, one of two things can happen.  

- If infinite spool is not set for the lane that the PREP sensor went low on, AFC will issue a pause command so the issue can be fixed before resuming print. Note: If `unload_on_runout: True` is set in AFC config section, lane will be unloaded from toolhead after pausing.
- If infinite spool is set with [SET_MAP](klipper/internal/spool.md#AFC_spool.AFCSpool.cmd_SET_MAP) macro, then AFC will unload filament from runout lane and then load lane as specified when running SET_MAP macro. If tool loading was successful print will continue. If tool load was unsuccessful AFC will issue pause command and an error will be displayed.  

A debounce delay can also be added so that the sensor(s) need to be low for a period of time before triggering the runout logic. By default this is set to zero but can be changed by adding `debounce_delay: <delay_value>` to your AFC config which is a global value. Debounce delay can also be added in AFC_extruder, AFC_hub, AFC_stepper, and AFC_lane configs which override the global AFC setting. See configuration sections for each config for more information.

Runout detection can be turned off while printing by disabling sensor in web GUI. If PREP sensor is disabled this also disables infinite spool. The state of the switches is not persistent and will reset to enabled when Klipper is restarted.

Example of runout enabled/disabled:
![runout_enabled_disabled](../assets/images/runout_switch.png)

## TD-1 Support
AFC has the ability to grab data from TD-1 devices that are connected to your printer. More information about this and setting it up can be found under [TD-1](td1.md) section.

## Exposing Lane Data for Third-Parties
AFC will store lane data in Moonraker's database at `<ip_address>/server/database/item?namespace=lane_data` so that third-parties (like orca once support is added) can read this data and know what color, TD(if enabled), mapping, material filament, etc. is in each lane.

Endpoint returns all lanes in system in a json format like the following:
```
{
    "namespace": "lane_data",
    "key": null,
    "value": {
        "lane1": {
            "color": "#122B44",
            "td": 4.0,
            "material": "ASA",
            "bed_temp": 105,
            "nozzle_temp":245,
            "scan_time": "2025-09-14T03:13:27.189383Z",
            "lane": "1",
            "spool_id": 12345
        },
        "lane2": {
            "color": "#122B44",
            "td": 4.0,
            "material": "ASA",
            "bed_temp": 105,
            "nozzle_temp":245,
            "scan_time": "2025-09-14T03:13:27.189383Z",
            "lane": "0",
            "spool_id": 54321
        },
        "lane3": {
            "color": "",
            "material": "",
            "bed_temp": "",
            "nozzle_temp": "",
            "scan_time": "",
            "lane": "2",
            "spool_id": null
        }
    }
}
```

- Color: Current color filament loaded in lane, if filament was scanned with TD-1 then TD-1 scanned color is returned  
- TD : Transmission distance if TD-1 is connected and enabled in system  
- Material: Material from spoolman or when manually entered with [SET_MATERIAL](klipper/internal/spool.md#AFC_spool.AFCSpool.cmd_SET_MATERIAL) macro  
- Bed Temp: Bed temperature pulled from spoolman data  
- Nozzle Temp: Nozzle temperature pulled from spoolman data  
- Scan Temp: Only is populated if TD-1 is connected and enabled in system and filament was scanned  
- Lane: Current tool mapping for lane/slot. eg. T0/T1/T2/etc.  
- Spool ID: Spool ID assigned to this lane via [SET_SPOOL_ID](klipper/internal/spool.md#AFC_spool.AFCSpool.cmd_SET_SPOOL_ID) or [SET_NEXT_SPOOL_ID](klipper/internal/spool.md#AFC_spool.AFCSpool.cmd_SET_NEXT_SPOOL_ID). Value is an integer when a spool is assigned, or `null` when the lane is empty/ejected
