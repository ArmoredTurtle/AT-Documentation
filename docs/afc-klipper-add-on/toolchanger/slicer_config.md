# Slicer Config

## Toolchange - Pass in Next Extruder Temperature 

To better support toolchangers, we want to be able to pass the next extruder temperature to AFC so that it can set the temperature for the next tool to be loaded before the load happens. We also take the opportunity to adjust the temperature of the old extruder if it is not the same one being used.

This is particularly important when doing print-by-object, because Orca does not emit temperatures for other extruders not involved in printing the first object in a way they can be looked up in print start.

In order to use this, change filament gcode should now have the following:

!!! note
    If this is not changed, operations remain the same as they were

#### Slicer Filament Gcode Section
```
T[next_extruder] PURGE_LENGTH={flush_length} NEW_EXTRUDER_TEMP={new_filament_temp}
```

#### Print Start Macro
The following is the start macro pattern to load the initial tool and ensure it's up to temperature:
```
T{INITIAL_TOOL} NEW_EXTRUDER_TEMP={extruder_temp} # Load Initial Tool
M109 S{extruder_temp} # wait for extruder temp, in case the tool was already loaded
```