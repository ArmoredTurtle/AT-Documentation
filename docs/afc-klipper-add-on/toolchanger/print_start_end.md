# Useful Things to Add to Your PRINT_START/END

## PRINT_START

### Turn on Active Toolhead During Print

Add the following to your PRINT_START macro to turn on your toolhead's nozzle LEDs.
The `nozzle_led_idx` variable in your `AFC_extruder` config section needs to be set up
for the macro call to work correctly. All tools also need to be passed in from your slicer
correctly. See the [Slicer Gcodes](https://stealthchanger.com/software/slicers/#slicer-gcodes)
section on the DraftShift Design website.

```
{% for tool_nr in printer.toolchanger.tool_numbers %}
    {% set tooltemp_param = 'T' ~ tool_nr|string ~ '_TEMP' %}
    {% if tooltemp_param in params %}
        M109 T{tool_nr} S{params[tooltemp_param]} D120
        AFC_SET_TOOLHEAD_LED MAP=T{tool_nr} TURN_ON=1
    {% endif %}
{% endfor %}
```

## PRINT_END

### Dock Active Toolhead Once Done Printing

To dock the current active toolhead once done printing, define the following as a standalone
macro and then call `_DOCK_TOOL` from within your PRINT_END macro. This is only valid if
using klipper-toolchanger. Thanks to Nic335 from the DraftShift Design team for this macro.

```
[gcode_macro _DOCK_TOOL]
gcode:
    UNSELECT_TOOL RESTORE_AXIS=""
    G0 Y330 X175
    G0 Z 10
    M107  
```

### Turn off Toolhead LEDs Once Done Printing

To turn off all toolhead LEDs once done printing, add the following to your PRINT_END macro.

```
{% for tool_nr in printer.toolchanger.tool_numbers %}
    AFC_SET_TOOLHEAD_LED MAP=T{tool_nr} TURN_ON=0
{% endfor %}
```