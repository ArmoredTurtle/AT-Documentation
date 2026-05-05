# [AFC_Hardware.cfg] Configuration Overview

The `AFC_Hardware.cfg` file is used to typically define options such as the AFC extruder configuration, filament 
switch bypass sensors, and buffer configurations.

This file is typically located in the `~/printer_data/config/AFC` directory and is created during the installation 
of the AFC-Klipper-Add-On.

## [AFC_extruder extruder] Section

The following options are available in the `[AFC_extruder extruder]` section of the `AFC_Hardware.cfg` file. These options 
control the configuration of the AFC system when interfacing with the extruder / toolhead.

!!! note

    These options will most likely require the most amount of configuration and tuning.

``` cfg
[AFC_extruder extruder]
pin_tool_start: mcu:pin
#    MCU defined pin for filament sensor located before (pre) the
#    extruder gears. This is used to detect the presence of filament
#    before the extruder gears. 
pin_tool_end: mcu:pin
#    MCU defined pin for filament sensor located after (post) the
#    extruder gears. This is used to detect the presence of filament
#    after the extruder gears.
tool_stn: 72
#    Default: 72
#    See documentation for details on how to calculate this value. 
#    https://armoredturtle.xyz/docs/afc-klipper-add-on/toolhead/calculation.html
tool_stn_unload: 100
#    Default: 100      
#    See documentation for details on how to calculate this value.
#    https://armoredturtle.xyz/docs/afc-klipper-add-on/toolhead/calculation.html
tool_sensor_after_extruder: 0
#    Default: 0
#    Extra distance to move in mm once pre/post sensors are clear. 
#    Useful for when only using post sensor, so this distance can 
#    be the amount to move to clear extruder gears.
tool_unload_speed: 25
#    Default: 25      
#    Unload speed in mm/s when unloading toolhead.
tool_load_speed: 25             
#    Default: 25
#    Load speed in mm/s when unloading toolhead.
buffer: <buffer_name>
#    Buffer to use for extruder, this variable can be overridden 
#    per lane.
enable_sensors_in_gui: False
#    Default: False
#    Set to True toolhead sensors switches as filament sensors in 
#    Mainsail/Fluidd gui, overrides value set in AFC.cfg.
enable_tool_runout: True
#    Default: True
#    If enabled and toolhead sensor(s) detect filament not present while printing AFC
#    will pause printing. Inputting value here overrides global value in AFC.cfg file
debounce_delay: 0
#    Default: 0
#    A period of time in seconds to debounce switches prior to detecting
#    runout. If switches are pressed and released during this delay,
#    the entire switch event is ignored.
#
#    This value overrides value set in AFC config section
```

### Temperature Settings
 
``` cfg
[AFC_extruder extruder]
deadband: 2
#    Default: 2
#    Temperature tolerance (°C) when checking if the extruder has reached
#    its target.
#    AFC considers the target reached when within ±deadband.
#    Increasing this value (e.g. 3–5) can help if the hotend oscillates
#    around the target temperature.
toolchange_temp_drop:0
#    Default: 0
#    Amount (°C) to lower this extruder’s temperature after it is deselected
#    during a toolchange. Applied immediately with no wait.
#
#    Set to 0 to disable temperature drop. Useful for faster tool swaps,
#    while non-zero values can help reduce oozing on inactive tools.
#
#    Overrides the global setting in AFC.cfg.
```

### Toolchanger Settings
 
!!! note
 
    The following options are only required for multi-toolhead toolchanger
    setups. Leave all of these unset for standard single-toolhead printers.
 
