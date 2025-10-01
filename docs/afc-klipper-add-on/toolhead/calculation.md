# Toolhead Variable Calculation

This document describes how to calculate the toolhead variable in the AFC Klipper add-on. These variables are
crucial for ensuring that the toolhead operates correctly with the AFC system.


!!!note 
    The following documentation pertains to the use of a toolhead cutter primarily. While some of the information
    may be applicable if you are using tip-forming, our official recommendation is to use a toolhead
    cutter.

These primarily consist of 2 variables:

- `tool_stn`: The distance from the toolhead sensor (typically the pre-extruder sensor) or extruder gears (if using ramming) to the top of any filament
   remaining in the hotend. 
- `tool_stn_unload`: The distance the toolhead needs to move to unload the filament.

### `tool_stn` Calculation

The `tool_stn` variable is the distance from the toolhead sensor/top of extruder gears(ramming) to the top of any filament remaining in the hotend.
This takes into account the amount of filament that is left in the hotend after the pushback and retract operations of
the cut macro have been performed. 

The easiest way to calculate this value is to start with a recommended value based on your toolhead/extruder combination.
A table of common values is provided [here](../../boxturtle/initial_startup/06-hotend-values.md).

!!!note 
    It may be easier to temporarily disable the `AFC_POOP` macro while you are adjusting the `tool_stn` value, as this
    will provide an easier way to observe the filament extruding from the hotend. Run `AFC_TOGGLE_MACRO POOP=0` to 
    disable the macro, and `AFC_TOGGLE_MACRO POOP=1` to re-enable it after you have finished adjusting the value.

Once you have a recommended value, fine-tune it by performing the following steps:

1. Unload the filament completely from the toolhead. This can be done by using the `TOOL_UNLOAD LANE=<lane>` command in
   the Klipper console.  
2. Load the filament into the toolhead using a toolchange command, such as `T1`.  
3. Observe the filament to see if it is extruding out of the hotend. If an excessive amount of filament extrudes out, 
   you will need to decrease the `tool_stn` value. If no filament extrudes, you will need to increase the `tool_stn` value.  
4. Repeat steps 1, 2 and 3 until you find a value that works for your toolhead/extruder combination.  

Use the following command to adjust the `tool_stn` value without having to restart the system:

```
UPDATE_TOOLHEAD_SENSORS EXTRUDER=<extruder> TOOL_STN=<length>
```

Once you have found a value that works, save it to the configuration with the following command:

```
SAVE_EXTRUDER_VALUES EXTRUDER=<extruder>
```

!!!note
    Don't forget to re-enable the `AFC_POOP` macro by running `AFC_TOGGLE_MACRO POOP=1` after you have finished
    adjusting the `tool_stn` value if you disabled it earlier. 

### `tool_stn_unload` Calculation

The `tool_stn_unload` variable is the distance that the filament moves when the toolhead is unloaded, after the cut macro
is executed and any pushback and retract operations have been performed. If you have a toolhead with the cutter
above the extruder gears, you should set this value to 0.

Like adjusting the `tool_stn` value, start with a recommended value based on your toolhead/extruder combination 
and use the following steps to fine-tune it:

1. Completely load the filament into the toolhead using a toolchange command, such as `T1`. 
2. Manually extrude filament until observe it coming out of the hotend. This will help reduce the changes of a
   jam occurring during the unload operation.
3. Manually execute the `AFC_CUT` macro to perform a cut operation.
4. Using a manual extrude command, retract the filament by a set amount, until the toolhead sensor is no longer triggered.

    For example, if your estimated `tool_stn_unload` value is `62mm`, start by issuing a manual retraction for 55mm
    and then manually retracting 1mm at a time until the toolhead sensor is no longer triggered. If using ramming retract until filament is out of toolhead extruders gears.

5. Once you have found the value that works, save it to the configuration with the following command:

```
UPDATE_TOOLHEAD_SENSORS EXTRUDER=<extruder> TOOL_STN_UNLOAD=<length>
SAVE_EXTRUDER_VALUES EXTRUDER=<extruder>
```