``` cfg
toolchanger_unit:
#    Default: <none>
#    Name of the AFC toolchanger this extruder belongs to.
#    Enables toolchanger features such as tool selection, swapping,
#    and AFC_SELECT_TOOL / AFC_UNSELECT_TOOL macros.
tool:
#    Default: <none>
#    Name of the tool as defined in your klipper toolchanger(KTC) configuration.
#
#    This value is used by AFC to look up the corresponding KTC
#    tool object and perform tool swaps through KTC.
map:
#    Default: <none>
#    Tool mapping label (e.g. T0, T1, etc).
#    Only needed when using a toolhead in standalone mode (not attached
#    to a unit such as AFC_BoxTurtle/NightOwl/etc) and need to override
#    KTC assigned T(n) macro.
custom_tool_swap:
#    Default: <none>
#    Custom macro to run when this tool is selected.
#    Replaces the default KTC SELECT_TOOL T<n> behavior.
#
#    Allows full control over tool pickup or activation behavior.
custom_unselect:
#    Default: <none>
#    Custom macro to run when this tool is deselected.
#    Replaces the default KTC UNSELECT_TOOL behavior.
#
#    Useful for custom docking, parking, or release routines.
```

#### LED Settings
 
!!! note
 
    All LED index values are 1-based and refer to positions within the LED
    chain defined by `led_name`. Indices assigned to `status_led_idx` and
    `nozzle_led_idx` must not overlap — AFC will raise a configuration
    error at startup if they do.
 
``` cfg
led_name:
#    Default: <none>
#    Name of the LED group used for this toolhead. Used for both status
#    indication and nozzle illumination.
#    Must match an LED defined in your Klipper config.
#
#    Example:
#      led_name: neopixel tool1_led
#
#    Required for AFC_SET_TOOLHEAD_LED and toolhead lighting control.
status_led_idx:
#    Default: <none>
#    Comma-separated LED index position(s) (1-based) within the led_name
#    chain reserved for AFC status indication. These LEDs reflect the
#    current lane/tool state (e.g. ready, loading, fault) and are excluded
#    from print lighting controlled by AFC_SET_EXTRUDER_LED macro.
#
#    Accepts a single index or a comma-separated list.
#
#    Leave unset if no LEDs are dedicated to status.
nozzle_led_idx:
#    Default: <none>
#    Comma-separated LED index position(s) (1-based) within the led_name
#    chain used for nozzle illumination. When set, AFC_SET_EXTRUDER_LED
#    macro toggles only these LEDs for print lighting instead of all non-status
#    LEDs in the chain. Leave unset to allow AFC_SET_EXTRUDER_LED macro to
#    toggle all LEDs not reserved by status_led_idx.
#
#    If not set, all LEDs except those in status_led_idx are used.
#    Must not overlap with status_led_idx.
```

## [AFC_buffer buffer_name] Section
The following options are available in the `[AFC_buffer buffer_name]` section of the `AFC_Hardware.cfg` file. These options
control the configuration of the AFC system when interfacing with the filament buffer.

``` cfg
[AFC_buffer buffer_name]
advance_pin: mcu:pin
#    MCU defined pin for advance sensor.
trailing_pin: mcu:pin
#    MCU defined pin for trailing sensor.
multiplier_high: 1.05
#    Default: 1.05
#    Factor to move more filament through the secondary extruder.
multiplier_low: 0.95
#    Default: 0.95
#    Factor to move less filament through the secondary extruder.
led_index: Buffer_Indicator:1
#    LED index for the buffer, used to control the buffer LED
#    (if present).
accel: 0
#    Default: 0 
#    Error if the buffer is not configured properly.
```

## [AFC_led Buffer_Indicator] Section

The following options are available in the `[AFC_led Buffer_Indicator]` section of the `AFC_Hardware.cfg` file. These options
control the configuration of the AFC system when interfacing with the buffer LED.

``` cfg
[AFC_led Buffer_Indicator]
pin: mcu:pin 
#    MCU defined pin for the LED.
chain_count: 1
#    Default: 1
#    Number of LEDs in the chain.
color_order: GRB
#    Default: GRB
#    Color order of the LED chain.
initial_RED: 0.0
#    Initial RED value of the LED.
initial_GREEN: 0.0
#    Initial GREEN value of the LED.
initial_BLUE: 0.0
#    Initial BLUE value of the LED.
initial_WHITE: 0.0
#    Initial WHITE value of the LED.
```